## Question #224
A company recently migrated its web application to AWS by rehosting the application on Amazon EC2 instances in a single AWS Region. 
The company wants to redesign its application architecture to be highly available and fault tolerant. 
Traffic must reach all running EC2 instances randomly.

Which combination of steps should the company take to meet these requirements? (Choose two.)

A. Create an Amazon Route 53 failover routing policy.

B. Create an Amazon Route 53 weighted routing policy.

C. Create an Amazon Route 53 multivalue answer routing policy.

D. Launch three EC2 instances: two instances in one Availability Zone and one instance in another Availability Zone.

E. Launch four EC2 instances: two instances in one Availability Zone and two instances in another Availability Zone.

## Question #224 분석 (Choose Two)

### 요구사항
- **단일 리전** EC2 인스턴스 재설계
- **고가용성 및 장애 내성** 확보
- **트래픽 랜덤 분산** 필요

### 선택지 분석

**A. Route 53 Failover 라우팅**
- **장애조치 전용**: Primary/Secondary 구조로 랜덤 분산 아님 ❌
- **요구사항 불일치**: 모든 인스턴스에 트래픽 분산 불가 ❌

**B. Route 53 Weighted 라우팅**
- **가중치 기반**: 설정된 비율로 트래픽 분산 ✅
- **랜덤 분산 가능**: 동일 가중치 설정 시 랜덤 효과 ✅
- **고가용성**: 인스턴스 장애 시 자동 제외 ✅

**C. Route 53 Multivalue Answer 라우팅** ⭐
- **진정한 랜덤**: 여러 IP를 동시 반환, 클라이언트가 랜덤 선택 ✅
- **헬스체크 지원**: 비정상 인스턴스 자동 제외 ✅
- **요구사항 완벽 충족**: 모든 정상 인스턴스에 랜덤 분산 ✅

**D. 3개 인스턴스 (2:1 배치)**
- **불균등 배치**: AZ 간 불균형으로 장애 내성 부족 ❌
- **단일 AZ 위험**: 한 AZ에 2개 집중으로 위험도 높음 ❌

**E. 4개 인스턴스 (2:2 배치)** ⭐
- **균등 배치**: 각 AZ에 2개씩 균등 분산 ✅
- **장애 내성**: 한 AZ 장애 시에도 50% 용량 유지 ✅
- **고가용성**: 멀티 AZ 배치로 가용성 극대화 ✅

### Route 53 라우팅 정책 비교

```yaml
Multivalue Answer:
- 모든 정상 레코드 반환
- 클라이언트가 랜덤 선택
- 진정한 로드 밸런싱
- 헬스체크 지원

Weighted:
- 가중치 기반 분산
- 서버 측 제어
- 비율 조정 가능
- 헬스체크 지원

Failover:
- Primary/Secondary만
- 장애조치 전용
- 랜덤 분산 불가
```

### 고가용성 배치 전략

```yaml
E안 (2:2 배치) 장점:
- AZ-A: 2 인스턴스
- AZ-B: 2 인스턴스
- 한 AZ 장애 시: 50% 용량 유지
- 균등 분산: 로드 밸런싱 최적화

D안 (2:1 배치) 문제:
- 불균등 분산
- 단일 AZ 과부하 위험
- 장애 복구 능력 제한
```
