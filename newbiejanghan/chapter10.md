# 알림 시스템 설계
## 알림 절차
### 기계별 지원 방안
- iOS: Apple Push Notification Service
- Android: Firebase Cloud Messaging
- SMS: third-party SMS API (ex. Twilio)
- EMail: third-party Email API (ex. SendGrid)

### 저장 데이터
- 국가 코드(country_code)
- 연락처(phone_number)
- 단말 토큰(device_token)

## 설계 유의사항
~~~~
- third-party services layer 의 확장성
- SPOF(Single Point of Failure), 규모 확장성 대응 필요
~~~~
### 알림 전송 서버
- API를 통해 알림 요청을 수신하고 처리
- 데이터베이스, 캐시에 질의하여 알림에 포함할 데이터 조회
- auto scaling 이 가능하게끔 설계

### 메세지 큐
- 알림 서버와 third party 사이에 존재
- 시스템 간 의존성을 제거하고 다량의 알림 요청을 대비할 수 있는 버퍼 역할.
- 각 third party 서비스 마다 큐를 설치하면 SPOF를 방지할 수 있음.

### 안정성
- 데이터 손실 방지: 큐에서 메세지를 꺼내 third-party service로 넘기는 **작업 서버**는 알림 로그 데이터베이스를 운영하여 재시도 메커니즘을 실행.
- 중복 전송 방지: 분산 시스템에서 100% 중복 전송을 방지할 방법은 없다. 중복을 탐지하는 메커니즘을 설계해서 중복 전송을 최소화.

### 큐 모니터링
- 메세지 큐에 쌓인 알림의 갯수를 모니터링하여 작업 서버 증설 여부를 판단.
