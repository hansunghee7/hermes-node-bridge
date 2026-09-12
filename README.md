# hermes-node-bridge

신솔라(Hermes Control Tower, 신PC) ↔ 구솔라(Hermes Worker, 구PC) 간 업무 전달 채널.

## 역할 구분

- 신솔라 = 신PC에서 실행되는 Hermes 에이전트
- 구솔라 = 구PC에서 실행되는 Hermes 에이전트
- 신PC = 신솔라가 실행되는 물리 PC
- 구PC = 구솔라가 실행되는 물리 PC

PC의 IP 주소만으로 노드의 정체성을 판단하지 않는다.

## 저장소 구조

```
README.md
tasks/
  incoming/     # 신솔라 → 구솔라 작업 요청
  completed/    # 구솔라 완료 결과
  failed/       # 구솔라 실패 결과
reports/       # 구솔라 실행 결과 보고
```

## 업무 흐름

1. 신솔라가 `tasks/incoming/`에 Markdown task 파일을 생성/커밋/푸시한다.
2. 구솔라는 `tasks/incoming/`을 pull하여 새 task를 읽는다.
3. 구솔라가 작업을 수행하고 결과를 `tasks/completed/` 또는 `tasks/failed/`에 작성한다.
4. 구솔라가 `reports/`에 실행 결과를 작성한다.
5. 구솔라가 커밋/푸시한다.
6. 신솔라가 pull하여 결과를 확인한다.

## Task 파일 형식

```markdown
REQUESTER:
EXECUTOR:
TARGET:
OBJECTIVE:
CONTEXT:
ACTION:
SUCCESS_CRITERIA:
VERIFICATION:
```

- REQUESTER: 요청 Hermes 에이전트 (예: 신솔라)
- EXECUTOR: 실행 Hermes 에이전트 (예: 구솔라)
- TARGET: 실행 대상 (예: 구PC, 구PC의 특정 서비스/주소)
- OBJECTIVE: 목표
- CONTEXT: 배경/이미 알려진 정보
- ACTION: 수행 지시 (구체적인 명령어/확인 항목)
- SUCCESS_CRITERIA: 성공 판정 기준
- VERIFICATION: 검증 방법/증거

## 구솔라 동작

구솔라는 다음을 수행한다.

1. 저장소 pull
2. `tasks/incoming/`에서 자신이 EXECUTOR로 지정된 최근 task를 찾는다
3. task를 읽는다
4. TARGET에서 실제 작업을 수행한다
5. 결과 파일을 `tasks/completed/` 또는 `tasks/failed/`에 작성한다
6. `reports/`에 결과 요약/증거를 작성한다
7. 커밋 후 푸시한다

파일명은 다음과 같이 쓴다.

- 요청: `tasks/incoming/<작업식별자>.md`
- 완료: `tasks/completed/<작업식별자>_result.md`
- 실패: `tasks/failed/<작업식별자>_result.md`
- 보고: `reports/<작업식별자>.md`

## 구솔라 fetch 방법

```bash
git clone <REPO_URL>
# 또는 이미 복제된 경우
git pull
```

## 구솔라 결과 push 방법

```bash
git add tasks/completed/ tasks/failed/ reports/
git commit -m "완료: <작업식별자>"
git push
```

실패 시:

```bash
git add tasks/failed/ reports/
git commit -m "실패: <작업식별자>"
git push
```

## 저장소 URL

구솔라는 아래 URL을 사용한다.

- 저장소: https://github.com/hansunghee7/hermes-node-bridge
- clone: `git clone https://github.com/hansunghee7/hermes-node-bridge.git`
- pull: 저장소 복제 후 `git pull`
- 웹 열람: https://github.com/hansunghee7/hermes-node-bridge
