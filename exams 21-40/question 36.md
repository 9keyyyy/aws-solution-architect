## Question #36
A company is building an application in the AWS Cloud. 
The application will store data in Amazon S3 buckets in two AWS Regions. 
The company must use an AWS Key Management Service (AWS KMS) customer managed key to encrypt all data that is stored in the S3 buckets. 
The data in both S3 buckets must be encrypted and decrypted with the same KMS key. 
The data and the key must be stored in each of the two Regions.

Which solution will meet these requirements with the LEAST operational overhead?

A. Create an S3 bucket in each Region. Configure the S3 buckets to use server-side encryption with Amazon S3 managed encryption keys (SSE-S3). Configure replication between the S3 buckets.

B. Create a customer managed multi-Region KMS key. Create an S3 bucket in each Region. Configure replication between the S3 buckets. Configure the application to use the KMS key with client-side encryption.

C. Create a customer managed KMS key and an S3 bucket in each Region. Configure the S3 buckets to use server-side encryption with Amazon S3 managed encryption keys (SSE-S3). Configure replication between the S3 buckets.

D. Create a customer managed KMS key and an S3 bucket in each Region. Configure the S3 buckets to use server-side encryption with AWS KMS keys (SSE-KMS). Configure replication between the S3 buckets.

## Question #36 분석

### ✅ 요구사항
- **두 AWS 리전**에 S3 버킷
- **AWS KMS 고객 관리형 키** 사용 필수
- **동일한 KMS 키**로 모든 데이터 암/복호화
- **데이터와 키** 모두 각 리전에 저장
- **최소 운영 오버헤드**

### ✅ 선택지 분석

**A. SSE-S3 + 복제**
- **SSE-S3**: S3 관리형 키 사용 
- **KMS 고객 관리형 키** 요구사항 위배
- 가장 기본적인 암호화 방식

**B. Multi-Region KMS 키 + 클라이언트 사이드 암호화** ⭐
- **Multi-Region 키**: 동일 키 ID로 두 리전 사용 ✅
- **클라이언트 사이드**: 애플리케이션에서 암호화 ✅
- **최소 운영**: 키 복제 자동화 ✅

**C. 리전별 개별 KMS 키 + SSE-S3**
- **개별 KMS 키**: 동일 키 사용 요구사항 위배 
- **SSE-S3**: KMS 키 사용 불가 
- 요구사항 완전 위배

**D. 리전별 개별 KMS 키 + SSE-KMS**
- **개별 KMS 키**: 동일 키 사용 요구사항 위배 
- **높은 운영 부담**: 키 동기화 수동 관리 필요 

### 📋 Multi-Region KMS 키 이해

#### **Multi-Region KMS 키 특징**
```yaml
핵심 개념:
  - 물리적으로는 각 리전에 별도 키 존재
  - 논리적으로는 동일한 키 ID 및 키 자료
  - 자동 복제 및 동기화
  - 각 리전에서 독립적 사용 가능

장점:
  ✅ 동일 키 ID로 여러 리전 접근
  ✅ 자동 키 복제 (운영 오버헤드 최소)
  ✅ 리전별 독립적 성능
  ✅ 재해 복구 시 키 가용성
```

#### **Multi-Region 키 작동 방식**
```yaml
키 생성:
  1. Primary Region (us-east-1)에서 Multi-Region 키 생성
  2. 키 ID: arn:aws:kms:*:123456789012:key/12345678-1234-1234-1234-123456789012
  3. Replica Region (eu-west-1)에 자동 복제
  4. 두 리전에서 동일한 키 자료 사용

암호화/복호화:
  - us-east-1: 로컬 키로 암호화/복호화
  - eu-west-1: 로컬 복제본으로 암호화/복호화  
  - 데이터는 리전 간 호환 가능
```

### 🔄 클라이언트 사이드 vs 서버 사이드 암호화

#### **클라이언트 사이드 암호화 (B번)**
```yaml
처리 과정:
  1. 애플리케이션이 KMS에서 데이터 키 요청
  2. 애플리케이션이 직접 데이터 암호화
  3. 암호화된 데이터를 S3에 저장
  4. 복호화 시 애플리케이션이 KMS 호출 후 직접 처리

장점:
  ✅ 전송 중/저장 중 모든 단계에서 암호화
  ✅ S3도 암호화된 데이터만 보관
  ✅ 완전한 End-to-End 암호화
  ✅ Multi-Region 키와 완벽 호환
```

#### **서버 사이드 암호화 문제점**
```yaml
SSE-KMS (D번) 문제:
  - 리전별 개별 KMS 키 필요
  - 키 동기화 수동 관리
  - 리전 간 데이터 호환성 문제
  - 높은 운영 복잡성

SSE-S3 (A, C번) 문제:
  - S3 관리형 키 사용 (고객 관리형 아님)
  - 요구사항 위배
```

### 📊 실제 구현 시나리오

#### **Multi-Region 키 + 클라이언트 암호화 (B번)**
```yaml
아키텍처:
  애플리케이션 (us-east-1)
    ↓ KMS Multi-Region 키 사용
  암호화된 데이터 → S3 (us-east-1)
    ↓ 복제
  암호화된 데이터 → S3 (eu-west-1)
    ↑ 동일한 KMS 키로 복호화 가능
  애플리케이션 (eu-west-1)

데이터 흐름:
  1. us-east-1 앱이 데이터 암호화 후 S3 저장
  2. S3 복제로 eu-west-1에 동일 데이터 복사
  3. eu-west-1 앱이 동일 키로 복호화 성공
```

#### **개별 KMS 키 방식 (D번) 문제점**
```yaml
복잡한 키 관리:
  1. us-east-1에 KMS 키 생성
  2. eu-west-1에 별도 KMS 키 생성  
  3. 두 키를 수동으로 동기화 유지
  4. 키 로테이션 시 양쪽 모두 관리
  5. 권한 정책 두 번 설정

운영 부담:
  ❌ 키 정책 일관성 관리
  ❌ 키 로테이션 동기화
  ❌ 액세스 권한 중복 관리
  ❌ 장애 시 키 복구 복잡성
```

### 🔍 암호화 방식별 세부 비교

#### **SSE-S3 vs SSE-KMS vs 클라이언트 사이드**
```yaml
SSE-S3:
  - 키 관리: AWS S3 자동
  - 암호화 위치: S3 서버
  - 키 제어권: AWS
  - 요구사항: 고객 관리형 키 ❌

SSE-KMS:
  - 키 관리: AWS KMS
  - 암호화 위치: S3 서버  
  - 키 제어권: 고객 ✅
  - Multi-Region: 복잡한 설정 ❌

클라이언트 사이드:
  - 키 관리: AWS KMS
  - 암호화 위치: 애플리케이션
  - 키 제어권: 고객 ✅
  - Multi-Region: 완벽 지원 ✅
```

#### **운영 오버헤드 비교**
```yaml
B번 (Multi-Region + 클라이언트):
  초기 설정: Multi-Region 키 생성 (1회)
  지속 운영: 자동 복제 (0시간)
  키 관리: 중앙집중식 (최소)

D번 (개별 키 + SSE-KMS):
  초기 설정: 리전별 키 생성 (2회)
  지속 운영: 수동 동기화 (주간)
  키 관리: 분산 관리 (복잡)
```
