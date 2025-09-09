## Question #225
A media company collects and analyzes user activity data on premises. 
The company wants to migrate this capability to AWS. 
The user activity data store will continue to grow and will be petabytes in size. 
The company needs to build a highly available data ingestion solution that facilitates on-demand analytics of existing data and new data with SQL.

Which solution will meet these requirements with the LEAST operational overhead?

A. Send activity data to an Amazon Kinesis data stream. Configure the stream to deliver the data to an Amazon S3 bucket.

B. Send activity data to an Amazon Kinesis Data Firehose delivery stream. Configure the stream to deliver the data to an Amazon Redshift cluster.

C. Place activity data in an Amazon S3 bucket. Configure Amazon S3 to run an AWS Lambda function on the data as the data arrives in the S3 bucket.

D. Create an ingestion service on Amazon EC2 instances that are spread across multiple Availability Zones. Configure the service to forward data to an Amazon RDS Multi-AZ database.

## Question #225 분석

### 요구사항
- **사용자 활동 데이터** 수집 및 분석
- **페타바이트급** 데이터 규모
- **고가용성** 데이터 수집
- **SQL 기반** 온디맨드 분석
- **최소 운영 오버헤드**

### 선택지 분석

**A. Kinesis Data Stream → S3**
- **데이터 수집**: 실시간 스트리밍 지원 ✅
- **SQL 분석 한계**: S3 자체는 SQL 쿼리 엔진 없음 ❌
- **추가 구성 필요**: Athena 등 별도 쿼리 서비스 필요 ❌

**B. Kinesis Data Firehose → Redshift** ⭐
- **자동 수집**: Firehose가 S3 및 Redshift로 자동 전송 ✅
- **SQL 분석**: Redshift에서 즉시 SQL 쿼리 가능 ✅
- **페타바이트 지원**: Redshift는 페타바이트급 처리 가능 ✅
- **최소 운영**: 완전 관리형 서비스로 운영 부담 최소 ✅

**C. S3 + Lambda 처리**
- **Lambda 제한**: 실행 시간 및 메모리 제한으로 대용량 부적합 ❌
- **SQL 분석 없음**: 추가 분석 도구 필요 ❌
- **확장성 문제**: 페타바이트급 처리에 부적절 ❌

**D. EC2 수집 서비스 + RDS Multi-AZ**
- **운영 부담**: EC2 관리 및 확장 설정 필요 ❌
- **RDS 한계**: 페타바이트급 분석에 부적합 ❌
- **확장성 문제**: 전통적 RDBMS는 대용량 분석 한계 ❌

### Kinesis Firehose + Redshift 장점

```yaml
데이터 수집:
- 고가용성 스트리밍
- 자동 배치 처리
- 압축 및 변환 지원

분석 능력:
- 페타바이트급 데이터웨어하우스
- 표준 SQL 지원
- 고성능 분석 쿼리
- 기존 BI 도구 연동

운영 효율성:
- 완전 관리형 서비스
- 자동 스케일링
- 백업 및 복구 자동화
```
