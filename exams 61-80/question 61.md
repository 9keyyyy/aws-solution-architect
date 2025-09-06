## Question #61
A company is developing a two-tier web application on AWS. 
The company's developers have deployed the application on an Amazon EC2 instance that connects directly to a backend Amazon RDS database. 
The company must not hardcode database credentials in the application. 
The company must also implement a solution to automatically rotate the database credentials on a regular basis.

Which solution will meet these requirements with the LEAST operational overhead?

A. Store the database credentials in the instance metadata. Use Amazon EventBridge (Amazon CloudWatch Events) rules to run a scheduled AWS Lambda function that updates the RDS credentials and instance metadata at the same time.

B. Store the database credentials in a configuration file in an encrypted Amazon S3 bucket. Use Amazon EventBridge (Amazon CloudWatch Events) rules to run a scheduled AWS Lambda function that updates the RDS credentials and the credentials in the configuration file at the same time. Use S3 Versioning to ensure the ability to fall back to previous values.

C. Store the database credentials as a secret in AWS Secrets Manager. Turn on automatic rotation for the secret. Attach the required permission to the EC2 role to grant access to the secret.

D. Store the database credentials as encrypted parameters in AWS Systems Manager Parameter Store. Turn on automatic rotation for the encrypted parameters. Attach the required permission to the EC2 role to grant access to the encrypted parameters.

## Question #61 분석

### ✅ 요구사항
- **2계층 웹 애플리케이션** (EC2 + RDS)
- **데이터베이스 자격 증명 하드코딩 금지**
- **자격 증명 자동 순환** (정기적)
- **최소 운영 오버헤드** 요구

### ✅ 선택지 분석

**A. 인스턴스 메타데이터 + EventBridge + Lambda**
- **인스턴스 메타데이터**: 보안에 부적합한 저장소 ❌
- **메타데이터 접근**: HTTP 요청으로 누구나 접근 가능 ❌
- **수동 구현**: Lambda로 직접 로테이션 로직 구현 ❌
- **운영 오버헤드**: 커스텀 코드 개발 및 유지보수 ❌

**B. S3 암호화 + EventBridge + Lambda + S3 Versioning**
- **S3 구성 파일**: 애플리케이션이 S3 API 호출 필요 ⚠️
- **수동 구현**: 복잡한 Lambda 로테이션 로직 ❌
- **동기화 문제**: RDS와 S3 파일 동시 업데이트 복잡 ❌
- **높은 운영 부담**: 여러 서비스 조율 및 오류 처리 ❌

**C. AWS Secrets Manager + 자동 로테이션** ⭐
- **전용 서비스**: 자격 증명 관리 특화 서비스 ✅
- **자동 로테이션**: 내장 기능으로 RDS 지원 ✅
- **제로 다운타임**: 무중단 로테이션 지원 ✅
- **최소 운영**: 설정만으로 완전 자동화 ✅
- **보안**: 암호화 및 세밀한 액세스 제어 ✅

**D. Systems Manager Parameter Store + 자동 로테이션**
- **Parameter Store**: 구성 관리용, 자격 증명에 제한적 ⚠️
- **로테이션 기능 없음**: 자동 로테이션 지원 안 함 ❌
- **수동 구현**: 별도 로테이션 로직 필요 ❌
- **RDS 통합**: 데이터베이스별 로테이션 지원 부족 ❌

### 📋 핵심 개념 정리

#### **AWS Secrets Manager 특징**
```yaml
자격 증명 전용 서비스:
  ✅ 데이터베이스 자격 증명 특화
  ✅ RDS, DocumentDB, Redshift 지원
  ✅ 다중 사용자 자격 증명 관리

자동 로테이션:
  ✅ 내장 로테이션 기능
  ✅ RDS 네이티브 통합
  ✅ 무중단 로테이션
  ✅ 커스텀 Lambda 함수 지원

보안:
  ✅ 전송/저장 시 암호화
  ✅ IAM 기반 액세스 제어
  ✅ VPC 엔드포인트 지원
  ✅ CloudTrail 로깅
```

#### **Parameter Store vs Secrets Manager**
```yaml
Parameter Store:
  ✅ 구성 데이터 관리
  ✅ 계층적 구조
  ✅ 무료 티어 (표준)
  ❌ 자동 로테이션 미지원
  ❌ 자격 증명 특화 기능 부족

Secrets Manager:
  ✅ 자격 증명 전용
  ✅ 자동 로테이션 내장
  ✅ 데이터베이스 통합
  💰 사용량 기반 과금
  ✅ 엔터프라이즈 기능
```

#### **정답 시나리오 (C번)**
```yaml
1. Secrets Manager에 RDS 자격 증명 저장
   - 데이터베이스 유형: MySQL/PostgreSQL 등
   - 호스트, 포트, 사용자명, 비밀번호
   
2. 자동 로테이션 활성화
   - 로테이션 주기: 30일 (권장)
   - 로테이션 Lambda: AWS 제공 템플릿
   - RDS와 자동 연동
   
3. EC2 IAM 역할 설정
   - secretsmanager:GetSecretValue 권한
   - 특정 시크릿에 대한 액세스
   
4. 애플리케이션 코드 수정
   - AWS SDK로 시크릿 값 조회
   - 자동 캐싱 및 새로고침
   
5. 자동 로테이션 프로세스
   - 새 자격 증명 생성
   - RDS 사용자 업데이트
   - 연결 테스트
   - 이전 자격 증명 비활성화

장점:
  ✅ 완전 자동화 (Zero Touch)
  ✅ RDS 네이티브 통합
  ✅ 무중단 로테이션
  ✅ 보안 모범 사례
  ✅ 운영 부담 최소
```

