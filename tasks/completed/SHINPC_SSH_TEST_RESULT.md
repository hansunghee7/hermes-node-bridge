# 신PC SSH 시험 결과

## 대상
- 신PC OpenSSH 서버 준비 가능 여부

## 시도한 것
- Get-Service sshd 확인
- Get-WindowsCapability 확인
- Add-WindowsCapability OpenSSH.Server~~~~0.0.1.0 시도

## 결과
- Get-Service sshd: 보이지 않음
- Get-WindowsCapability: COMException 실패
- Add-WindowsCapability OpenSSH.Server: COMException 실패
- 신PC sshd 서비스: 현재 안 보임

## 결론
- 신PC에서 ssh 클라이언트는 사용 가능
- 신PC에서 sshd는 지금 당장 준비되지 않음
- 구PC→신PC SSH 방향은 신PC sshd 필요
- 신PC→구PC 방향은 구PC 측 ssh 존재 여부에 달림

## 다음
- 신PC sshd 준비 가능 여부를 다른 방식으로 재검토할지 판단
- 구PC ssh 존재 여부는 구솔라에게 확인
- SSH 방향은 먼저 정한 뒤 진행할 과제 분리
