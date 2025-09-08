## Question #201
A company is developing a marketing communications service that targets mobile app users. 
The company needs to send confirmation messages with Short Message Service (SMS) to its users. 
The users must be able to reply to the SMS messages. 
The company must store the responses for a year for analysis.

What should a solutions architect do to meet these requirements?

A. Create an Amazon Connect contact flow to send the SMS messages. Use AWS Lambda to process the responses.

B. Build an Amazon Pinpoint journey. Configure Amazon Pinpoint to send events to an Amazon Kinesis data stream for analysis and archiving.

C. Use Amazon Simple Queue Service (Amazon SQS) to distribute the SMS messages. Use AWS Lambda to process the responses.

D. Create an Amazon Simple Notification Service (Amazon SNS) FIFO topic. Subscribe an Amazon Kinesis data stream to the SNS topic for analysis and archiving.

## Question #201 분석

### 요구사항
- **모바일 앱 사용자**에게 SMS 확인 메시지 발송
- **양방향 SMS**: 사용자 응답 가능
- **응답 저장**: 1년간 분석용 보관
- **마케팅 커뮤니케이션** 서비스

### 선택지 분석

**A. Amazon Connect + Lambda**
- **Connect 용도**: 콜센터/고객 서비스용, 마케팅 SMS에 부적합 ❌
- **복잡성**: 불필요한 오버엔지니어링 ❌

**B. Amazon Pinpoint + Kinesis** ⭐
- **Pinpoint SMS**: 마케팅 SMS 전용 서비스 ✅
- **양방향 지원**: SMS 응답 수신 및 처리 ✅
- **Journey 기능**: 마케팅 캠페인 자동화 ✅
- **Kinesis 통합**: 응답 데이터 스트리밍 및 분석 ✅
- **1년 보관**: 데이터 스트림에서 장기 저장소로 전송 ✅

**C. SQS + Lambda**
- **SQS 역할 오해**: 메시지 큐는 SMS 발송 서비스 아님 ❌
- **SMS 기능 없음**: SQS는 SMS 전송 불가 ❌

**D. SNS FIFO + Kinesis**
- **SNS 제한**: 일방향 알림만, 응답 수신 불가 ❌
- **양방향 미지원**: SMS 응답 처리 기능 없음 ❌

### Amazon Pinpoint 장점

```yaml
마케팅 최적화:
✅ SMS/Email/Push 통합 플랫폼
✅ 사용자 세그멘테이션
✅ A/B 테스트 지원
✅ 캠페인 성과 분석

양방향 SMS:
✅ 발신 SMS 발송
✅ 수신 SMS 처리
✅ 키워드 기반 자동 응답
✅ 실시간 이벤트 스트리밍

데이터 분석:
✅ Kinesis로 실시간 스트리밍
✅ S3/Redshift 장기 저장
✅ 응답률 분석
✅ 사용자 행동 패턴 분석
```

### 아키텍처 흐름

```yaml
1. Pinpoint Journey로 SMS 발송
2. 사용자 SMS 응답
3. Pinpoint가 응답 수신
4. 이벤트를 Kinesis로 스트리밍
5. Kinesis에서 S3/분석 도구로 전송
6. 1년간 데이터 보관 및 분석
```
