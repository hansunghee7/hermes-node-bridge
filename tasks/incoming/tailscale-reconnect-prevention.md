REQUESTER:
신솔라

EXECUTOR:
구솔라

TARGET:
구PC

OBJECTIVE:
구PC의 Tailscale 단절로 신PC → 구PC 원격접속이 끊기는 문제가 다시 발생하지 않도록 재발방지 체계를 구축한다.

CONTEXT:
- 기존 문제: 구PC Tailscale 로그아웃으로 IP가 변경되어 신PC → 구PC RDP가 끊김
- 현재는 RDP가 정상 작동 중이므로 RDP 설정 자체는 불필요하게 변경하지 않는다
- 프로그램 삭제나 다른 시스템 최적화로 범위를 확장하지 않는다
- 신PC/구PC 구분과 IP 기반 노드 추론은 하지 않는다

ACTION:
1. 현재 Tailscale 상태와 현재 Tailscale IP를 확인한다.
   - `tailscale status`
   - `tailscale ip`
   - 서비스/프로세스 상태도 확인
2. Tailscale이 부팅 후 자동으로 실행되고 로그인 상태가 유지되는지 확인한다.
3. Tailscale 상태를 주기적으로 확인하는 Watcher를 구현한다.
4. 다음 상태를 비정상으로 판정한다.
   - `Logged out`
   - `NeedsLogin`
   - Tailscale IP 없음
   - Tailscale 프로세스/서비스 중단
5. 비정상 상태가 발견되면 가능한 범위에서 자동 복구한다.
   - Tailscale 서비스 재시작
   - 상태 재확인
   - IP 재확인
6. 자동 복구가 성공하면 정상 상태를 다시 검증한다.
7. 자동 복구가 실패하고 사용자 인증이 필요한 경우:
   - 무한 재시도하지 않는다.
   - 문제 상태를 명확히 기록한다.
   - 신솔라에 필요한 사용자 조치를 보고한다.
8. 현재 Tailscale IP를 문서에 하드코딩하지 않는다.
   항상 `tailscale ip` 등으로 현재 IP를 확인한다.
9. RDP 자체도 최종적으로 확인할 수 있도록 현재 상태를 기록한다.
10. Watcher가 실제로 실행되고 있는지, 재부팅 후에도 살아나는지 검증한다.

SUCCESS_CRITERIA:
- Windows 부팅 후 Tailscale 자동 실행 확인
- 로그인 상태 확인
- Tailscale IP 존재 확인
- Watcher 자동 실행 확인
- 정상 상태 감지 확인
- 비정상 상태 감지 로직 확인
- 가능한 자동 복구 확인
- 복구 실패 시 신솔라에 보고하는 경로 확인
- 결과를 GitHub `reports/`에 기록

VERIFICATION:
최종 보고에 다음이 포함되어야 한다.
- 현재 Tailscale 상태
- 자동 실행 방식
- Watcher 실행 방식
- 감지 기준
- 자동 복구 범위
- 인증이 필요한 경우의 처리 방식
- 재부팅 후 검증 결과
- 남은 수동 조치

결과 파일:
- tasks/completed/tailscale-reconnect-prevention_result.md
- reports/tailscale-reconnect-prevention.md
