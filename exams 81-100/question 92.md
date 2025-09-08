## Question #92
A company is storing sensitive user information in an Amazon S3 bucket. 
The company wants to provide secure access to this bucket from the application tier running on Amazon EC2 instances inside a VPC.

Which combination of steps should a solutions architect take to accomplish this? (Choose two.)

A. Configure a VPC gateway endpoint for Amazon S3 within the VPC.

B. Create a bucket policy to make the objects in the S3 bucket public.

C. Create a bucket policy that limits access to only the application tier running in the VPC.

D. Create an IAM user with an S3 access policy and copy the IAM credentials to the EC2 instance.

E. Create a NAT instance and have the EC2 instances use the NAT instance to access the S3 bucket.

## Question #92 분석 

### ✅ 요구사항
- **민감한 사용자 정보** S3 저장
- **VPC 내 EC2**에서 안전한 S3 접근
- **보안 접근** 방식 구현

### ✅ 선택지 분석

**A. VPC Gateway Endpoint for S3** ⭐
- **안전한 네트워크 경로**: 인터넷 우회, VPC 내부 통신 ✅
- **민감 데이터 보호**: AWS 백본 네트워크만 사용 ✅

**B. 버킷 퍼블릭 정책**
- **보안 위험**: 민감 정보를 퍼블릭으로 노출 ❌
- **요구사항 위배**: "secure access" 정반대 ❌

**C. VPC 제한 버킷 정책** ⭐
- **접근 제어**: VPC 내 애플리케이션만 접근 허용 ✅
- **최소 권한**: 필요한 리소스만 접근 ✅
- **민감 데이터 보호**: 외부 접근 차단 ✅

**D. IAM 자격증명 복사**
- **보안 위험**: EC2에 하드코딩된 자격증명 ❌
- **모범 사례 위반**: IAM Role 사용해야 함 ❌

**E. NAT Instance 사용**
- **인터넷 경유**: 민감 데이터가 인터넷 통과 ❌
- **보안 위험**: 불필요한 인터넷 노출 ❌

### 📋 선택된 조합 (A + C)

**A + C 조합 효과:**
```yaml
네트워크 보안 (A):
✅ VPC 내부 전용 경로
✅ 인터넷 우회
✅ AWS 백본 네트워크

접근 제어 (C):
✅ VPC 기반 제한
✅ 특정 애플리케이션만 접근
✅ 외부 차단
```

**버킷 정책 예시 (C):**
```json
{
  "Statement": [{
    "Effect": "Allow",
    "Principal": "*",
    "Action": "s3:*",
    "Resource": ["arn:aws:s3:::bucket/*"],
    "Condition": {
      "StringEquals": {
        "aws:SourceVpc": "vpc-12345678"
      }
    }
  }]
}
```
