## Question #207
A company owns an asynchronous API that is used to ingest user requests and, based on the request type, dispatch requests to the appropriate microservice for processing. 
The company is using Amazon API Gateway to deploy the API front end, and an AWS Lambda function that invokes Amazon DynamoDB to store user requests before dispatching them to the processing microservices.

The company provisioned as much DynamoDB throughput as its budget allows, but the company is still experiencing availability issues and is losing user requests.

What should a solutions architect do to address this issue without impacting existing users?

A. Add throttling on the API Gateway with server-side throttling limits.

B. Use DynamoDB Accelerator (DAX) and Lambda to buffer writes to DynamoDB.

C. Create a secondary index in DynamoDB for the table with the user requests.

D. Use the Amazon Simple Queue Service (Amazon SQS) queue and Lambda to buffer writes to DynamoDB.

## Question #207 분석

### 요구사항
- **비동기 API** 사용자 요청 수집
- **DynamoDB** 처리량 한계로 가용성 문제
- **사용자 요청 손실** 발생
- **기존 사용자 영향 없이** 해결

### 핵심 문제
```yaml
현재 상황:
- API Gateway → Lambda → DynamoDB 직접 쓰기
- DynamoDB 처리량 한계 초과
- 요청 급증 시 쓰기 실패
- 사용자 요청 손실

근본 원인: 버퍼링 없는 직접 쓰기
```

### 선택지 분석

**A. API Gateway 스로틀링**
- **사용자 영향**: 요청 차단으로 사용자 경험 저하 ❌
- **근본 해결 실패**: DynamoDB 병목 미해결 ❌
- **요구사항 위반**: "기존 사용자 영향 없이" 조건 위배 ❌

**B. DAX + Lambda 버퍼링**
- **DAX 목적**: 읽기 가속화용, 쓰기 버퍼링 아님 ❌
- **Lambda 한계**: 실행 시간 제한으로 버퍼링 부적합 ❌
- **문제 해결 실패**: DynamoDB 쓰기 병목 지속 ❌

**C. 보조 인덱스 생성**
- **쿼리 성능**: 읽기 성능만 개선 ❌
- **쓰기 성능**: 처리량 병목 미해결 ❌
- **추가 부하**: 인덱스 유지로 쓰기 부하 증가 ❌

**D. SQS 큐 + Lambda 버퍼링** ⭐
- **요청 보존**: SQS가 모든 요청 안전하게 저장 ✅
- **처리량 평준화**: Lambda가 DynamoDB 한계 내에서 처리 ✅
- **사용자 영향 없음**: 즉시 SQS 응답, 백그라운드 처리 ✅
- **가용성 향상**: 요청 손실 방지 ✅

### SQS 버퍼링 아키텍처

```yaml
Before:
API Gateway → Lambda → DynamoDB (병목)
결과: 요청 손실

After:  
API Gateway → Lambda → SQS → Lambda → DynamoDB
결과: 모든 요청 보존, 안정적 처리
```

### 처리 흐름

```yaml
1. 사용자 요청 → API Gateway
2. Lambda가 SQS에 즉시 저장 (빠른 응답)
3. SQS → 별도 Lambda가 배치 처리
4. DynamoDB 처리량 내에서 안정적 쓰기
5. 요청 손실 없이 모든 데이터 보존
```
