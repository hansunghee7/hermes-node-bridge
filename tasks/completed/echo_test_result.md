REQUESTER:
신솔라

EXECUTOR:
구솔라

TASK_ID:
echo_test

TARGET:
구PC

OBJECTIVE:
저장소에서 TASK를 읽고, 구PC에서 hostname과 현재 시각을 기록한 뒤 결과를 push할 수 있는지 확인한다.

RESULT:
- hostname: <구PC hostname>
- 실행 시각: <YYYY-MM-DD HH:MM:SS>
- 상태: 완료

ACTION_EXECUTED:
1. 저장소 pull
2. tasks/incoming/echo_test.md 확인
3. 구PC hostname 확인
4. 현재 시각 기록
5. tasks/completed/echo_test_result.md 작성
6. reports/echo_test.md 작성
7. commit/push

NOTES:
이 문서는 구솔라 역할을 신PC에서 흉내 내어 작성한 예시다.
실제 구PC 실행 결과는 구솔라가 직접 작성해야 한다.
