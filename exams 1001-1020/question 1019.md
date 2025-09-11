## Question #1019
A company is developing an application in the AWS Cloud. 
The application's HTTP API contains critical information that is published in Amazon API Gateway. 
The critical information must be accessible from only a limited set of trusted IP addresses that belong to the company's internal network.

Which solution will meet these requirements?

A. Set up an API Gateway private integration to restrict access to a predefined set of IP addresses.

B. Create a resource policy for the API that denies access to any IP address that is not specifically allowed.

C. Directly deploy the API in a private subnet. Create a network ACL. Set up rules to allow the traffic from specific IP addresses.

D. Modify the security group that is attached to API Gateway to allow inbound traffic from only the trusted IP addresses.

## Question #1019 분석

### 문제 상황
- **API Gateway**에 중요한 정보를 포함한 HTTP API 배포
- **회사 내부 네트워크의 신뢰할 수 있는 IP 주소**에서만 접근 허용
- **제한된 IP 세트**에서만 API 접근 가능해야 함

### 핵심 요구사항
- **특정 IP 주소**에서만 API 접근
- **API Gateway 보안** 구현
- **신뢰할 수 있는 내부 네트워크**만 허용

### 선택지 분석

**A. API Gateway Private Integration**
- **Private Integration**: 백엔드와의 연결 방식 (VPC 내부 서비스 연동) ❌
- **용도 불일치**: IP 기반 접근 제어와 무관 ❌
- **Private Integration**: API를 비공개로 만드는 것이 아니라 백엔드 연결 방식 ❌
- **IP 제한 기능 없음**: Private Integration으로는 특정 IP 제한 불가 ❌

**B. API Gateway Resource Policy (IP 기반 Deny)** ⭐
- **Resource Policy**: API Gateway의 접근 제어 정책 ✅
- **IP 기반 제어**: 특정 IP 주소 기반으로 허용/거부 가능 ✅
- **Deny 정책**: 허용되지 않은 IP에서 접근 거부 ✅
- **직접적 해결**: 요구사항에 정확히 부합 ✅

**C. Private Subnet + Network ACL**
- **API Gateway 배포 위치**: API Gateway는 AWS 관리형 서비스로 특정 서브넷에 배포되지 않음 ❌
- **잘못된 개념**: API Gateway는 VPC 외부의 관리형 서비스 ❌
- **Network ACL 적용 불가**: API Gateway에 직접 Network ACL 적용 불가 ❌
- **구현 불가능**: 기술적으로 실현 불가능한 접근법 ❌

**D. Security Group 수정**
- **Security Group 적용 불가**: API Gateway는 관리형 서비스로 Security Group 연결 안됨 ❌
- **VPC 리소스 아님**: API Gateway는 VPC 리소스가 아니므로 SG 적용 불가 ❌
- **잘못된 보안 접근**: API Gateway 보안 모델과 맞지 않음 ❌

