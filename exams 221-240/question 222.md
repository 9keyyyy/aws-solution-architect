## Question #222
A company has hired an external vendor to perform work in the company’s AWS account. 
The vendor uses an automated tool that is hosted in an AWS account that the vendor owns. 
The vendor does not have IAM access to the company’s AWS account.

How should a solutions architect grant this access to the vendor?

A. Create an IAM role in the company’s account to delegate access to the vendor’s IAM role. Attach the appropriate IAM policies to the role for the permissions that the vendor requires.

B. Create an IAM user in the company’s account with a password that meets the password complexity requirements. Attach the appropriate IAM policies to the user for the permissions that the vendor requires.

C. Create an IAM group in the company’s account. Add the tool’s IAM user from the vendor account to the group. Attach the appropriate IAM policies to the group for the permissions that the vendor requires.

D. Create a new identity provider by choosing “AWS account” as the provider type in the IAM console. Supply the vendor’s AWS account ID and user name. Attach the appropriate IAM policies to the new provider for the permissions that the vendor requires.

## Question #222 분석

### 요구사항
- **외부 벤더** 작업 수행 필요
- **벤더 자체 AWS 계정**에서 자동화 툴 운영
- **벤더는 회사 계정 IAM 접근 없음**
- **안전한 접근 권한** 부여 필요

### 선택지 분석

**A. IAM Role + Cross-Account Trust** ⭐
- **Cross-Account 접근**: 벤더 계정의 IAM 역할이 회사 계정 역할 assume 가능 ✅
- **임시 자격증명**: 장기 자격증명 없이 안전한 접근 ✅
- **최소 권한**: 필요한 권한만 역할에 부여 ✅
- **감사 추적**: 모든 작업이 CloudTrail에 기록 ✅

**B. IAM User + 패스워드**
- **장기 자격증명**: 패스워드/액세스 키 관리 부담 ❌
- **보안 위험**: 자격증명 노출 및 남용 위험 ❌
- **회전 복잡성**: 정기적 패스워드 변경 필요 ❌

**C. IAM Group + 외부 사용자 추가**
- **기술적 불가능**: 다른 AWS 계정의 IAM 사용자를 그룹에 추가 불가 ❌
- **Cross-Account 미지원**: IAM 그룹은 동일 계정 내에서만 작동 ❌

**D. Identity Provider + AWS 계정 타입**
- **잘못된 구성**: AWS 계정은 Identity Provider 타입이 아님 ❌
- **기술적 오류**: 존재하지 않는 설정 방법 ❌

### Cross-Account Role 설정

```yaml
1. 회사 계정에서 IAM Role 생성:
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": {
      "AWS": "arn:aws:iam::VENDOR-ACCOUNT-ID:role/VendorToolRole"
    },
    "Action": "sts:AssumeRole"
  }]
}

2. 벤더 계정에서 Role Assume:
aws sts assume-role \
    --role-arn arn:aws:iam::COMPANY-ACCOUNT:role/VendorAccessRole \
    --role-session-name VendorSession
```

### 보안 장점

```yaml
임시 자격증명:
- 자동 만료 (1-12시간)
- 세션별 고유 토큰
- 장기 키 관리 불필요

조건부 접근:
- IP 주소 제한 가능
- 시간 기반 제한 가능
- MFA 요구 가능

감사 및 모니터링:
- 모든 AssumeRole 기록
- 세션별 활동 추적
- 실시간 모니터링 가능
```
