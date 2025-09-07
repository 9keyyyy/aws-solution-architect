## Question #68
A solutions architect is designing a new hybrid architecture to extend a company's on-premises infrastructure to AWS. 
The company requires a highly available connection with consistent low latency to an AWS Region. 
The company needs to minimize costs and is willing to accept slower traffic if the primary connection fails.

What should the solutions architect do to meet these requirements?

A. Provision an AWS Direct Connect connection to a Region. Provision a VPN connection as a backup if the primary Direct Connect connection fails.

B. Provision a VPN tunnel connection to a Region for private connectivity. Provision a second VPN tunnel for private connectivity and as a backup if the primary VPN connection fails.

C. Provision an AWS Direct Connect connection to a Region. Provision a second Direct Connect connection to the same Region as a backup if the primary Direct Connect connection fails.

D. Provision an AWS Direct Connect connection to a Region. Use the Direct Connect failover attribute from the AWS CLI to automatically create a backup connection if the primary Direct Connect connection fails.

## Question #68 분석

### ✅ 요구사항
- **하이브리드 아키텍처**: 온프레미스를 AWS로 확장
- **고가용성 연결**: 안정적인 연결 필요
- **일관된 저지연**: 성능 요구사항
- **비용 최소화**: 경제적 효율성 우선
- **장애 시 느린 속도 허용**: 백업 연결 성능 타협 가능

### ✅ 선택지 분석

**A. Direct Connect + VPN 백업** ⭐
- **주 연결**: Direct Connect로 고성능 전용선 ✅
- **백업 연결**: VPN으로 비용 효율적 백업 ✅
- **비용 최적화**: VPN은 Direct Connect 대비 저렴 ✅
- **성능 타협**: 장애 시 VPN의 느린 속도 허용 ✅
- **고가용성**: 이중화 구성으로 연결 안정성 보장 ✅

**B. VPN + VPN 백업**
- **주 연결**: VPN으로는 일관된 저지연 보장 어려움 ❌
- **성능 한계**: 인터넷 기반으로 지연 시간 변동 ❌
- **대역폭 제한**: Direct Connect 대비 성능 부족 ❌
- **요구사항 미충족**: 고성능 요구사항 불만족 ❌

**C. Direct Connect + Direct Connect 백업**
- **고성능**: 두 연결 모두 최고 성능 ✅
- **비용 문제**: Direct Connect 이중화는 매우 비싸다 ❌
- **과도한 투자**: 백업용으로 고가 솔루션 불필요 ❌
- **비용 최소화 위반**: 핵심 요구사항 위배 ❌

**D. Direct Connect + 자동 백업 기능**
- **잘못된 기능**: AWS CLI failover 속성 존재하지 않음 ❌
- **실제 불가능**: 해당 기능은 AWS에서 제공하지 않음 ❌
- **백업 미제공**: 실제 백업 연결이 구성되지 않음 ❌

### 📋 핵심 개념 정리

#### **하이브리드 연결 옵션 비교**
```yaml
Direct Connect:
  ✅ 전용 물리 연결
  ✅ 일관된 저지연
  ✅ 높은 대역폭
  ❌ 높은 비용
  ❌ 단일 장애점 위험

VPN Connection:
  ✅ 저렴한 비용
  ✅ 빠른 구축
  ⚠️ 인터넷 기반 변동성
  ⚠️ 상대적 고지연

하이브리드 백업:
  ✅ 비용 효율적
  ✅ 이중화 보장
  ✅ 성능-비용 균형
```

#### **Direct Connect + VPN 백업 아키텍처**
```yaml
정상 운영:
  ✅ Direct Connect로 모든 트래픽
  ✅ 1-10ms 일관된 지연시간
  ✅ 최대 100Gbps 대역폭
  ✅ 예측 가능한 네트워크 성능

장애 발생 시:
  ✅ VPN으로 자동 라우팅
  ⚠️ 20-100ms 지연시간 증가
  ⚠️ 대역폭 제한 (보통 1Gbps 이하)
  ✅ 비즈니스 연속성 유지
```

#### **정답 시나리오 (A번)**
```yaml
1. Direct Connect 구성
   - 전용 포트 프로비저닝
   - BGP 라우팅 설정
   - VGW/DX Gateway 연결

2. VPN 백업 구성
   - Customer Gateway 설정
   - IPSec VPN 터널 생성
   - 백업 라우팅 경로 설정

3. 라우팅 우선순위
   - Direct Connect: AS Path 우선
   - VPN: 백업 경로로 설정
   - 자동 장애조치 구현

4. 운영 결과
   ✅ 평상시 최고 성능
   ✅ 장애시 연결 유지
   ✅ 비용 효율적 백업
   ✅ 고가용성 달성
```
