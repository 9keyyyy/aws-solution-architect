## Question #55
A solutions architect is developing a VPC architecture that includes multiple subnets. 
The architecture will host applications that use Amazon EC2 instances and Amazon RDS DB instances. 
The architecture consists of six subnets in two Availability Zones. 
Each Availability Zone includes a public subnet, a private subnet, and a dedicated subnet for databases. 
Only EC2 instances that run in the private subnets can have access to the RDS databases.

Which solution will meet these requirements?

A. Create a new route table that excludes the route to the public subnets' CIDR blocks. Associate the route table with the database subnets.

B. Create a security group that denies inbound traffic from the security group that is assigned to instances in the public subnets. Attach the security group to the DB instances.

C. Create a security group that allows inbound traffic from the security group that is assigned to instances in the private subnets. Attach the security group to the DB instances.

D. Create a new peering connection between the public subnets and the private subnets. Create a different peering connection between the private subnets and the database subnets.

## Question #55 분석

### ✅ 요구사항
- **6개 서브넷** (각 AZ당 3개씩, 2개 AZ)
- **서브넷 구성**: Public, Private, Database 전용
- **접근 제한**: **Private 서브넷의 EC2만** RDS 접근 가능
- Public 서브넷에서 Database 접근 차단

### ✅ 선택지 분석

**A. 라우트 테이블에서 Public 서브넷 CIDR 제외**
- **라우팅 문제**: 네트워크 라우팅으로는 보안 제어 불가 ❌
- **동일 VPC**: 모든 서브넷이 같은 VPC 내에서 통신 가능 ❌
- **접근 제어 방식 오류**: 라우팅 ≠ 보안 그룹 ❌

**B. Public 서브넷 보안 그룹을 거부하는 보안 그룹**
- **보안 그룹 특성**: Deny 규칙 지원 안 함 ❌
- **Allow 기반**: 보안 그룹은 Allow 규칙만 가능 ❌
- **구현 불가능**: AWS 보안 그룹 작동 방식과 맞지 않음 ❌

**C. Private 서브넷 보안 그룹만 허용하는 보안 그룹** ⭐
- **Allow 기반 제어**: 보안 그룹 정상 작동 방식 ✅
- **선택적 접근**: Private 서브넷에서만 인바운드 허용 ✅
- **기본 거부**: 명시되지 않은 트래픽은 자동 차단 ✅
- **정확한 보안 모델**: 최소 권한 원칙 적용 ✅

**D. 서브넷 간 피어링 연결**
- **VPC 피어링 오용**: 동일 VPC 내 서브넷에는 불필요 ❌
- **복잡성 증가**: 불필요한 네트워크 복잡화 ❌
- **비용 증가**: 피어링 연결은 서로 다른 VPC용 ❌

### 📋 핵심 개념 정리

#### **AWS 보안 그룹 특성**
```yaml
보안 그룹 규칙:
  ✅ Allow 규칙만 가능 (화이트리스트)
  ❌ Deny 규칙 지원 안 함
  ✅ 보안 그룹 간 참조 가능
  ✅ 기본값: 모든 인바운드 차단

참조 방식:
  ✅ 다른 보안 그룹을 소스로 지정
  ✅ 동적 멤버십 (인스턴스 추가/제거 자동 반영)
```

#### **정답 시나리오 (C번)**
```yaml
1. Private 서브넷 EC2용 보안 그룹 생성
   - Name: private-ec2-sg
   
2. Database용 보안 그룹 생성
   - Name: database-sg
   - Inbound Rule: 
     * Source: private-ec2-sg
     * Port: 3306 (MySQL) 또는 해당 DB 포트
     * Protocol: TCP

3. 결과:
   ✅ Private 서브넷 EC2 → Database 접근 가능
   ❌ Public 서브넷 EC2 → Database 접근 차단
   ❌ 기타 모든 소스 → Database 접근 차단
```
