## Question #208
A company needs to move data from an Amazon EC2 instance to an Amazon S3 bucket. 
The company must ensure that no API calls and no data are routed through public internet routes. 
Only the EC2 instance can have access to upload data to the S3 bucket.

Which solution will meet these requirements?

A. Create an interface VPC endpoint for Amazon S3 in the subnet where the EC2 instance is located. Attach a resource policy to the S3 bucket to only allow the EC2 instance’s IAM role for access.

B. Create a gateway VPC endpoint for Amazon S3 in the Availability Zone where the EC2 instance is located. Attach appropriate security groups to the endpoint. Attach a resource policy to the S3 bucket to only allow the EC2 instance’s IAM role for access.

C. Run the nslookup tool from inside the EC2 instance to obtain the private IP address of the S3 bucket’s service API endpoint. Create a route in the VPC route table to provide the EC2 instance with access to the S3 bucket. Attach a resource policy to the S3 bucket to only allow the EC2 instance’s IAM role for access.

D. Use the AWS provided, publicly available ip-ranges.json file to obtain the private IP address of the S3 bucket’s service API endpoint. Create a route in the VPC route table to provide the EC2 instance with access to the S3 bucket. Attach a resource policy to the S3 bucket to only allow the EC2 instance’s IAM role for access.

## Question #208 분석

### 요구사항
- **EC2 → S3** 데이터 이동
- **퍼블릭 인터넷 완전 우회** 필수
- **EC2만 S3 접근** 허용

### 선택지 분석

**A. Interface VPC Endpoint + IAM 정책** ⭐
- **프라이빗 네트워크**: Interface Endpoint가 서브넷에 ENI 생성 ✅
- **DNS 해석**: S3 API 호출이 프라이빗 IP로 라우팅 ✅
- **접근 제어**: IAM 역할 기반 S3 버킷 정책 ✅
- **완전 격리**: 모든 트래픽이 VPC 내부 경유 ✅

**B. Gateway VPC Endpoint + 보안 그룹**
- **기술적 오류**: Gateway Endpoint는 보안 그룹 지원 안함 ❌
- **문제 설명 부정확**: 불가능한 구성 제시 ❌

**C. nslookup + 라우팅 테이블**
- **S3 특성 오해**: S3는 프라이빗 IP 제공하지 않음 ❌
- **기술적 불가능**: nslookup으로 해결 불가 ❌

**D. ip-ranges.json + 라우팅**
- **퍼블릭 IP 사용**: 여전히 인터넷 경유 ❌
- **요구사항 미충족**: 퍼블릭 라우트 사용 ❌

### Interface VPC Endpoint 작동 방식

```yaml
구성:
1. 서브넷에 네트워크 인터페이스 생성
2. S3 API 호출이 프라이빗 IP로 해석
3. VPC 내부 트래픽만 발생
4. IAM 정책으로 접근 제어

결과:
- 완전한 프라이빗 통신
- 인터넷 우회
- 세밀한 접근 제어
```
