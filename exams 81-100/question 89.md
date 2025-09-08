## Question #89
A company uses Amazon S3 to store its confidential audit documents. 
The S3 bucket uses bucket policies to restrict access to audit team IAM user credentials according to the principle of least privilege. 
Company managers are worried about accidental deletion of documents in the S3 bucket and want a more secure solution.

What should a solutions architect do to secure the audit documents?

A. Enable the versioning and MFA Delete features on the S3 bucket.

B. Enable multi-factor authentication (MFA) on the IAM user credentials for each audit team IAM user account.

C. Add an S3 Lifecycle policy to the audit team's IAM user accounts to deny the s3:DeleteObject action during audit dates.

D. Use AWS Key Management Service (AWS KMS) to encrypt the S3 bucket and restrict audit team IAM user accounts from accessing the KMS key.

## Question #89 분석

### ✅ 요구사항
- **기밀 감사 문서** S3 저장
- **실수로 인한 삭제 방지** 강화
- **더 안전한 솔루션** 구현
- **최소 권한 원칙** 유지

### ✅ 선택지 분석

**A. Versioning + MFA Delete 활성화** ⭐
- **Versioning**: 삭제된 파일 복구 가능 ✅
- **MFA Delete**: 삭제 시 추가 인증 요구 ✅
- **실수 방지**: 우발적 삭제로부터 이중 보호 ✅
- **감사 요구사항**: 규제 환경에 적합 ✅

**B. IAM 사용자 MFA 활성화**
- **로그인 보안**: 계정 접근 보안 강화 ✅
- **삭제 방지 한계**: 로그인 후 삭제는 여전히 가능 ❌
- **부분적 해결**: 실수 삭제 근본 해결 안됨 ❌

**C. Lifecycle 정책으로 삭제 거부**
- **정책 오해**: Lifecycle은 s3:DeleteObject 거부 불가 ❌
- **기술적 불가능**: Lifecycle != IAM 정책 ❌
- **잘못된 접근**: 서비스 기능 오해 ❌

**D. KMS 암호화 + 키 접근 제한**
- **암호화 강화**: 데이터 보호 향상 ✅
- **삭제 방지 한계**: 암호화는 삭제 방지와 무관 ❌
- **접근 불가**: 키 제한 시 감사팀 업무 불가 ❌

### 📋 MFA Delete 핵심 기능

```yaml
MFA Delete 요구사항:
- 버전 삭제 시 MFA 토큰 필수
- 버전 영구 삭제 시 MFA 토큰 필수
- Root 계정만 MFA Delete 설정 가능

보호 효과:
✅ 우발적 삭제 방지
✅ 악의적 삭제 차단
✅ 규제 요구사항 충족
✅ 감사 추적 강화
```

### 💡 A안 구현 방식

```bash
# Versioning 활성화
aws s3api put-bucket-versioning \
    --bucket audit-documents \
    --versioning-configuration Status=Enabled

# MFA Delete 활성화 (Root 계정 필요)
aws s3api put-bucket-versioning \
    --bucket audit-documents \
    --versioning-configuration Status=Enabled,MFADelete=Enabled \
    --mfa "arn:aws:iam::account:mfa/root-account-mfa-device 123456"
```

### 📊 보안 수준 비교

```yaml
현재: 버킷 정책만
위험: 실수 삭제 가능

A안: Versioning + MFA Delete
보호: 이중 안전장치

B안: IAM MFA
보호: 로그인 단계만

C안: 기술적 불가능

D안: 암호화만
보호: 삭제와 무관
```
