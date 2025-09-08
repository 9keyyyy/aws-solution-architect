## Question #94
A company is designing an application where users upload small files into Amazon S3. 
After a user uploads a file, the file requires one-time simple processing to transform the data and save the data in JSON format for later analysis.
Each file must be processed as quickly as possible after it is uploaded. 
Demand will vary. 
On some days, users will upload a high number of files. 
On other days, users will upload a few files or no files.

Which solution meets these requirements with the LEAST operational overhead?

A. Configure Amazon EMR to read text files from Amazon S3. Run processing scripts to transform the data. Store the resulting JSON file in an Amazon Aurora DB cluster.

B. Configure Amazon S3 to send an event notification to an Amazon Simple Queue Service (Amazon SQS) queue. Use Amazon EC2 instances to read from the queue and process the data. Store the resulting JSON file in Amazon DynamoDB.

C. Configure Amazon S3 to send an event notification to an Amazon Simple Queue Service (Amazon SQS) queue. Use an AWS Lambda function to read from the queue and process the data. Store the resulting JSON file in Amazon DynamoDB.

D. Configure Amazon EventBridge (Amazon CloudWatch Events) to send an event to Amazon Kinesis Data Streams when a new file is uploaded. Use an AWS Lambda function to consume the event from the stream and process the data. Store the resulting JSON file in an Amazon Aurora DB cluster.

## Question #94 분석

### ✅ 요구사항
- **소규모 파일** S3 업로드
- **일회성 간단 처리** → JSON 변환
- **최대한 빠른 처리** 필요
- **가변적 수요** (높음/낮음/없음)
- **최소 운영 오버헤드**

### ✅ 선택지 분석

**A. EMR + Aurora**
- **EMR 오버킬**: 소규모 파일에 클러스터 불필요 ❌
- **높은 비용**: 상시 운영 클러스터 비용 ❌
- **복잡성**: 클러스터 관리 부담 ❌

**B. S3 → SQS → EC2 → DynamoDB**
- **EC2 관리**: 서버 관리 오버헤드 ❌
- **확장성**: 수동 Auto Scaling 설정 필요 ❌
- **비용**: 미사용 시에도 인스턴스 비용 ❌

**C. S3 → SQS → Lambda → DynamoDB** ⭐
- **서버리스**: 완전 관리형, 운영 부담 없음 ✅
- **자동 확장**: 파일 수에 따라 자동 처리 ✅
- **비용 효율**: 사용량 기반 과금 ✅
- **빠른 처리**: 이벤트 기반 즉시 실행 ✅

**D. EventBridge → Kinesis → Lambda → Aurora**
- **불필요한 복잡성**: EventBridge + Kinesis 오버엔지니어링 ❌
- **지연 증가**: 다단계 처리로 속도 저하 ❌
- **Aurora 오버킬**: 간단한 JSON 저장에 부적절 ❌

### 📋 C안 아키텍처

```yaml
흐름:
1. 파일 업로드 → S3
2. S3 Event → SQS 큐
3. SQS → Lambda 트리거
4. Lambda 처리 → DynamoDB 저장

특징:
✅ 완전 서버리스
✅ 자동 확장
✅ 이벤트 기반 즉시 처리
✅ 최소 운영 부담
```

### 💰 운영 오버헤드 비교

```yaml
A안: EMR 클러스터 관리 + Aurora 운영
B안: EC2 관리 + Auto Scaling 설정
C안: 설정 후 완전 자동 운영
D안: 다중 서비스 통합 관리

C안이 가장 낮은 운영 부담
```
