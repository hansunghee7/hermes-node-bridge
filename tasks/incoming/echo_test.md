REQUESTER:
신솔라

EXECUTOR:
구솔라

TARGET:
구PC

OBJECTIVE:
GitHub 업무 브리지가 구솔라에게 전달되고, 구솔라가 실제로 읽고, 결과를 다시 GitHub에 기록할 수 있는지 최소 E2E 검증한다.

CONTEXT:
- 신PC와 구PC 사이 RDP 연결이 현재 되지 않는다.
- 본 task는 RDP 문제보다 먼저, GitHub 브리지가 실제로 작동하는지 확인하기 위한 최소 테스트다.
- 구솔라는 이 task를 읽은 뒤 자신의 호스트명과 현재 시각을 기록하고, 결과를 tasks/completed/와 reports/에 남긴다.

ACTION:
1. 이 저장소를 pull한다.
2. tasks/incoming/에서 EXECUTOR가 구솔라로 지정된 task를 찾는다.
3. 이 task를 읽는다.
4. 구PC에서 hostname을 확인한다. (예: hostname 명령어, 또는 PowerShell/cmd에서 컴퓨터 이름 확인)
5. 현재 시각을 기록한다.
6. 아래 결과를 작성한다.
   - tasks/completed/echo_test_result.md
   - reports/echo_test.md
7. 커밋 후 푸시한다.

SUCCESS_CRITERIA:
- 구솔라가 실제로 hostname과 시각을 기록했다.
- tasks/completed/echo_test_result.md가 생성되었다.
- reports/echo_test.md가 생성되었다.
- 커밋과 푸시가 성공했다.
- 신솔라가 pull하여 결과를 확인할 수 있다.

VERIFICATION:
신솔라는 git pull 후 아래 파일을 확인한다.
- tasks/completed/echo_test_result.md
- reports/echo_test.md
파일 안에 구PC의 hostname과 실행 시각이 기록되어 있으면 성공.

RESULT_LOCATION:
tasks/completed/echo_test_result.md
reports/echo_test.md
