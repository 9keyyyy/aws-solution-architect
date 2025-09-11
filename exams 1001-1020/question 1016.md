## Question #1016
A company runs an application in a private subnet behind an Application Load Balancer (ALB) in a VPC. 
The VPC has a NAT gateway and an internet gateway. 
The application calls the Amazon S3 API to store objects.

According to the company's security policy, traffic from the application must not travel across the internet.

Which solution will meet these requirements MOST cost-effectively?

A. Configure an S3 interface endpoint. Create a security group that allows outbound traffic to Amazon S3.

B. Configure an S3 gateway endpoint. Update the VPC route table to use the endpoint.

C. Configure an S3 bucket policy to allow traffic from the Elastic IP address that is assigned to the NAT gateway.

D. Create a second NAT gateway in the same subnet where the legacy application is deployed. Update the VPC route table to use the second NAT gateway.

## Question #1016 분석

### 문제 상황
- Private subnet의 애플리케이션 → ALB 뒤에 위치
- S3 API 호출하여 객체 저장
- **트래픽이 인터넷을 거치면 안됨** (보안 정책)
- **비용 효율적** 솔루션 필요

### 핵심 요구사항
- **인터넷 트래픽 회피**
- **S3 접근 유지**
- **비용 최적화**

### VPC Endpoint 비교

| 구분 | Gateway Endpoint | Interface Endpoint |
|------|------------------|-------------------|
| **지원 서비스** | S3, DynamoDB만 | 대부분 AWS 서비스 |
| **비용** | **무료** | ENI당 시간당 요금 |
| **구현** | Route Table 수정 | Security Group 필요 |
| **트래픽** | AWS 내부망 | AWS 내부망 |

### 선택지 분석

**A. S3 Interface Endpoint + Security Group**
```yaml
기능: ✅ 인터넷 우회, S3 접근 가능
비용: ❌ ENI당 시간당 요금 발생
구현: Security Group 설정 필요
```

**B. S3 Gateway Endpoint + Route Table** ⭐
```yaml
기능: ✅ 인터넷 우회, S3 접근 가능  
비용: ✅ 완전 무료
구현: Route Table 수정만 필요
```

**C. S3 Bucket Policy + NAT Gateway**
```yaml
기능: ❌ 여전히 인터넷 경유 (NAT Gateway → IGW)
보안: ❌ 보안 정책 위반
비용: ❌ NAT Gateway 데이터 전송 비용
```

**D. 추가 NAT Gateway**
```yaml
기능: ❌ 여전히 인터넷 경유
비용: ❌ 추가 NAT Gateway 비용
목적: ❌ 문제 해결과 무관
```

