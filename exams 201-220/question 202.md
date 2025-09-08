## Question #202
A company is planning to move its data to an Amazon S3 bucket. 
The data must be encrypted when it is stored in the S3 bucket. 
Additionally, the encryption key must be automatically rotated every year.

Which solution will meet these requirements with the LEAST operational overhead?

A. Move the data to the S3 bucket. Use server-side encryption with Amazon S3 managed encryption keys (SSE-S3). Use the built-in key rotation behavior of SSE-S3 encryption keys.

B. Create an AWS Key Management Service (AWS KMS) customer managed key. Enable automatic key rotation. Set the S3 bucket’s default encryption behavior to use the customer managed KMS key. Move the data to the S3 bucket.

C. Create an AWS Key Management Service (AWS KMS) customer managed key. Set the S3 bucket’s default encryption behavior to use the customer managed KMS key. Move the data to the S3 bucket. Manually rotate the KMS key every year.

D. Encrypt the data with customer key material before moving the data to the S3 bucket. Create an AWS Key Management Service (AWS KMS) key without key material. Import the customer key material into the KMS key. Enable automatic key rotation.

## Question #202 분석

### 요구사항
- **S3 데이터 암호화** 필수
- **연간 자동 키 로테이션**
- **최소 운영 오버헤드**

### 선택지 분석

**A. SSE-S3 + 내장 키 로테이션** ⭐
- **자동 암호화**: S3 관리형 키로 투명한 암호화 ✅
- **자동 로테이션**: SSE-S3 키는 정기적으로 자동 로테이션 ✅
- **운영 부담 없음**: 완전 AWS 관리, 설정 후 자동 ✅
- **추가 비용 없음**: 기본 S3 암호화 기능 ✅

**B. KMS 고객 관리형 키 + 자동 로테이션**
- **자동 로테이션**: KMS 자동 로테이션 지원 ✅
- **추가 복잡성**: 고객 관리형 키 생성 및 관리 필요 ❌
- **추가 비용**: KMS 키 사용료 발생 ❌
- **불필요한 복잡성**: 요구사항 대비 오버엔지니어링 ❌

**C. KMS 고객 관리형 키 + 수동 로테이션**
- **수동 작업**: 매년 수동 키 로테이션 필요 ❌
- **운영 부담**: 정기적 수동 개입 필요 ❌
- **최소 오버헤드 위반**: 수동 작업 요구 ❌

**D. 고객 키 재료 + KMS**
- **복잡한 구현**: 외부 키 재료 관리 필요 ❌
- **자동 로테이션 불가**: 외부 키 재료는 자동 로테이션 미지원 ❌
- **높은 운영 부담**: 키 재료 생성 및 관리 복잡 ❌

### SSE-S3 vs KMS 비교

```yaml
SSE-S3:
운영: 완전 자동, 설정 불필요
비용: 추가 비용 없음
로테이션: 자동 (AWS 관리)
복잡성: 최소

KMS 고객 관리형:
운영: 키 정책 및 권한 관리
비용: 키 사용료 + API 호출료
로테이션: 설정 가능한 자동
복잡성: 중간
```

