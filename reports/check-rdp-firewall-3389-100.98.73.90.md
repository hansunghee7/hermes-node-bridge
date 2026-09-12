# 보고서: 구PC 인바운드 방화벽 3389 허용 확인 (check-rdp-firewall-3389-100.98.73.90)

**작성일**: 2026-09-12  
**작성자**: 구솔라  
**대상**: 구PC (DESKTOP-202AP4O, Tailscale IP 100.98.73.90)  
**목적**: 3389 인바운드 방화벽 허용 여부를 구PC에서 직접 확인  

---

## 요약

관리자 권한 부족으로 `Get-NetFirewallRule` 기반 방화벽 규칙 직접 조회는 실패(Error 5)했으나, 대체 수단으로 3389 리스닝, TermService RUNNING, RDP-Tcp 포트/설정은 확인했다. 인바운드 방화벽에서 TCP 3389 Allow 규칙이 명시적으로 존재하는지는 관리자 권한 없이 확정하지 못했다.

---

## 확인 결과

### 성공 확인

- **3389 LISTENING**: TCP 0.0.0.0:3389 및 ::3389 리스닝 중
- **TermService**: RUNNING (WIN32_EXIT_CODE 0)
- **RDP-Tcp 레지스트리**: 포트 3389, NLA=0, SecurityLayer=0
- **방화벽 프로파일**: Domain/Private/Public 모두 활성화

### 실패/미확인

- `Get-NetFirewallRule -DisplayName '*Remote Desktop*'` → Error 5 (PermissionDenied)
- netsh advfirewall로 Remote Desktop 규칙만 필터링 → 유의미한 목록 없음 (권한 제한 추정)
- **TCP 3389 인바운드 Allow 규칙 존재 여부 미확정**

---

## 상세 명령어 및 출력

### netstat

```
TCP    0.0.0.0:3389           0.0.0.0:0              LISTENING
TCP    127.0.0.1:3389         127.0.0.1:50679        ESTABLISHED
```

### sc.exe query TermService

- STATE: 4 RUNNING
- WIN32_EXIT_CODE: 0

### RDP-Tcp 레지스트리

- PortNumber: 3389
- UserAuthentication: 0
- SecurityLayer: 0

### 방화벽 규칙 조회 실패

Get-NetFirewallRule / netsh advfirewall 모두 현재 사용자(Desktop) 권한으로는 제한적 접근만 가능.

---

## 결론

- 3389 리스닝 및 TermService는 정상 작동 중
- 방화벽 규칙이 완전히 차단한 상태는 아닐 가능성이 높음 (0.0.0.0:3389 전체 인터페이스 리스닝)
- 그러나 **인바운드 방화벽 규칙이 명시적으로 Allow인지 확정하려면 관리자 권한 PowerShell 필요**
- 신PC에서 실제 RDP 로그인을 시도해 성공 여부로 간접 확인 가능 (신솔라 몫)

---

*본 보고서는 tasks/completed/check-rdp-firewall-3389-100.98.73.90_result.md의 요약본입니다.*
