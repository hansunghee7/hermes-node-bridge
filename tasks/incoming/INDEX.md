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
  - 상태: 구솔라 완료(부분), 인바운드 3389 Allow 확정 미완료
  - 결과 근거: tasks/completed/check-rdp-firewall-3389-100.98.73.90_result.md
  - 참고: 관리자 권한 부족으로 Get-NetFirewallRule 직접 조회 실패
  - 미확인: 인바운드 3389 Allow 규칙 존재 여부

- 구솔라_0순위_후속_방화벽3389_확정_텔레그램확인.md
  - TASK_ID: 구솔라_0순위_후속_방화벽3389_확정_텔레그램확인
  - 목적: 인바운드 3389 방화벽 허용 여부 확정, 구솔라 텔레그램 수신 가능 여부 확인
  - 상태: 구솔라 완료(부분), 인바운드 3389 Allow 미확정, 텔레그램 수신 여부 확정 불가
  - 결과 근거: tasks/completed/구솔라_0순위_후속_방화벽3389_확정_텔레그램확인_result.md, reports/구솔라_0순위_후속_방화벽3389_확정_텔레그램확인.md
  - 참고: 구솔라가 저장소 push까지 완료, INDEX 반영이 늦음
  - 미결: 인바운드 3389 Allow 규칙, 텔레그램 수신 가능 여부

- 구솔라_텔레그램확인_및_방화벽3389_확정.md
  - 상태: 구솔라 처리 완료로 추정, 문서 상태는 미정리
  - 참고: 구솔라_0순위_후속_방화벽3389_확정_텔레그램확인.md와 중복 가능, 신솔라가 정리

- 구솔라_추가확인_3389방화벽_텔레그램수신_재시도.md
  - TASK_ID: 구솔라_추가확인_3389방화벽_텔레그램수신_재시도
  - 목적: 인바운드 3389 방화벽 허용 여부 재확인, 구솔라 텔레그램 수신 가능 여부 확정
  - 상태: 구솔라 처리 대기, 지금 즉시 처리 가능
  - 참고: check-rdp-firewall-3389-100.98.73.90, 구솔라_0순위_후속_방화벽3389_확정_텔레그램확인의 미결을 이어서 처리
  - 주의: 관리자 권한 부족 시 그 사유를 남기고, 텔레그램 수신 가능 여부는 가능한 범위에서 판단해 남긴다

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

- GU_SOLAR_CHECK_SSH_STATUS.md
  - TASK_ID: 구솔라 SSH 상태 확인 요청
  - 목적: 구PC SSH 클라이언트/서버 존재 여부와 SSH 필요 방향 확인
  - 상태: 구솔라 처리 대기
  - 참고: 신PC는 ssh 클라이언트 존재, sshd 준비 불가 확인
  - 주의: 신PC→구PC, 구PC→신PC 중 어느 방향이 필요한지, 구PC에 ssh가 있는지 확인
  - 요청 내용: 구PC ssh 존재 여부, SSH 필요 방향, Tailscale SSH 가능 여부

- SHINPC_TELEGRAM_SEND_TEST.md
  - TASK_ID: 신PC 텔레그램 발송 시험
  - 목적: 신PC에서 텔레그램 알림 발송 가능 범위를 시험
  - 상태: 신PC 진행 준비, 시험 설계 단계
  - 참고: config.yaml hermes-telegram 도구셋 존재, Telegram Desktop 설치 흔적 존재
  - 주의: 민감정보 없이 가능한 범위에서만 시험
  - 병행 항목: 구솔라 텔레그램 수신 가능 여부 확인

- GU_SOLAR_TELEGRAM_RECEIVE_CHECK.md
  - TASK_ID: 구솔라 텔레그램 수신 가능 여부 확인
  - 목적: 구PC에서 텔레그램 알림 수신 가능 여부를 가능한 범위에서 확인
  - 상태: 구솔라 처리 대기
  - 참고: 신PC 텔레그램 발송 시험 준비 중, 구솔라 앱 실행 중 확인됨
  - 주의: 민감정보 없이 가능한 범위에서만 확인

- SHINPC_BACKLOG_PARALLEL_TASKS.md
  - TASK_ID: 신PC 병행 과제 및 백로그 설계
  - 목적: 신PC가 구PC 쪽 작업만 기다리지 않고 병행 가능한 과제를 정리하고 진행
  - 상태: 신PC 진행 중, SSH 가능 여부 시험은 완료했으나 sshd 준비 불가 확인
  - 참고: 신PC ssh 클라이언트 존재 확인, sshd는 미확인, Add-WindowsCapability 실패
  - 병행 방향: 0순위는 텔레그램 소통 확보, 1순위는 SSH 상호 활용, 그 외는 신PC 즉시 진행 가능 항목
  - SSH 결과: tasks/completed/SHINPC_SSH_TEST_RESULT.md
  - SSH 방향 확정: 신PC→구PC 우선 검토, 구PC→신PC는 신PC sshd 준비 필요

구솔라는 아래 순서로 처리한다.

1. 가장 위에 있는 미처리 TASK부터 확인한다.
2. TASK_ID를 기록한다.
3. 이미 completed/failed로 이동한 TASK는 다시 처리하지 않는다.
4. 처리 결과는 tasks/completed/ 또는 tasks/failed/에 남긴다.
5. 보고서는 reports/에 남긴다.

TASK를 처리했으면 해당 문서를 incoming에서 이동시키지 말고, 결과 파일로 상태를 남긴다.
이동이 필요한 경우 신솔라가 정리한다.
