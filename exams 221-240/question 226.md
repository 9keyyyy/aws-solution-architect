## Question #226
A company collects data from thousands of remote devices by using a RESTful web services application that runs on an Amazon EC2 instance. 
The EC2 instance receives the raw data, transforms the raw data, and stores all the data in an Amazon S3 bucket. 
The number of remote devices will increase into the millions soon. 
The company needs a highly scalable solution that minimizes operational overhead.

Which combination of steps should a solutions architect take to meet these requirements? (Choose two.)

A. Use AWS Glue to process the raw data in Amazon S3.

B. Use Amazon Route 53 to route traffic to different EC2 instances.

C. Add more EC2 instances to accommodate the increasing amount of incoming data.

D. Send the raw data to Amazon Simple Queue Service (Amazon SQS). Use EC2 instances to process the data.

E. Use Amazon API Gateway to send the raw data to an Amazon Kinesis data stream. Configure Amazon Kinesis Data Firehose to use the data stream as a source to deliver the data to Amazon S3.

## Question #226 재분석 (A + E)

### 요구사항
- **수천 → 수백만 디바이스**로 확장
- **RESTful API**로 데이터 수집
- **고확장성** 및 **최소 운영 오버헤드**

### 정답 조합 (A + E)

**A. AWS Glue로 S3 데이터 처리** ⭐
- **서버리스 ETL**: 데이터 변환 작업을 완전 관리형으로 처리 ✅
- **자동 확장**: 데이터 볼륨에 따라 자동 스케일링 ✅
- **운영 부담 없음**: 서버 관리 불필요 ✅
- **S3 통합**: 저장된 원시 데이터를 직접 처리 ✅

**E. API Gateway + Kinesis + Firehose** ⭐
- **수집 확장성**: API Gateway가 수백만 디바이스 요청 처리 ✅
- **실시간 스트리밍**: Kinesis로 실시간 데이터 수집 ✅
- **자동 S3 저장**: Firehose가 자동으로 S3에 배치 전송 ✅
- **완전 서버리스**: 모든 구성요소가 관리형 서비스 ✅

### 완전한 아키텍처

```yaml
데이터 흐름:
1. 수백만 디바이스 → API Gateway (RESTful API)
2. API Gateway → Kinesis Data Stream (실시간 수집)
3. Kinesis → Firehose → S3 (원시 데이터 저장)
4. AWS Glue → S3 데이터 변환 → S3 (처리된 데이터)

특징:
- 완전 서버리스 아키텍처
- 자동 확장 (무제한)
- 운영 오버헤드 최소
- 비용 효율적 (사용량 기반)
```

### A안과 E안의 역할 분담

```yaml
E안 (데이터 수집):
- 수백만 디바이스로부터 실시간 수집
- API Gateway로 확장성 보장
- Kinesis로 스트리밍 처리
- Firehose로 S3 자동 저장

A안 (데이터 처리):
- S3의 원시 데이터를 변환
- 서버리스 ETL 작업
- 자동 스케일링
- 처리된 데이터 재저장
```
