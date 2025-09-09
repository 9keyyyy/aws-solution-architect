## Question #257
A company is building a solution that will report Amazon EC2 Auto Scaling events across all the applications in an AWS account. 
The company needs to use a serverless solution to store the EC2 Auto Scaling status data in Amazon S3. 
The company then will use the data in Amazon S3 to provide near-real-time updates in a dashboard. 
The solution must not affect the speed of EC2 instance launches.

How should the company move the data to Amazon S3 to meet these requirements?

A. Use an Amazon CloudWatch metric stream to send the EC2 Auto Scaling status data to Amazon Kinesis Data Firehose. Store the data in Amazon S3.

B. Launch an Amazon EMR cluster to collect the EC2 Auto Scaling status data and send the data to Amazon Kinesis Data Firehose. Store the data in Amazon S3.

C. Create an Amazon EventBridge rule to invoke an AWS Lambda function on a schedule. Configure the Lambda function to send the EC2 Auto Scaling status data directly to Amazon S3.

D. Use a bootstrap script during the launch of an EC2 instance to install Amazon Kinesis Agent. Configure Kinesis Agent to collect the EC2 Auto Scaling status data and send the data to Amazon Kinesis Data Firehose. Store the data in Amazon S3.

## Question #257 분석

### 요구사항
- **EC2 Auto Scaling 이벤트** 보고 (계정 전체)
- **서버리스 솔루션**으로 S3 저장
- **준실시간 대시보드** 업데이트
- **EC2 인스턴스 시작 속도에 영향 없음**

### 선택지 분석

**A. CloudWatch Metric Stream + Kinesis Data Firehose** ⭐
- **서버리스**: 완전 관리형 서비스로 서버 관리 불필요 ✅
- **Auto Scaling 이벤트 캡처**: CloudWatch가 Auto Scaling 메트릭 자동 수집 ✅
- **준실시간**: Kinesis Firehose로 S3에 실시간 전송 ✅
- **인스턴스 시작 영향 없음**: 별도 스트림으로 성능 영향 없음 ✅

**B. EMR 클러스터 + Kinesis Data Firehose**
- **서버리스 아님**: EMR 클러스터 관리 필요 ❌
- **오버킬**: 단순 데이터 수집에 빅데이터 처리 클러스터 부적절 ❌
- **비용 비효율**: 지속적인 클러스터 운영 비용 ❌

**C. EventBridge + Lambda (스케줄 기반)**
- **스케줄 기반 한계**: 실시간성 부족, 이벤트 누락 가능성 ❌
- **복잡성**: Auto Scaling 상태 데이터 수집 로직 직접 구현 필요 ❌
- **지연 발생**: 스케줄 간격에 따른 데이터 지연 ❌

**D. 부트스트랩 스크립트 + Kinesis Agent**
- **인스턴스 시작 영향**: 부트스트랩 스크립트로 시작 시간 지연 ❌
- **요구사항 위반**: EC2 시작 속도에 직접적 영향 ❌
- **복잡한 관리**: 각 인스턴스별 Agent 설치 및 관리 필요 ❌

### CloudWatch Metric Stream 솔루션

```yaml
데이터 흐름:
1. Auto Scaling Group 이벤트 발생
2. CloudWatch가 메트릭 자동 수집
3. Metric Stream이 실시간으로 Kinesis Data Firehose 전송
4. Firehose가 S3에 배치 저장
5. 대시보드가 S3 데이터 기반으로 업데이트

장점:
- 완전 서버리스
- 실시간 데이터 스트리밍
- Auto Scaling 네이티브 통합
- 인스턴스 성능 영향 없음
```

