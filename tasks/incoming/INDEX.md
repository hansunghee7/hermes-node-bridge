# Incoming TASK 목록

현재 tasks/incoming/에 등록된 TASK와 참고 문서는 아래와 같다.

- GU_SOLAR_ENTRY_WORKFLOW.md
  - TASK_ID: 구솔라 진입 워크플로
  - 목적: 구솔라가 세션 끊김 후 복귀하거나 새로 합류했을 때 따라야 할 표준 진입 절차
  - 상태: 우선 확인
  - 참고: 구솔라가 들어오면 먼저 이 문서를 본다

- check-rdp-firewall-3389-100.98.73.90.md
  - TASK_ID: check-rdp-firewall-3389-100.98.73.90
  - 목적: 구PC 인바운드 방화벽 3389 허용 여부 직접 확인
  - 상태: 구솔라 처리 대기
  - 참고: 구솔라가 이미 RDP_recovery_diagnostics_100.98.73.90에서 미확인으로 남긴 항목

- GU_SOLAR_RECOVERY_BRIEF.md
  - TASK_ID: 구솔라 복귀 지시서
  - 목적: 구솔라가 파워쉘 재실행 후 빠르게 업무를 재개하도록 안내
  - 상태: 우선 확인
  - 참고: 구솔라는 복귀 즉시 이 문서를 먼저 읽는다

- RDP_recovery_diagnostics_100.98.73.90.md
  - TASK_ID: RDP_recovery_diagnostics_100.98.73.90
  - 목적: 구PC Tailscale 상태와 RDP 관련 상태를 직접 확인하여 신PC→100.98.73.90 RDP 실패 원인 특정
  - 상태: 구솔라 완료, 신솔라 검증 중
  - 참고: 구솔라 결과 및 보고 완료, 신PC에서 독립 검증 진행 중

- echo_test.md
  - TASK_ID: echo_test
  - 목적: 구솔라가 저장소를 읽고 결과를 push할 수 있는지 최소 E2E 확인
  - 상태: 신PC에서 구솔라 역할 시뮬레이션 결과 존재
  - 참고: tasks/completed/echo_test_result.md, reports/echo_test.md 확인
  - 실제 구PC 실행은 구솔라가 수행해야 함

- tailscale-reconnect-prevention.md
  - TASK_ID: tailscale-reconnect-prevention
  - 목적: 구PC Tailscale 단절 재발방지 체계 구축
  - 상태: 구솔라 선행 처리 대상
  - 참고: 구솔라가 Tailscale 재발방지대책을 먼저 처리 중

- GU_SOLAR_CHECKLIST.md
  - TASK_ID: 구솔라 실행 전 체크
  - 목적: 구솔라가 작업 시작 전 확인할 순서 정리
  - 상태: 참고 문서
  - 참고: 구솔라는 실행 전 이 문서를 먼저 읽는다

구솔라는 아래 순서로 처리한다.

1. 가장 위에 있는 미처리 TASK부터 확인한다.
2. TASK_ID를 기록한다.
3. 이미 completed/failed로 이동한 TASK는 다시 처리하지 않는다.
4. 처리 결과는 tasks/completed/ 또는 tasks/failed/에 남긴다.
5. 보고서는 reports/에 남긴다.

TASK를 처리했으면 해당 문서를 incoming에서 이동시키지 말고, 결과 파일로 상태를 남긴다.
이동이 필요한 경우 신솔라가 정리한다.
