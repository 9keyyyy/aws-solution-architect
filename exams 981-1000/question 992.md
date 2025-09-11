## Question #992
A company's software development team needs an Amazon RDS Multi-AZ cluster. The RDS cluster will serve as a backend for a desktop client that is deployed on premises. The desktop client requires direct connectivity to the RDS cluster.

The company must give the development team the ability to connect to the cluster by using the client when the team is in the office.

Which solution provides the required connectivity MOST securely?

A. Create a VPC and two public subnets. Create the RDS cluster in the public subnets. Use AWS Site-to-Site VPN with a customer gateway in the company's office.

B. Create a VPC and two private subnets. Create the RDS cluster in the private subnets. Use AWS Site-to-Site VPN with a customer gateway in the company's office.

C. Create a VPC and two private subnets. Create the RDS cluster in the private subnets. Use RDS security groups to allow the company's office IP ranges to access the cluster.

D. Create a VPC and two public subnets. Create the RDS cluster in the public subnets. Create a cluster user for each developer. Use RDS security groups to allow the users to access the cluster.

## Question #992 분석

### 문제 상황
- **RDS Multi-AZ 클러스터** 필요
- **온프레미스 데스크톱 클라이언트**에서 직접 연결
- **개발팀이 사무실에서 접근** 필요
- **최대 보안** 요구

### 핵심 요구사항
- **온프레미스 ↔ RDS 연결**
- **직접 연결성**
- **최대 보안**

### 보안 원칙
1. **Private Subnet**: DB는 인터넷에 직접 노출 금지
2. **VPN 연결**: 안전한 온프레미스 연결
3. **Security Group**: 네트워크 레벨 접근 제어

### 선택지 분석

**A. Public Subnet + Site-to-Site VPN**
- Public Subnet: RDS가 인터넷에 노출 ❌
- 보안 위험 증가 ❌

**B. Private Subnet + Site-to-Site VPN** ⭐
- Private Subnet: RDS 인터넷 격리 ✅
- Site-to-Site VPN: 암호화된 전용 연결 ✅
- 최고 보안 수준 ✅

**C. Private Subnet + IP 범위 허용**
- 인터넷을 통한 연결 → 보안 위험 ❌
- VPN 없이는 직접 연결 불가 ❌

**D. Public Subnet + 사용자별 계정**
- Public Subnet: 보안 위험 ❌
- 사용자 계정은 애플리케이션 레벨 보안 ❌

### Site-to-Site VPN 장점

```yaml
보안:
- IPSec 암호화 터널
- 전용 네트워크 연결
- 인터넷 트래픽과 격리

연결성:
- 온프레미스 ↔ VPC 직접 연결
- RDS에 Private IP로 접근
- 안정적인 네트워크 성능
```

### Private vs Public Subnet

**Private Subnet (권장)**
```yaml
- 인터넷 게이트웨이 없음
- 외부에서 직접 접근 불가
- VPN/Direct Connect로만 접근
```

**Public Subnet (비권장)**
```yaml
- 인터넷에 직접 노출
- 보안 공격 위험
- DB 모범 사례 위배
```
