## Question #994
company's IT security guidelines mandate that the database credentials be encrypted and rotated every 14 days.

What should a solutions architect do to meet this requirement with the LEAST operational effort?

A. Create a new AWS Key Management Service (AWS KMS) encryption key. Use AWS Secrets Manager to create a new secret that uses the KMS key with the appropriate credentials. Associate the secret with the Aurora DB cluster. Configure a custom rotation period of 14 days.

B. Create two parameters in AWS Systems Manager Parameter Store: one for the user name as a string parameter and one that uses the SecureString type for the password. Select AWS Key Management Service (AWS KMS) encryption for the password parameter, and load these parameters in the application tier. Implement an AWS Lambda function that rotates the password every 14 days.

C. Store a file that contains the credentials in an AWS Key Management Service (AWS KMS) encrypted Amazon Elastic File System (Amazon EFS) file system. Mount the EFS file system in all EC2 instances of the application tier. Restrict the access to the file on the file system so that the application can read the file and that only super users can modify the file. Implement an AWS Lambda function that rotates the key in Aurora every 14 days and writes new credentials into the file.

D. Store a file that contains the credentials in an AWS Key Management Service (AWS KMS) encrypted Amazon S3 bucket that the application uses to load the credentials. Download the file to the application regularly to ensure that the correct credentials are used. Implement an AWS Lambda function that rotates the Aurora credentials every 14 days and uploads these credentials to the file in the S3 bucket.

## Question #994 분석

### 문제 상황
- **DB 자격증명 암호화** 필요
- **14일마다 로테이션** 필요
- **최소 운영 노력** 요구

### 핵심 요구사항
- **자격증명 암호화**
- **자동 14일 로테이션**
- **최소 관리 부담**

### 선택지 분석

**A. KMS + Secrets Manager + 자동 로테이션** ⭐
- Secrets Manager: 자격증명 전용 관리 서비스 ✅
- 자동 로테이션 내장 기능 ✅
- Aurora 네이티브 통합 ✅
- 14일 커스텀 주기 설정 가능 ✅
- 최소 운영 노력 ✅

**B. Parameter Store + Lambda 로테이션**
- Parameter Store: 자격증명보다 구성 관리용 ❌
- Lambda 수동 구현 → 운영 부담 증가 ❌
- 로테이션 로직 직접 개발 필요 ❌

**C. EFS + Lambda 로테이션**
- 파일 시스템 기반 → 복잡성 증가 ❌
- 파일 권한 관리 필요 ❌
- Lambda 수동 구현 ❌

**D. S3 + Lambda 로테이션**
- 파일 기반 자격증명 관리 → 비효율 ❌
- 정기 다운로드 필요 ❌
- Lambda 수동 구현 ❌

### Secrets Manager vs Parameter Store

| 구분 | Secrets Manager | Parameter Store |
|------|-----------------|-----------------|
| **목적** | 자격증명 전용 | 구성 관리 |
| **로테이션** | 자동 지원 | 수동 구현 |
| **DB 통합** | 네이티브 | 제한적 |
| **비용** | 높음 | 낮음 |

### Secrets Manager 자동 로테이션

```yaml
기능:
- Aurora/RDS 네이티브 통합
- 자동 로테이션 Lambda 제공
- 무중단 자격증명 교체
- 애플리케이션 투명성

설정:
- 로테이션 주기: 14일
- 자동 실행: AWS 관리
- 에러 처리: 자동 롤백
```

### 운영 노력 비교

```yaml
Option A (Secrets Manager):
- 초기 설정만 필요
- 이후 완전 자동화
- 에러 처리 자동

Other Options:
- Lambda 함수 개발/유지보수
- 에러 처리 직접 구현
- 모니터링 설정 필요
```
