# 신PC 진행 기록

## 시각
신PC에서 확인 및 시도를 병행한 시점 기준

## 신PC가 진행한 것
- RDP 로그인 시도
  - 대상: 구PC Tailscale IP 100.98.73.90
  - 동작: mstsc 실행
  - 결과: RDP 창 실행 시도까지 성공, 로그인 성공/실패는 자동 판정 불가
  - 근거: reports/RDP_login_attempt_100.98.73.90.md

- SSH 클라이언트 존재 여부 확인
  - 결과: ssh.exe 존재
  - 출처: C:\Program Files\Git\usr\bin\ssh.exe
  - sshd 서비스: 미확인
  - ProgramData/ssh: 미확인
  - 신PC에 SSH 서버가 현재 떠 있는 상태는 아님

- 백로그 설계
  - 신PC 병행 과제 문서 작성
  - 0순위: 텔레그램 소통 확보
  - 1순위: SSH 상호 활용
  - 그 외: 신PC 즉시 진행 가능 항목 분리
  - 문서: tasks/incoming/SHINPC_BACKLOG_PARALLEL_TASKS.md

## 신PC가 아직 확정하지 못한 것
- 실제 RDP 로그인 성공 여부
- 신PC SSH 서버 사용 가능 여부
- 텔레그램 발송 가능 여부
- 구PC SSH 존재 여부
- 신PC↔구PC 간 SSH 실제 연결 가능 여부

## 다음 신PC 진행 후보
- 신PC SSH 서버 준비 가능 여부 시험
- 신PC에서 텔레그램 발송 가능 범위 확인
- RDP 로그인 성공/실패 정황 추가 확인
- SSH 상호 활용 방향 정리 후 구솔라와 연결 시험 준비
