## Question #230
A company is concerned that two NAT instances in use will no longer be able to support the traffic needed for the company’s application. 
A solutions architect wants to implement a solution that is highly available, fault tolerant, and automatically scalable.

What should the solutions architect recommend?

A. Remove the two NAT instances and replace them with two NAT gateways in the same Availability Zone.

B. Use Auto Scaling groups with Network Load Balancers for the NAT instances in different Availability Zones.

C. Remove the two NAT instances and replace them with two NAT gateways in different Availability Zones.

D. Replace the two NAT instances with Spot Instances in different Availability Zones and deploy a Network Load Balancer.

## Question #230 분석

### 요구사항
- **NAT 인스턴스 2개** 트래픽 처리 한계
- **고가용성**, **장애 내성**, **자동 확장** 필요

### 선택지 분석

**A. 동일 AZ에 NAT Gateway 2개**
- **단일 AZ 위험**: AZ 장애 시 모든 NAT 서비스 중단 ❌
- **고가용성 미충족**: 단일 장애점 존재 ❌

**B. Auto Scaling + NLB + NAT 인스턴스**
- **복잡성**: NAT 인스턴스 직접 관리 지속 ❌
- **운영 부담**: Auto Scaling 및 헬스체크 설정 복잡 ❌
- **최적화 부족**: 관리형 서비스 대비 비효율 ❌

**C. 다른 AZ에 NAT Gateway 2개** ⭐
- **고가용성**: 멀티 AZ 배치로 AZ 장애 내성 ✅
- **완전 관리형**: AWS가 확장성 및 가용성 보장 ✅
- **자동 확장**: 트래픽에 따라 자동 처리량 조정 ✅
- **운영 부담 없음**: 패치, 모니터링 등 AWS 담당 ✅

**D. Spot 인스턴스 + NLB**
- **가용성 위험**: Spot 인스턴스 중단 가능성 ❌
- **NAT 서비스 부적합**: 중단 불가 서비스에 Spot 사용 부적절 ❌

### NAT Gateway vs NAT Instance

```yaml
NAT Gateway:
✅ 완전 관리형 서비스
✅ 자동 확장 (45Gbps까지)
✅ 고가용성 (99.95% SLA)
✅ 운영 부담 없음

NAT Instance:
❌ 수동 관리 필요
❌ 제한된 처리량
❌ 단일 장애점
❌ 패치 및 모니터링 부담
```

### 고가용성 아키텍처

```yaml
권장 구성:
- AZ-A: NAT Gateway 1
- AZ-B: NAT Gateway 2
- 각 AZ의 프라이빗 서브넷이 해당 AZ NAT Gateway 사용
- AZ 장애 시 다른 AZ로 라우팅 전환 가능
```
