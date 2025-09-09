## Question #228
A company has an API that receives real-time data from a fleet of monitoring devices. 
The API stores this data in an Amazon RDS DB instance for later analysis. 
The amount of data that the monitoring devices send to the API fluctuates. 
During periods of heavy traffic, the API often returns timeout errors.

After an inspection of the logs, the company determines that the database is not capable of processing the volume of write traffic that comes from the API. 
A solutions architect must minimize the number of connections to the database and must ensure that data is not lost during periods of heavy traffic.

Which solution will meet these requirements?

A. Increase the size of the DB instance to an instance type that has more available memory.

B. Modify the DB instance to be a Multi-AZ DB instance. Configure the application to write to all active RDS DB instances.

C. Modify the API to write incoming data to an Amazon Simple Queue Service (Amazon SQS) queue. Use an AWS Lambda function that Amazon SQS invokes to write data from the queue to the database.

D. Modify the API to write incoming data to an Amazon Simple Notification Service (Amazon SNS) topic. Use an AWS Lambda function that Amazon SNS invokes to write data from the topic to the database.

## Question #228 분석

### 요구사항
- **실시간 모니터링 데이터** API 수신
- **RDS 저장** 후 분석
- **변동하는 트래픽**으로 타임아웃 발생
- **DB 연결 수 최소화** 및 **데이터 손실 방지**

### 핵심 문제
```yaml
현재 상황:
- API가 직접 RDS에 쓰기
- 트래픽 급증 시 DB 처리 한계 초과
- 타임아웃으로 데이터 손실 발생
```

### 선택지 분석

**A. DB 인스턴스 크기 증대**
- **임시 해결**: 트래픽이 더 증가하면 동일 문제 재발 ❌
- **비용 증가**: 지속적인 고비용 인스턴스 운영 ❌
- **근본 해결 실패**: 아키텍처 문제 미해결 ❌

**B. Multi-AZ + 모든 인스턴스 쓰기**
- **Multi-AZ 오해**: Multi-AZ는 읽기 분산용이 아님 ❌
- **쓰기 분산 불가**: Primary 인스턴스에만 쓰기 가능 ❌
- **기술적 불가능**: 잘못된 아키텍처 이해 ❌

**C. SQS 큐 + Lambda 처리** ⭐
- **트래픽 평준화**: SQS가 급증 트래픽 버퍼링 ✅
- **데이터 손실 방지**: 큐에서 메시지 지속성 보장 ✅
- **연결 수 최소화**: Lambda가 배치로 DB 처리 ✅
- **자동 확장**: Lambda 동시성으로 처리량 조절 ✅

**D. SNS 토픽 + Lambda**
- **일회성 전달**: SNS는 메시지 지속성 없음 ❌
- **데이터 손실 위험**: 처리 실패 시 메시지 손실 ❌
- **버퍼링 없음**: 트래픽 급증 문제 해결 안됨 ❌

### SQS + Lambda 아키텍처

```yaml
데이터 흐름:
1. 모니터링 디바이스 → API
2. API → SQS 큐 (즉시 응답)
3. SQS → Lambda 트리거 (배치 처리)
4. Lambda → RDS (제어된 연결)

장점:
- 트래픽 급증 시 큐에 버퍼링
- Lambda 동시성으로 처리량 조절
- DB 연결 수 제한 가능
- 메시지 재시도로 안정성 보장
```
