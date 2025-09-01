## Question #2
A company needs the ability to analyze the log files of its proprietary application. 
The logs are stored in JSON format in an Amazon S3 bucket. 
Queries will be simple and will run on-demand. 
A solutions architect needs to perform the analysis with minimal changes to the existing architecture.
What should the solutions architect do to meet these requirements with the LEAST amount of operational overhead?

A. Use Amazon Redshift to load all the content into one place and run the SQL queries as needed. <br>
B. Use Amazon CloudWatch Logs to store the logs. Run SQL queries as needed from the Amazon CloudWatch console.<br>
C. Use Amazon Athena directly with Amazon S3 to run the queries as needed.<br>
D. Use AWS Glue to catalog the logs. Use a transient Apache Spark cluster on Amazon EMR to run the SQL queries as needed.<br>

### ✅ 요구사항:
- JSON 형태의 로그 파일이 Amazon S3에 저장되어 있음
- 간단한 쿼리를 온디맨드로 실행
- 기존 아키텍처에 최소한의 변경
- 최소한의 운영 오버헤드


### ✅ 선택지 분석
A. Amazon Redshift 사용
- 데이터를 S3에서 Redshift로 이동 필요 (기존 아키텍처 변경)
- 클러스터 관리 필요 (높은 운영 오버헤드)
- 지속적인 비용 발생

B. Amazon CloudWatch Logs 사용
- S3의 로그를 CloudWatch Logs로 이동 필요 (기존 아키텍처 변경)
- 제한적인 SQL 쿼리 기능
- 데이터 중복 저장으로 인한 비용 증가

**C. Amazon Athena 직접 사용** ⭐
- S3 데이터를 그대로 활용 (기존 아키텍처 변경 최소)
- 서버리스 - 인프라 관리 불필요
- 온디맨드 쿼리 - 사용한 만큼만 비용 지불
- JSON 형태 로그 분석에 최적화

D. AWS Glue + EMR 사용
- EMR 클러스터 관리 필요 (운영 오버헤드 증가)
- 간단한 쿼리에 비해 과도한 복잡성
- 클러스터 생성/삭제 시간으로 인한 지연

### ✅ 개념 정리
1. **Amazon Athena**
    - 서버리스 쿼리 서비스로 S3의 데이터를 SQL로 직접 분석
    - 인프라 관리 불필요, 쿼리 실행 시에만 비용 발생
    - JSON, CSV, Parquet 등 다양한 형태 지원
    - AWS Glue Data Catalog와 연동하여 스키마 관리

2. **Amazon Redshift**
    - 데이터 웨어하우스 서비스로 대용량 분석에 특화
    - 클러스터 기반으로 지속적인 관리 및 비용 필요
    - 복잡한 분석과 대용량 데이터 처리에 적합

3. **Amazon CloudWatch Logs**
    - 로그 수집 및 모니터링 서비스
    - 실시간 로그 스트리밍과 기본적인 검색 기능 제공
    - SQL 쿼리보다는 로그 모니터링에 최적화

4. **AWS Glue + Amazon EMR**
    - Glue: 데이터 카탈로그 및 ETL 서비스
    - EMR: Apache Spark/Hadoop 기반 빅데이터 처리 플랫폼
    - 대규모 데이터 변환 및 복잡한 분석에 적합하지만 관리 복잡성 높음
