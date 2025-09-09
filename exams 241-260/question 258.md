## Question #258
A company has an application that places hundreds of .csv files into an Amazon S3 bucket every hour. 
The files are 1 GB in size. 
Each time a file is uploaded, the company needs to convert the file to Apache Parquet format and place the output file into an S3 bucket.

Which solution will meet these requirements with the LEAST operational overhead?

A. Create an AWS Lambda function to download the .csv files, convert the files to Parquet format, and place the output files in an S3 bucket. Invoke the Lambda function for each S3 PUT event.

B. Create an Apache Spark job to read the .csv files, convert the files to Parquet format, and place the output files in an S3 bucket. Create an AWS Lambda function for each S3 PUT event to invoke the Spark job.

C. Create an AWS Glue table and an AWS Glue crawler for the S3 bucket where the application places the .csv files. Schedule an AWS Lambda function to periodically use Amazon Athena to query the AWS Glue table, convert the query results into Parquet format, and place the output files into an S3 bucket.

D. Create an AWS Glue extract, transform, and load (ETL) job to convert the .csv files to Parquet format and place the output files into an S3 bucket. Create an AWS Lambda function for each S3 PUT event to invoke the ETL job.

## Question #258 분석

### 요구사항
- **시간당 수백 개** 1GB CSV 파일 업로드
- **Apache Parquet 포맷 변환** 필요
- **최소 운영 오버헤드**

### 선택지 분석

**A. Lambda 함수로 CSV 변환**
- **크기 제한**: Lambda는 최대 15분 실행, 1GB 파일 처리에 부적합 ❌
- **메모리 제한**: 10GB 최대 메모리로 대용량 파일 처리 어려움 ❌
- **타임아웃 위험**: 복잡한 변환 작업에 시간 초과 가능성 높음 ❌

**B. Spark Job + Lambda 트리거**
- **복잡성**: Spark 클러스터 관리 및 운영 필요 ❌
- **운영 부담**: 클러스터 설정, 모니터링, 확장 관리 ❌
- **비용**: 지속적인 컴퓨팅 리소스 필요 ❌

**C. Glue Crawler + Athena 쿼리 (스케줄 기반)**
- **실시간성 부족**: 스케줄 기반으로 즉시 변환 불가 ❌
- **복잡한 워크플로우**: 여러 서비스 조합으로 복잡성 증가 ❌
- **지연 발생**: 파일 업로드와 변환 간 시간 지연 ❌

**D. AWS Glue ETL Job + Lambda 트리거** ⭐
- **대용량 처리**: Glue는 1GB+ 파일 처리에 최적화 ✅
- **CSV → Parquet 네이티브**: 내장 변환 기능으로 최적 성능 ✅
- **완전 관리형**: 서버 관리 없이 자동 확장 ✅
- **이벤트 기반**: S3 PUT 이벤트로 즉시 변환 시작 ✅

### Glue ETL의 장점

```yaml
대용량 파일 처리:
- Apache Spark 기반 분산 처리
- 자동 스케일링
- 메모리 효율적 처리

CSV → Parquet 최적화:
- 내장 데이터 포맷 변환
- 스키마 자동 추론
- 압축 및 최적화 자동 적용

운영 효율성:
- 서버리스 실행
- 자동 리소스 관리
- 모니터링 내장
```
