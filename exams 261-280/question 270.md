## Question #270
A company is using a centralized AWS account to store log data in various Amazon S3 buckets. 
A solutions architect needs to ensure that the data is encrypted at rest before the data is uploaded to the S3 buckets. 
The data also must be encrypted in transit.

Which solution meets these requirements?

A. Use client-side encryption to encrypt the data that is being uploaded to the S3 buckets.

B. Use server-side encryption to encrypt the data that is being uploaded to the S3 buckets.

C. Create bucket policies that require the use of server-side encryption with S3 managed encryption keys (SSE-S3) for S3 uploads.

D. Enable the security option to encrypt the S3 buckets through the use of a default AWS Key Management Service (AWS KMS) key.

## Question #270 분석

### 요구사항
- **S3 버킷**에 로그 데이터 저장
- **업로드 전 저장 시 암호화** 필요
- **전송 중 암호화** 필요

### 암호화 유형 구분

**저장 시 암호화 (Encryption at Rest):**
- 데이터가 S3에 저장될 때 암호화
- 클라이언트 사이드 또는 서버 사이드 암호화

**전송 중 암호화 (Encryption in Transit):**
- 데이터 전송 과정에서 암호화
- HTTPS/TLS 프로토콜 사용

### 선택지 분석

**A. 클라이언트 사이드 암호화** ⭐
- **저장 시 암호화**: 클라이언트에서 암호화 후 업로드하므로 저장 전 암호화 ✅
- **전송 중 암호화**: HTTPS 사용으로 전송 중에도 암호화 ✅
- **업로드 전 암호화**: 요구사항에 정확히 부합 ✅

**B. 서버 사이드 암호화**
- **저장 시 암호화**: S3에서 수신 후 암호화 ✅
- **업로드 전 암호화 아님**: 업로드 과정에서는 평문, S3 도착 후 암호화 ❌

**C. SSE-S3 버킷 정책**
- **동일한 서버 사이드 문제**: 업로드 후 암호화 ❌
- **정책 강제**: 암호화 강제하지만 타이밍 문제 동일 ❌

**D. KMS 기본 키 암호화**
- **서버 사이드 방식**: B, C와 동일한 문제 ❌
- **업로드 후 암호화**: 요구사항 미충족 ❌

### 핵심 차이점

```yaml
문제의 핵심: "업로드 전 암호화"

클라이언트 사이드:
데이터 → 클라이언트 암호화 → HTTPS 전송 → S3 저장

서버 사이드:
데이터 → HTTPS 전송 → S3 수신 → S3 암호화 → 저장
```

