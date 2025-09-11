## Question #1011
A company hosts an application in a private subnet. The company has already integrated the application with Amazon Cognito. The company uses an Amazon Cognito user pool to authenticate users.

The company needs to modify the application so the application can securely store user documents in an Amazon S3 bucket.

Which combination of steps will securely integrate Amazon S3 with the application? (Choose two.)

A. Create an Amazon Cognito identity pool to generate secure Amazon S3 access tokens for users when they successfully log in.

B. Use the existing Amazon Cognito user pool to generate Amazon S3 access tokens for users when they successfully log in.

C. Create an Amazon S3 VPC endpoint in the same VPC where the company hosts the application.

D. Create a NAT gateway in the VPC where the company hosts the application. Assign a policy to the S3 bucket to deny any request that is not initiated from Amazon Cognito.

E. Attach a policy to the S3 bucket that allows access only from the users' IP addresses.

## Question #1011 분석

### 문제 상황
- **Private subnet**의 애플리케이션
- **Cognito User Pool** 이미 구현 (인증)
- **S3에 사용자 문서 저장** 필요
- **보안** 요구사항

### 핵심 요구사항
- **S3 액세스 토큰** 생성
- **Private subnet에서 S3 접근**

### Cognito 구조

**User Pool**: 인증 (Authentication)
**Identity Pool**: 권한 부여 (Authorization) → AWS 서비스 접근

### 선택지 분석

**A. Cognito Identity Pool 생성** ⭐
- User Pool 인증 후 Identity Pool이 AWS 자격증명 제공 ✅
- S3 접근을 위한 임시 자격증명 생성 ✅

**B. User Pool로 S3 토큰 생성**
- User Pool은 인증만 담당, AWS 서비스 토큰 생성 안함 ❌

**C. S3 VPC Endpoint** ⭐
- Private subnet에서 인터넷 거치지 않고 S3 접근 ✅
- 보안 향상 ✅

**D. NAT Gateway + 버킷 정책**
- 인터넷 경유 (보안성 낮음) ❌
- Cognito 기반 정책 복잡함 ❌

**E. IP 기반 정책**
- Private subnet IP는 동적이고 공유됨 ❌
- 사용자별 구분 불가 ❌

### 올바른 아키텍처

```yaml
1. User Pool: 사용자 인증
2. Identity Pool: AWS 자격증명 제공
3. VPC Endpoint: 보안 S3 접근
```

**정답: A + C**

Cognito Identity Pool로 S3 접근 자격증명을 제공하고, VPC Endpoint로 보안 네트워크 연결을 구현합니다.