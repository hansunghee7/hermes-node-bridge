REQUESTER:
신솔라

EXECUTOR:
구솔라

TASK_ID:
RDP_recovery_diagnostics_100.98.73.90

TARGET:
구PC

OBJECTIVE:
현재 구PC의 Tailscale 상태와 RDP 관련 상태를 직접 확인하여, 신PC에서 100.98.73.90으로 RDP 접속이 실패하는 원인을 특정한다.

CONTEXT:
- 신PC에서 100.98.73.90으로 ping은 실패함
- 신PC에서 100.98.73.90:3389 테스트는 타임아웃됨
- 신PC tailscale status에서 구PC가 offline/idle로 보임
- 신솔라는 구PC에 직접 접근할 수 없으므로 구솔라가 구PC에서 직접 확인해야 함
- 과거 IP를 현재 주소라고 가정하지 않음

ACTION:
구솔라가 구PC에서 직접 아래를 확인한다.

1. 현재 Tailscale 상태
   - tailscale status
   - tailscale ip
   - 서비스/프로세스 실행 여부
2. 구PC가 Tailscale 네트워크에 연결되어 있는지 확인
3. Tailscale 로그아웃/재인증 필요 여부 확인
4. TermService 상태 확인
5. TCP 3389 LISTEN 여부 확인
6. 3389 인바운드 방화벽 상태 확인
7. 현재 구PC에서 실제 RDP 수신 가능 여부 확인
8. 가능하면 구PC에서 3389 포트가 외부에서 도달 가능한지 판단하는 데 필요한 정보 기록

SUCCESS_CRITERIA:
- 구PC의 현재 Tailscale IP가 확인됨
- Tailscale 연결 상태가 기록됨
- 3389 관련 상태가 직접 확인됨
- 신PC가 RDP 테스트를 재개할 수 있을 정도의 정보가 보고됨
- 추측이 아니라 구PC에서 직접 확인한 결과가 기록됨

VERIFICATION:
신솔라는 보고서를 직접 pull하여 아래를 확인한다.
- 현재 Tailscale IP
- Tailscale 연결 상태
- 3389 LISTEN 여부
- 방화벽/TermService 상태
- 복구 필요 여부

EXPECTED_OUTPUT:
- reports/RDP_recovery_diagnostics_100.98.73.90.md
- tasks/completed/RDP_recovery_diagnostics_100.98.73.90_result.md 또는 tasks/failed/ 동일 파일
