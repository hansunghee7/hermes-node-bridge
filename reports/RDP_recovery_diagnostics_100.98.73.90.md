# RDP 복구 진단 보고서 (RDP_recovery_diagnostics_100.98.73.90)

- **TASK_ID**: RDP_recovery_diagnostics_100.98.73.90
- **실행자**: 구솔라 (구PC, DESKTOP-202AP4O)
- **시작 시각**: 2026-09-12 22:15:44
- **종료 시각**: 2026-09-12 22:15:50
- **실행 대상**: 구PC (hostname=DESKTOP-202AP4O, 사용자=Desktop)

## 1. 현재 Tailscale 상태 (구PC 직접 확인)

```
100.98.73.90  desktop-202ap4o-1  simon@  windows  -
100.83.83.53  desktop-202ap4o    simon@  windows  offline, last seen 6h ago
100.67.79.19  desktop-2udnsls    simon@  windows  active; direct 172.30.1.7:41641, tx 31968 rx 4400
```

**현재 사용 중인 Tailscale IP**: `100.98.73.90` (IPv6: `fd7a:115c:a1e0::a53b:495b`)

**참고 — 신PC tailscale status와의 차이**:
TASK 컨텍스트에는 "신PC tailscale status에서 구PC가 offline/idle로 보임"이라고 기록되어 있었으나,
지금 구PC 자체 `tailscale status`에서는 desktop-202ap4o-1 (100.98.73.90)이 목록에 보이며 상태가 `-`로 표시된다.
신PC 쪽에서 보는 상태와 구PC 자체 상태는 다를 수 있으므로, 신PC에서 다시 `tailscale status`를 실행해
100.98.73.90의 현재 상태를 직접 확인해야 한다. (이 진단은 구PC 측 확인만 포함한다.)

**과거 IP 참고**: 100.83.83.53은 offline 상태 (6시간 전 마지막 접속). 현재 주소로 사용하지 않는다.

## 2. Tailscale 서비스/프로세스 상태

- **Tailscale 서비스**: Name=Tailscale, Status=Running, StartType=Automatic
- 부팅 시 자동 시작 설정됨. 현재 실행 중.

## 3. TermService (원격 데스크톱 서비스) 상태

- **TermService**: Name=TermService, Status=Running, StartType=Automatic
- 부팅 시 자동 시작 설정됨. 현재 실행 중.

## 4. TCP 3389 LISTEN 여부

- `::` (IPv6 all-interfaces): 3389 Listen, OwningProcess=1176 (TermService)
- `0.0.0.0` (IPv4 all-interfaces): 3389 Listen, OwningProcess=1176 (TermService)
- netstat 확인: TCP 0.0.0.0:3389 LISTENING, TCP [::]:3389 LISTENING, UDP 0.0.0.0:3389, UDP [::]:3389

**결론**: 3389 포트가 IPv4/IPv6 모두 Listen 중이다.

## 5. 3389 인바운드 방화벽 상태

- `Get-NetFirewallRule -DisplayName '*Remote Desktop*'` 조회가 실패했다 (권한/쿼리 문제 가능).
- 방화벽 허용 여부는 이 진단에서 직접 확인하지 못했다.
- 신PC에서 3389 도달 테스트(ping/포트 스캔)로 간접 확인하는 것이 더 확실하다.

## 6. RDP 수신 가능 여부

- `Win32_TerminalServiceSetting.AllowTSConnections` = **1 (True)**
- RDP 연결 수락이 활성화되어 있다.

## 7. 현재 세션

```
USERNAME    SESSIONNAME    ID    STATE    IDLE TIME    LOGON TIME
>desktop    console        1     Active   49           2026-09-12 2:10
```

- console 세션이 Active 상태. 현재 사용자는 desktop (로그온 시간 2026-09-12 2:10).

## 8. 종합 판단

| 항목 | 상태 |
|------|------|
| 현재 Tailscale IP | 100.98.73.90 (확보됨) |
| Tailscale 서비스 | Running, Automatic |
| TermService | Running, Automatic |
| TCP 3389 LISTEN | 예 (::, 0.0.0.0 모두) |
| RDP 수신 허용 | 예 (AllowTSConnections=1) |
| 인바운드 방화벽 | 미확인 (조회 실패, 신PC 측 테스트 필요) |
| 신PC→구PC 도달성 | 이 진단에서는 확인 불가. 신PC에서 ping/3389 테스트로 확인 필요 |

**추정 원인 (확정 아님, 신PC 측 확인 필요)**:
- 구PC 측에서는 RDP 수신 준비가 되어 있다. 신PC에서 ping/3389 도달이 안 된다면,
  (a) 신PC tailscale status에서 구PC가 실제로 offline인지 재확인이 필요하거나,
  (b) 방화벽/라우팅/세션 수준에서 신PC→구PC 경로가 막혀 있을 가능성이 있다.
- 구PC 자체 Tailscale 로그아웃/재인증 필요 상태는 현재 아님 (서비스 Running, IP 있음).

**신PC에서 이어서 할 일**:
1. 신PC에서 `tailscale status`로 100.98.73.90의 현재 상태 재확인
2. 신PC에서 `ping 100.98.73.90` 재시도
3. 신PC에서 100.98.73.90:3389 도달 테스트 (Test-NetConnection 또는 nc/텔넷 등)
4. 필요하면 구PC 측 인바운드 방화벽 규칙을 별도 권한으로 확인

## 9. 결과 파일 위치

- 결과 (완료): `tasks/completed/RDP_recovery_diagnostics_100.98.73.90_result.md`
- 이 보고서: `reports/RDP_recovery_diagnostics_100.98.73.90.md`

## 10. 신솔라 확인용 요약

신솔라는 이 보고서를 pull한 뒤 아래를 확인한다:
- 현재 Tailscale IP: **100.98.73.90**
- Tailscale 연결 상태: 구PC 자체 기준 Running, IP 있음 (신PC 측 상태 재확인 필요)
- 3389 LISTEN 여부: **예**
- TermService 상태: Running
- 방화벽 상태: 미확인 (신PC 측 테스트 또는 별도 권한으로 확인 필요)
- 복구 필요 여부: 구PC 측 RDP 수신 준비는 되어 있음. 신PC→구PC 도달성 재확인이 다음 단계.
