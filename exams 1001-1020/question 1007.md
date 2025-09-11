## Question #1007
A medical company wants to perform transformations on a large amount of clinical trial data that comes from several customers. The company must extract the data from a relational database that contains the customer data. Then the company will transform the data by using a series of complex rules. The company will load the data to Amazon S3 when the transformations are complete.

All data must be encrypted where it is processed before the company stores the data in Amazon S3. All data must be encrypted by using customer-specific keys.

Which solution will meet these requirements with the LEAST amount of operational effort?

A. Create one AWS Glue job for each customer. Attach a security configuration to each job that uses server-side encryption with Amazon S3 managed keys (SSE-S3) to encrypt the data.

B. Create one Amazon EMR cluster for each customer. Attach a security configuration to each cluster that uses client-side encryption with a custom client-side root key (CSE-Custom) to encrypt the data.

C. Create one AWS Glue job for each customer. Attach a security configuration to each job that uses client-side encryption with AWS KMS managed keys (CSE-KMS) to encrypt the data.

D. Create one Amazon EMR cluster for each customer. Attach a security configuration to each cluster that uses server-side encryption with AWS KMS keys (SSE-KMS) to encrypt the data.

## Question #1007 분석

### 문제 상황
- **대량 임상 시험 데이터** 변환
- **관계형 DB → 복잡한 변환 규칙 → S3**
- **처리 중 암호화** 필요
- **고객별 키** 사용

### 핵심 요구사항
- **ETL 작업** (Extract, Transform, Load)
- **처리 중 암호화**
- **고객별 키 분리**
- **최소 운영 노력**

### ETL 서비스 비교

**AWS Glue**: 서버리스 ETL (관리 부담 적음)
**Amazon EMR**: 클러스터 기반 (관리 부담 많음)

### 암호화 방식 비교

**Server-side 암호화**: 저장시 암호화 (S3에서)
**Client-side 암호화**: 처리 중 암호화 (애플리케이션에서)

### 선택지 분석

**A. Glue + SSE-S3**
- SSE-S3: AWS 관리 키 (고객별 키 아님) ❌
- 고객별 키 요구사항 미충족 ❌

**B. EMR + CSE-Custom**
- EMR: 클러스터 관리 부담 ❌
- 운영 노력 많음 ❌

**C. Glue + CSE-KMS** ⭐
- Glue: 서버리스 ETL (최소 운영) ✅
- CSE-KMS: 처리 중 암호화 + 고객별 키 ✅
- 요구사항 모두 충족 ✅

**D. EMR + SSE-KMS**
- EMR: 클러스터 관리 부담 ❌
- SSE-KMS: 저장시만 암호화 (처리 중 암호화 아님) ❌

### 암호화 세부사항

**CSE-KMS (Client-Side Encryption)**
```yaml
특징:
- 데이터 처리 중에도 암호화 유지
- 고객별 KMS 키 사용 가능
- Glue 작업에서 지원
```

