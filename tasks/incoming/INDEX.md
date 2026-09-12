# Incoming TASK 목록

현재 tasks/incoming/에 등록된 TASK는 아래와 같다.

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
