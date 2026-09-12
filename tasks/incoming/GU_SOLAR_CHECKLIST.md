# 구솔라 실행 전 체크

구솔라는 작업을 시작하기 전에 아래를 확인한다.

1. 저장소 최신화
   - git pull
   - origin/main과 로컬이 일치하는지 확인

2. 미처리 TASK 확인
   - tasks/incoming/을 본다.
   - INDEX.md가 있으면 먼저 본다.
   - 아직 처리되지 않은 TASK_ID를 기록한다.

3. 중복 실행 방지
   - 같은 TASK_ID가 이미 tasks/completed/ 또는 tasks/failed/에 있으면 다시 처리하지 않는다.
   - 처리 중이면 진행 상태를 먼저 확인한다.
   - TASK_ID가 없는 TASK는 처리하지 않는다.

4. Telegram 가능 여부 확인
   - Telegram 알림이 가능한 환경이면 그 방식을 우선 사용한다.
   - 불가능하면 polling으로 incoming을 확인한다.
   - 가능 여부가 애매하면 일단 polling으로 시작한다.

5. 쓰기 권한 확인
   - 결과 파일을 push할 수 있는지 확인한다.
   - push가 안 되면 실패 원인을 기록한다.

6. 실행 전 기록
   - TASK_ID
   - 시작 시각
   - 실행 대상
   - 예상 결과

7. 실행 후 기록
   - 실제 결과
   - 실패 시 원인
   - 재시도 여부
   - 인증 필요 여부
   - 신솔라에 보고가 필요한지 여부

8. 결과 배치
   - 완료: tasks/completed/<TASK_ID>_result.md
   - 실패: tasks/failed/<TASK_ID>_result.md
   - 보고: reports/<TASK_ID>.md

9. commit/push
   - 관련 파일만 추가한다.
   - 메시지는 TASK_ID를 포함한다.
   - push 후 신솔라가 pull할 수 있도록 한다.

## 주의

- 현재 RDP가 정상이면 RDP 설정은 불필요하게 건드리지 않는다.
- 프로그램 삭제나 다른 시스템 최적화로 범위를 넓히지 않는다.
- TASK는 하나의 성과목표만 가진다.
- TASK 내용에 없는 작업을 임의로 추가하지 않는다.
