## Question #273
A rapidly growing ecommerce company is running its workloads in a single AWS Region. 
A solutions architect must create a disaster recovery (DR) strategy that includes a different AWS Region. 
The company wants its database to be up to date in the DR Region with the least possible latency. 
The remaining infrastructure in the DR Region needs to run at reduced capacity and must be able to scale up if necessary.

Which solution will meet these requirements with the LOWEST recovery time objective (RTO)?

A. Use an Amazon Aurora global database with a pilot light deployment.

B. Use an Amazon Aurora global database with a warm standby deployment.

C. Use an Amazon RDS Multi-AZ DB instance with a pilot light deployment.

D. Use an Amazon RDS Multi-AZ DB instance with a warm standby deployment.

## Question #273 분석

### 요구사항
- **급성장 이커머스** 단일 리전 운영
- **DR 전략** 다른 리전 구축
- **데이터베이스**: DR 리전에서 최소 지연시간으로 최신 상태 유지
- **인프라**: DR 리전에서 축소 용량 운영, 필요시 확장
- **최저 RTO** 달성

### DR 배포 전략 구분

```yaml
Pilot Light:
- 핵심 구성요소만 최소 운영
- 장애 시 인프라 확장 필요
- RTO: 수십 분 ~ 수 시간

Warm Standby:
- 축소된 완전 시스템 운영
- 장애 시 즉시 확장 가능
- RTO: 수 분 ~ 수십 분
```

### 데이터베이스 옵션 비교

```yaml
Aurora Global Database:
- 교차 리전 자동 복제
- 1초 미만 지연시간
- 1분 내 승격 가능

RDS Multi-AZ:
- 동일 리전 내 복제만
- 교차 리전 복제 불가
- DR 리전 데이터베이스 요구사항 미충족
```

### 선택지 분석

**A. Aurora Global Database + Pilot Light**
- **데이터베이스**: 최소 지연시간 복제 ✅
- **인프라**: 핵심만 운영, 확장 시간 필요 ⚡
- **RTO**: 중간 수준

**B. Aurora Global Database + Warm Standby** ⭐
- **데이터베이스**: 최소 지연시간 복제 ✅
- **인프라**: 축소 운영 중, 즉시 확장 ✅
- **RTO**: 최소 (수 분) ✅
- **요구사항 완벽 충족**: 모든 조건 만족 ✅

**C. RDS Multi-AZ + Pilot Light**
- **치명적 결함**: Multi-AZ는 교차 리전 복제 불가 ❌
- **DR 요구사항 미충족**: 다른 리전에 최신 DB 불가 ❌

**D. RDS Multi-AZ + Warm Standby**
- **동일한 Multi-AZ 문제**: 교차 리전 복제 불가 ❌

### Aurora Global Database 장점

```yaml
교차 리전 복제:
- 1초 미만 복제 지연시간
- 최대 5개 보조 리전 지원
- 자동 장애조치 (1분 내)

Warm Standby 조합:
- DB: Aurora Global로 즉시 가용
- 인프라: 사전 구축된 축소 환경
- 확장: Auto Scaling으로 즉시 용량 증대
```
