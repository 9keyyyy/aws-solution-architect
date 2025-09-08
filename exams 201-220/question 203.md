## Question #203
The customers of a finance company request appointments with financial advisors by sending text messages. 
A web application that runs on Amazon EC2 instances accepts the appointment requests. 
The text messages are published to an Amazon Simple Queue Service (Amazon SQS) queue through the web application. 
Another application that runs on EC2 instances then sends meeting invitations and meeting confirmation email messages to the customers. 
After successful scheduling, this application stores the meeting information in an Amazon DynamoDB database.

As the company expands, customers report that their meeting invitations are taking longer to arrive.

What should a solutions architect recommend to resolve this issue?

A. Add a DynamoDB Accelerator (DAX) cluster in front of the DynamoDB database.

B. Add an Amazon API Gateway API in front of the web application that accepts the appointment requests.

C. Add an Amazon CloudFront distribution. Set the origin as the web application that accepts the appointment requests.

D. Add an Auto Scaling group for the application that sends meeting invitations. Configure the Auto Scaling group to scale based on the depth of the SQS queue.

## Question #203 분석

### 요구사항
- **금융 회사** 고객의 약속 예약 시스템
- **문제**: 회의 초대장 도착 시간 지연
- **확장** 상황에서 성능 저하

### 현재 아키텍처
```yaml
흐름:
1. 고객 텍스트 → 웹 앱 (EC2)
2. 웹 앱 → SQS 큐 발행
3. 이메일 앱 (EC2) → SQS 처리
4. 이메일 발송 → DynamoDB 저장

병목: 이메일 처리 앱의 처리 능력 부족
```

### 선택지 분석

**A. DAX 클러스터 추가**
- **DynamoDB 가속화**: 읽기 성능만 개선 ❌
- **병목 위치 오해**: 문제는 이메일 처리, DB 읽기 아님 ❌

**B. API Gateway 추가**
- **웹 앱 앞단**: 요청 접수 단계 개선 ❌
- **병목 위치 오해**: 문제는 이메일 발송 처리 속도 ❌

**C. CloudFront 배포**
- **정적 콘텐츠 가속**: 웹 앱 응답 속도 개선 ❌
- **병목 위치 오해**: 이메일 처리 지연과 무관 ❌

**D. Auto Scaling + SQS 큐 깊이 기반** ⭐
- **정확한 병목 해결**: 이메일 처리 앱 확장 ✅
- **SQS 큐 깊이**: 실제 처리 대기량 기반 확장 ✅
- **자동 대응**: 부하 증가 시 자동 인스턴스 추가 ✅
- **비용 효율**: 필요 시에만 확장 ✅

### 병목 분석

```yaml
문제 지점:
❌ 웹 앱 성능 (요청 접수는 정상)
❌ SQS 큐 성능 (큐는 메시지 적재만)
❌ DynamoDB 성능 (저장은 마지막 단계)
✅ 이메일 처리 앱 (SQS → 이메일 발송)

해결책:
이메일 처리 인스턴스 수 증가 필요
```

### Auto Scaling 설정

```yaml
CloudWatch 메트릭:
- ApproximateNumberOfMessages (SQS)
- 타겟: 인스턴스당 5개 메시지

확장 정책:
- 큐 깊이 증가 → 인스턴스 추가
- 큐 깊이 감소 → 인스턴스 감소
- 처리 능력 = 큐 백로그 비례
```
