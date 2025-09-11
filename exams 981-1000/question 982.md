## Question #982
A company has an Amazon S3 bucket that contains sensitive data files. The company has an application that runs on virtual machines in an on-premises data center. The company currently uses AWS IAM Identity Center.

The application requires temporary access to files in the S3 bucket. The company wants to grant the application secure access to the files in the S3 bucket.

Which solution will meet these requirements?

A. Create an S3 bucket policy that permits access to the bucket from the public IP address range of the company’s on-premises data center.

B. Use IAM Roles Anywhere to obtain security credentials in IAM Identity Center that grant access to the S3 bucket. Configure the virtual machines to assume the role by using the AWS CLI.

C. Install the AWS CLI on the virtual machine. Configure the AWS CLI with access keys from an IAM user that has access to the bucket.

D. Create an IAM user and policy that grants access to the bucket. Store the access key and secret key for the IAM user in AWS Secrets Manager. Configure the application to retrieve the access key and secret key at startup.

## Question #982 분석

### 문제 상황
- **S3 민감 데이터** 보관
- **온프레미스 VM** 애플리케이션
- **IAM Identity Center** 사용 중
- **임시 접근** 필요

### 핵심 요구사항
- **보안 접근**
- **임시 자격증명**
- **온프레미스에서 S3 접근**

### 선택지 분석

**A. S3 버킷 정책 + 공인 IP 범위**
- IP 기반 접근 제어 → 보안 취약 ❌
- 임시 접근이 아닌 영구 접근 ❌

**B. IAM Roles Anywhere** ⭐
- 온프레미스용 임시 자격증명 솔루션 ✅
- IAM Identity Center와 통합 ✅
- 인증서 기반 보안 인증 ✅
- AWS CLI로 역할 assume ✅

**C. IAM 사용자 액세스 키**
- 장기 자격증명 → 보안 위험 ❌
- "임시 접근" 요구사항 미충족 ❌

**D. IAM 사용자 + Secrets Manager**
- 여전히 장기 자격증명 ❌
- 복잡성 증가 ❌

### IAM Roles Anywhere

```yaml
기능:
- 온프레미스 워크로드용 임시 자격증명
- X.509 인증서 기반 인증
- 기존 PKI 인프라 활용
- IAM Role과 동일한 권한 모델

장점:
- 임시 자격증명 (1시간 만료)
- 인증서 기반 강력한 보안
- 중앙화된 권한 관리
```

### 보안 모범 사례

**임시 vs 장기 자격증명**
- 임시: 자동 만료, 보안 우수
- 장기: 관리 부담, 유출 위험
