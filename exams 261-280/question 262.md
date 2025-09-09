## Question #262
A company plans to use Amazon ElastiCache for its multi-tier web application. 
A solutions architect creates a Cache VPC for the ElastiCache cluster and an App VPC for the application’s Amazon EC2 instances. 
Both VPCs are in the us-east-1 Region.

The solutions architect must implement a solution to provide the application’s EC2 instances with access to the ElastiCache cluster.

Which solution will meet these requirements MOST cost-effectively?

A. Create a peering connection between the VPCs. Add a route table entry for the peering connection in both VPCs. Configure an inbound rule for the ElastiCache cluster’s security group to allow inbound connection from the application’s security group.

B. Create a Transit VPC. Update the VPC route tables in the Cache VPC and the App VPC to route traffic through the Transit VPC. Configure an inbound rule for the ElastiCache cluster's security group to allow inbound connection from the application’s security group.

C. Create a peering connection between the VPCs. Add a route table entry for the peering connection in both VPCs. Configure an inbound rule for the peering connection’s security group to allow inbound connection from the application’s security group.

D. Create a Transit VPC. Update the VPC route tables in the Cache VPC and the App VPC to route traffic through the Transit VPC. Configure an inbound rule for the Transit VPC’s security group to allow inbound connection from the application’s security group.

## Question #262 분석

### 요구사항
- **ElastiCache 클러스터** (Cache VPC)와 **EC2 애플리케이션** (App VPC) 연결
- **동일 리전** (us-east-1) 내 VPC 간 통신
- **최고 비용 효율성**

### 선택지 분석

**A. VPC Peering + 보안 그룹 설정** ⭐
- **VPC Peering**: 두 VPC 간 직접 연결로 비용 효율적 ✅
- **라우팅 테이블**: 양쪽 VPC에 피어링 경로 추가 ✅
- **올바른 보안 그룹**: ElastiCache 보안 그룹에 애플리케이션 보안 그룹 허용 ✅
- **최저 비용**: 피어링 연결은 데이터 전송 비용만 발생 ✅

**B. Transit VPC + 보안 그룹 설정**
- **과도한 복잡성**: 단순 2개 VPC 연결에 Transit VPC 불필요 ❌
- **높은 비용**: 추가 EC2 인스턴스 및 NAT Gateway 비용 ❌
- **올바른 보안 설정**: 보안 그룹 설정은 정확 ✅

**C. VPC Peering + 잘못된 보안 그룹**
- **보안 그룹 오류**: "피어링 연결의 보안 그룹"은 존재하지 않는 개념 ❌
- **기술적 불가능**: 피어링 자체는 보안 그룹이 없음 ❌

**D. Transit VPC + 잘못된 보안 그룹**
- **동일한 비용 문제**: Transit VPC 불필요한 복잡성 ❌
- **보안 그룹 오류**: "Transit VPC의 보안 그룹"으로는 ElastiCache 접근 불가 ❌

### VPC Peering 솔루션

```yaml
설정 과정:
1. Cache VPC ↔ App VPC 피어링 연결 생성
2. 양쪽 라우팅 테이블에 피어링 경로 추가
3. ElastiCache 보안 그룹 인바운드 규칙:
   - 소스: App VPC 애플리케이션 보안 그룹
   - 포트: 6379 (Redis) 또는 11211 (Memcached)

비용:
- 피어링 연결: 무료
- 데이터 전송: 동일 AZ 내 무료, 교차 AZ 시 소량
```
