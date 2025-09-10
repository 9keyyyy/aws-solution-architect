## Question #267
A company has one million users that use its mobile app. 
The company must analyze the data usage in near-real time. 
The company also must encrypt the data in near-real time and must store the data in a centralized location in Apache Parquet format for further processing.

Which solution will meet these requirements with the LEAST operational overhead?

A. Create an Amazon Kinesis data stream to store the data in Amazon S3. Create an Amazon Kinesis Data Analytics application to analyze the data. Invoke an AWS Lambda function to send the data to the Kinesis Data Analytics application.

B. Create an Amazon Kinesis data stream to store the data in Amazon S3. Create an Amazon EMR cluster to analyze the data. Invoke an AWS Lambda function to send the data to the EMR cluster.

C. Create an Amazon Kinesis Data Firehose delivery stream to store the data in Amazon S3. Create an Amazon EMR cluster to analyze the data.

D. Create an Amazon Kinesis Data Firehose delivery stream to store the data in Amazon S3. Create an Amazon Kinesis Data Analytics application to analyze the data.

## Question #267 분석

### 요구사항
- **100만 사용자** 모바일 앱 데이터
- **준실시간 분석** 필요
- **준실시간 암호화** 필요
- **Apache Parquet 포맷**으로 중앙 저장
- **최소 운영 오버헤드**

### 선택지 분석

**A. Kinesis Data Stream + S3 + Kinesis Analytics + Lambda**
- **복잡한 아키텍처**: 여러 서비스 조합으로 복잡성 증가 ❌
- **Lambda 오버헤드**: 불필요한 중간 처리 단계 ❌
- **운영 부담**: Data Stream 샤드 관리 필요 ❌

**B. Kinesis Data Stream + S3 + EMR + Lambda**
- **EMR 관리 부담**: 클러스터 프로비저닝 및 관리 필요 ❌
- **높은 운영 오버헤드**: EMR 운영 복잡성 ❌
- **동일한 Lambda 문제**: 불필요한 중간 단계 ❌

**C. Kinesis Data Firehose + S3 + EMR**
- **EMR 운영 부담**: 여전히 클러스터 관리 필요 ❌
- **Firehose 장점**: 자동 S3 전송 및 Parquet 변환 ✅

**D. Kinesis Data Firehose + S3 + Kinesis Analytics** ⭐
- **완전 관리형**: 모든 구성요소가 서버리스/관리형 ✅
- **자동 Parquet 변환**: Firehose가 자동으로 Parquet 포맷 저장 ✅
- **내장 암호화**: Firehose와 S3 암호화 자동 지원 ✅
- **준실시간 분석**: Kinesis Analytics로 스트림 데이터 실시간 분석 ✅
- **최소 운영**: 서버 관리, 샤드 관리 불필요 ✅

### Firehose + Analytics 조합 장점

```yaml
데이터 흐름:
모바일 앱 → Kinesis Data Firehose → S3 (Parquet)
           ↓
    Kinesis Analytics (준실시간 분석)

운영 효율성:
- 자동 스케일링
- 자동 포맷 변환
- 내장 압축 및 암호화
- 서버 관리 불필요
```
