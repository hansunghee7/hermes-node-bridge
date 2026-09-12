# 운영 방식

이 저장소는 신솔라와 구솔라가 업무를 주고받는 공식 채널이다.

- 신솔라: 신PC의 Hermes Control Tower
- 구솔라: 구PC의 Hermes Worker
- 신PC/구PC: 물리 PC

저장소는 공개 저장소다.
읽기는 누구나 가능하다.
쓰기는 저장소 쓰기 권한이 있는 계정만 가능하다.

## 디렉터리 규칙

```
tasks/incoming/     # 신솔라가 구솔라에게 전달한 미처리 업무
tasks/completed/    # 구솔라 완료 결과
tasks/failed/       # 구솔라 실패 결과
reports/           # 구솔라 실행 보고
```

## 업무 문서 형식

모든 TASK는 아래 형식으로 만든다.

```text
TASK_ID:
REQUESTER:
EXECUTOR:
TARGET:
OBJECTIVE:
CONTEXT:
ACTION:
SUCCESS_CRITERIA:
VERIFICATION:
EXPECTED_OUTPUT:
```

TASK_ID는 중복 실행을 막는 열쇠다.
한 TASK는 하나의 성과목표를 가져야 한다.

## 구솔라 작동 방식

구솔라는 다음 순서로 작업한다.

1. 저장소를 clone하거나 pull한다.
2. tasks/incoming/에서 아직 처리되지 않은 TASK를 찾는다.
3. TASK_ID를 기록한다.
4. TASK를 읽는다.
5. TARGET에서 실제 작업을 수행한다.
6. 결과를 검증한다.
7. tasks/completed/ 또는 tasks/failed/에 결과를 작성한다.
8. reports/에 보고를 작성한다.
9. commit하고 push한다.

처리 순서:
- 사람이 직접 변경하지 않는 한, incoming의 TASK는 구솔라가 가져간다.
- 이미 completed/failed로 이동한 TASK는 다시 처리하지 않는다.
- 동일한 TASK_ID가 incoming에 다시 들어오면 새로 처리하지 말고 상태를 확인한다.

## 알림과 폴링

가능하면 Telegram으로 새 TASK를 알린다.
Telegram이 불가능하면 구솔라가 polling으로 incoming을 확인한다.

Telegram 알림 예:
- TASK 등록: TASK-003 등록: tasks/incoming/TASK-003.md 확인
- 완료 알림: TASK-003 완료: 결과 reports/에 기록

Telegram에는 상세 작업 내용을 넣지 않는다.

Telegram방은 작업 토론 공간이 아니다.
신솔라와 구솔라는 Telegram방에서 계속 말을 주고받지 않는다.
Telegram은 핑만 주고받는 공간으로 쓴다.
작업 내용, 결과, 설명, 논의, 상태 토론은 Telegram에 남기지 않는다.
실제 내용은 저장소 문서로만 남긴다.
필요할 때만 짧게 핑을 보내 “지금 뭔가 있다”만 알린다.

Polling 예:
- 일정 간격으로 tasks/incoming/을 확인한다.
- 새 TASK가 있으면 처리한다.
- 처리 중이면 중복 실행하지 않는다.
- 처리 불가면 실패를 기록한다.

## 결과 파일 배치

- 완료: tasks/completed/<TASK_ID>_result.md
- 실패: tasks/failed/<TASK_ID>_result.md
- 보고: reports/<TASK_ID>.md

## 신솔라 역할

신솔라는 다음을 담당한다.

- 목표 정의
- 작업 분해
- TASK 생성
- 구솔라 위임
- 결과 검증
- 필요 시 재지시

## 구솔라 역할

구솔라는 다음을 담당한다.

- TASK 수신
- 실제 실행
- 오류 복구
- 결과 검증
- 보고서 작성
- 완료 상태 기록

## 범위

이번 채널의 목표는 신솔라와 구솔라가 사용자 중계 없이 업무를 주고받는 실전 연계체계를 만드는 것이다.

다음 작업은 이 채널에서 하지 않는다.

- 불필요한 Provider 변경
- 다른 AI 시스템 최적화
- 신PC/구PC 전체 최적화
- RDP 문제 재조사
- Hermes 구조의 대규모 재설계

## 문서 시작/종료 원칙

모든 작업은 말로만 시작하거나 말로 끝내지 않는다.

새 작업을 시작할 때는:
- tasks/incoming/에 TASK 문서를 만든다
- TASK_ID, 요청자, 실행자, 대상, 목표, 맥락, 실행 항목, 성공 기준, 검증 방법을 남긴다
- 가능한 빨리 push한다

작업 중 상태가 바뀌면:
- 현재 상태, 현재 IP, 다음 행동, 대기 이유를 저장소 문서로 남긴다
- 세션이 닫혀도 이어질 수 있게 한다

작업을 끝낼 때는:
- tasks/completed/ 또는 tasks/failed/에 결과 문서를 남긴다
- reports/에 요약 보고서를 남긴다
- 커밋과 push까지 마쳐야 완료로 본다

종료 전 확인:
- 신PC에서 pull하여 최신 상태를 확인한다
- 문서가 없으면 완료되지 않은 것으로 본다

이 원칙 때문에, 신솔라와 구솔라 모두 중요 판단, 현재 상태, 다음 액션, 실패 사유는 반드시 문서로 남긴다.

## 저장소 정보

저장소: https://github.com/hansunghee7/hermes-node-bridge
clone: git clone https://github.com/hansunghee7/hermes-node-bridge.git
풀: git pull
웹: https://github.com/hansunghee7/hermes-node-bridge
