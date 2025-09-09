## Question #237
An application running on an Amazon EC2 instance in VPC-A needs to access files in another EC2 instance in VPC-B. 
Both VPCs are in separate AWS accounts. 
The network administrator needs to design a solution to configure secure access to EC2 instance in VPC-B from VPC-A. 
The connectivity should not have a single point of failure or bandwidth concerns.

Which solution will meet these requirements?

A. Set up a VPC peering connection between VPC-A and VPC-B.

B. Set up VPC gateway endpoints for the EC2 instance running in VPC-B.

C. Attach a virtual private gateway to VPC-B and set up routing from VPC-A.

D. Create a private virtual interface (VIF) for the EC2 instance running in VPC-B and add appropriate routes from VPC-A.

## Question #237 분석

### 요구사항
- **VPC-A에서 VPC-B** EC2 접근
- **서로 다른 AWS 계정**
- **단일 장애점 없음**
- **대역폭 제약 없음**
- **보안 접근**

### 선택지 분석

**A. VPC Peering 연결** ⭐
- **교차 계정 지원**: 다른 계정 간 VPC 연결 가능 ✅
- **프라이빗 네트워크**: AWS 백본 네트워크를 통한 직접 연결 ✅
- **단일 장애점 없음**: AWS 인프라 레벨 이중화 ✅
- **대역폭 제약 없음**: VPC 간 무제한 대역폭 ✅

**B. VPC Gateway Endpoint**
- **용도 불일치**: S3/DynamoDB 전용 서비스, EC2 접근용 아님 ❌
- **기능 오해**: Gateway Endpoint는 AWS 서비스 접근용 ❌

**C. Virtual Private Gateway + 라우팅**
- **VPN 연결**: 온프레미스 연결용 서비스 ❌
- **용도 부적합**: VPC 간 연결이 아닌 하이브리드 연결용 ❌

**D. Private Virtual Interface (VIF)**
- **Direct Connect 구성요소**: 물리적 전용선 연결용 ❌
- **과도한 복잡성**: VPC 간 단순 연결에 부적절 ❌

### VPC Peering 특징

```yaml
교차 계정 설정:
1. VPC-A 계정에서 피어링 요청
2. VPC-B 계정에서 요청 승인
3. 양쪽 라우팅 테이블 설정
4. 보안 그룹 규칙 설정

고가용성:
- AWS 글로벌 인프라 활용
- 자동 이중화
- 단일 장애점 없음
```
