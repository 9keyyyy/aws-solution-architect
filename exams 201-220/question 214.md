## Question #214
A company’s reporting system delivers hundreds of .csv files to an Amazon S3 bucket each day. 
The company must convert these files to Apache Parquet format and must store the files in a transformed data bucket.

Which solution will meet these requirements with the LEAST development effort?

A. Create an Amazon EMR cluster with Apache Spark installed. Write a Spark application to transform the data. Use EMR File System (EMRFS) to write files to the transformed data bucket.

B. Create an AWS Glue crawler to discover the data. Create an AWS Glue extract, transform, and load (ETL) job to transform the data. Specify the transformed data bucket in the output step.

C. Use AWS Batch to create a job definition with Bash syntax to transform the data and output the data to the transformed data bucket. Use the job definition to submit a job. Specify an array job as the job type.

D. Create an AWS Lambda function to transform the data and output the data to the transformed data bucket. Configure an event notification for the S3 bucket. Specify the Lambda function as the destination for the event notification.

## Question #214 분석

### 요구사항
- **수백 개 CSV 파일** 일일 처리
- **Apache Parquet 포맷** 변환
- **최소 개발 노력**

### 선택지 분석

**A. EMR + Spark 애플리케이션**
- **클러스터 관리**: EMR 클러스터 설정 및 관리 필요 ❌
- **코드 개발**: Spark 애플리케이션 직접 개발 ❌
- **운영 복잡성**: 높은 개발 및 운영 부담 ❌

**B. AWS Glue Crawler + ETL Job** ⭐
- **완전 관리형**: 서버리스 ETL 서비스 ✅
- **자동 스키마 발견**: Crawler가 CSV 구조 자동 분석 ✅
- **내장 변환**: CSV → Parquet 기본 지원 ✅
- **최소 개발**: 코드 작성 없이 GUI 설정만 ✅

**C. AWS Batch + Bash 스크립트**
- **컨테이너 관리**: 작업 정의 및 컨테이너 설정 ❌
- **스크립트 개발**: Bash 변환 로직 직접 구현 ❌
- **복잡성**: 불필요한 인프라 관리 ❌

**D. Lambda 함수**
- **실행 제한**: 15분 타임아웃으로 대용량 파일 처리 어려움 ❌
- **메모리 제한**: 10GB 최대 메모리로 수백 개 파일 처리 부적합 ❌
- **코드 개발**: 변환 로직 직접 구현 필요 ❌

### AWS Glue 장점

```yaml
ETL 전용 서비스:
✅ CSV → Parquet 네이티브 지원
✅ 스키마 자동 추론
✅ 데이터 카탈로그 통합
✅ 확장성 자동 제공

개발 효율성:
✅ 시각적 ETL 편집기
✅ 코드 자동 생성
✅ 내장 변환 함수
✅ 설정 기반 구성
```

### 구현 과정

```yaml
1. Glue Crawler 생성
   - 소스 S3 버킷 지정
   - 스키마 자동 발견

2. ETL Job 생성
   - 입력: CSV 파일
   - 변환: Parquet 포맷
   - 출력: 변환된 데이터 버킷

3. 스케줄링
   - 일일 자동 실행
   - 완전 자동화
```
