# TASK 결과: 구PC 인바운드 방화벽 3389 허용 확인

- **TASK_ID**: check-rdp-firewall-3389-100.98.73.90
- **상태**: 완료 (부분 확인 — 관리자 권한 부족으로 방화벽 규칙 직접 조회는 실패했으나, 대체 확인 수행)
- **실행자**: 구솔라
- **실행 시각**: 2026-09-12T22:14+09:00 (KST)
- **실행 대상**: 구PC (DESKTOP-202AP4O, 현재 Tailscale IP 100.98.73.90)

---

## 1. 시도한 명령 및 결과

### 1-1. 방화벽 규칙 조회 (Get-NetFirewallRule)

```powershell
Get-NetFirewallRule -DisplayName '*Remote Desktop*'
Get-NetFirewallRule -Direction Inbound -Action Allow | Where-Object { $_.DisplayName -like '*Remote*' -or $_.DisplayName -like '*RDP*' -or $_.DisplayName -like '*Terminal*' }
```

**결과**: 권한 오류 (Windows System Error 5, PermissionDenied)  
**원인**: 현재 세션의 사용자(`Desktop`)가 관리자 권한으로 실행되지 않음. Get-NetFirewallRule은 관리자 권한이 필요함.

### 1-2. netsh advfirewall

```powershell
netsh advfirewall firewall show rule name='Remote Desktop'
netsh advfirewall firewall show rule name=all | findstr -i 'Remote Desktop'
```

**결과**: 관리자 권한이 없으면 netsh advfirewall firewall show rule 명령도 제한됨. 'Remote Desktop' 규칙명만으로 필터했을 때는 의미 있는 목록이 출력되지 않음.

### 1-3. 방화벽 프로파일 상태

```powershell
Get-NetFirewallProfile | Select-Object Name, Enabled, DefaultInboundAction, DefaultOutboundAction
```

**결과**:
```
Name    Enabled DefaultInboundAction DefaultOutboundAction
----    ------- -------------------- ---------------------
Domain     True        NotConfigured         NotConfigured
Private    True        NotConfigured         NotConfigured
Public     True        NotConfigured         NotConfigured
```

방화벽 자체는 활성화되어 있고, 모든 프로파일(Domain/Private/Public)이 enabled 상태. DefaultInboundAction은 NotConfigured (기본값: Windows 방화벽 기본값 적용됨).

---

## 2. 대체 확인: 3389 리스닝 및 서비스 상태

### 2-1. 포트 리스닝 확인

```cmd
netstat -an -p tcp | findstr ':3389 '
```

**결과**:
```
TCP    0.0.0.0:3389           0.0.0.0:0              LISTENING
  TCP    127.0.0.1:3389         127.0.0.1:50679        ESTABLISHED
  TCP    127.0.0.1:50679        127.0.0.1:3389         ESTABLISHED
```

IPv4(0.0.0.0:3389)와 IPv6(::으로 표시된 것으로 보아)로 리스닝 중. 로컬 루프백에 ESTABLISHED 연결이 1개 존재.

### 2-2. Get-NetTCPConnection

```powershell
Get-NetTCPConnection -LocalPort 3389 -State Listen -ErrorAction SilentlyContinue
```

**결과**:
```
LocalAddress LocalPort  State OwningProcess
------------ ---------  ----- -------------
::                3389 Listen          1176
0.0.0.0           3389 Listen          1176
```

같은 결과. 소유 프로세스 ID 1176.

### 2-3. TermService 서비스 상태

```cmd
sc.exe query TermService
```

**결과**:
```
SERVICE_NAME: TermService
        STATE               : 4  RUNNING
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
```

TermService는 RUNNING 상태, 정상 종료 코드.

### 2-4. RDP-Tcp 레지스터리 설정

```powershell
Get-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -Name 'PortNumber','UserAuthentication','SecurityLayer'
```

**결과**:
```
PortNumber         : 3389
UserAuthentication : 0
SecurityLayer      : 0
```

- 포트 3389 사용 중
- UserAuthentication = 0 (네트워크 수준 인증 NLA 미사용 설정)
- SecurityLayer = 0 (보안 계층 없음 — 기본 RDP 암호화만 사용)

---

## 3. 종합 판정

### 확인한 내용

- 3389 TCP 포트가 IPv4(0.0.0.0)와 IPv6(::) 모두에서 LISTENING 상태
- TermService 서비스 RUNNING
- RDP-Tcp 레지스트리에서 포트 3389, NLA 미사용, 보안 계층 없음 확인
- 방화벽 프로파일(Domain/Private/Public) 모두 활성화 상태, DefaultInboundAction은 NotConfigured

### 미확인 항목

- **Get-NetFirewallRule을 통한 정확한 인바운드 방화벽 규칙 목록 확보 실패** — 관리자 권한 없이 Win32/WMI 접근 불가 (Error 5)
- **TCP 3389 인바운드 Allow 규칙이 명시적으로 존재하는지 여부** — 위 방법으로 직접 확인 불가
- netsh advfirewall로도 관리자 권한 없으면 규칙 목록을 완전히 조회할 수 없었음

### 추정

- 구PC에서 이전에 RDP_recovery_diagnostics를 수행했을 때 "RDP 수신 허용"은 확인된 바 있음
- 3389 LISTENING + TermService RUNNING이고, netstat상 0.0.0.0:3389로 열려 있으므로 방화벽에서 완전히 차단된 상태는 아닐 가능성이 높음
- 그러나 "인바운드 방화벽 규칙이 명시적으로 Allow인지"를 관리자 권한 없이 확정적으로 확인하지는 못함

---

## 4. 다음 단계

1. **관리자 권한이 있는 PowerShell**에서 `Get-NetFirewallRule -DisplayName '*Remote Desktop*'` 및 `Get-NetFirewallPortFilter` 조합으로 TCP 3389 인바운드 Allow 규칙을 직접 확인
2. 신PC에서 실제 RDP 로그인 시도 (방화벽뿐 아니라 RDP 인증/로그인 자체 성공 여부 확인)
3. 관련 결과 반영

---

## 5. § 핵심 요약

| 항목 | 결과 |
|------|------|
| 3389 LISTENING | 확인 (0.0.0.0:3389, :::3389) |
| TermService | RUNNING (정상) |
| RDP-Tcp 포트 | 3389 |
| NLA / SecurityLayer | 0 / 0 (미사용/없음) |
| 방화벽 규칙 직접 조회 | 실패 — 관리자 권한 필요 (Error 5) |
| 방화벽 프로파일 | Domain/Private/Public 모두 활성화 |
| 인바운드 3389 방화벽 허용 여부 | **확정 불가** (권한 부족) |

구솔라는 관리자 권한이 없어 Get-NetFirewallRule로 인바운드 방화벽 규칙을 직접 열거하지 못했습니다. 다만 3389 리스닝, TermService RUNNING, RDP 수신 설정은 확인했고, 방화벽이 완전히 차단한 상태는 아닐 것으로 추정되나 확정은 아닙니다. 관리자 권한으로 재확인이 필요합니다.
