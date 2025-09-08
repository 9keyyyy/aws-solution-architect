## Question #217
A company runs a global web application on Amazon EC2 instances behind an Application Load Balancer. 
The application stores data in Amazon Aurora. 
The company needs to create a disaster recovery solution and can tolerate up to 30 minutes of downtime and potential data loss. 
The solution does not need to handle the load when the primary infrastructure is healthy.

What should a solutions architect do to meet these requirements?

A. Deploy the application with the required infrastructure elements in place. Use Amazon Route 53 to configure active-passive failover. Create an Aurora Replica in a second AWS Region.

B. Host a scaled-down deployment of the application in a second AWS Region. Use Amazon Route 53 to configure active-active failover. Create an Aurora Replica in the second Region.

C. Replicate the primary infrastructure in a second AWS Region. Use Amazon Route 53 to configure active-active failover. Create an Aurora database that is restored from the latest snapshot.

D. Back up data with AWS Backup. Use the backup to create the required infrastructure in a second AWS Region. Use Amazon Route 53 to configure active-passive failover. Create an Aurora second primary instance in the second Region.

## Question #217 분석

### 요구사항
- **글로벌 웹 애플리케이션** (EC2 + ALB + Aurora)
- **재해복구 솔루션** 필요
- **30분 다운타임 허용**
- **데이터 손실 허용** 가능
- **평상시 부하 처리 불필요**

### 선택지 분석

**A. 필요 인프라 배치 + Route 53 Active-Passive + Aurora Replica** ⭐
- **Active-Passive**: 평상시 부하 처리 불필요 조건 충족 ✅
- **Aurora Replica**: 자동 장애조치, 30분 RTO 달성 가능 ✅
- **인프라 준비**: 재해 시 즉시 활성화 ✅
- **비용 효율**: 필요한 리소스만 미리 배치 ✅

**B. 축소 배포 + Active-Active + Aurora Replica**
- **Active-Active**: 평상시에도 트래픽 분산, 요구사항 위반 ❌
- **축소 배포**: 성능 부족 위험 ❌

**C. 전체 인프라 복제 + Active-Active + 스냅샷 복원**
- **Active-Active**: 요구사항 위반 (평상시 부하 처리 불필요) ❌
- **과도한 비용**: 전체 인프라 복제로 비용 증가 ❌
- **스냅샷 복원**: Aurora Replica보다 복구 시간 길어짐 ❌

**D. AWS Backup + Active-Passive + Aurora 두 번째 Primary**
- **복잡한 복구**: 백업에서 인프라 생성 시간 소요 ❌
- **30분 RTO 위험**: 인프라 생성 시간으로 목표 달성 어려움 ❌

### Active-Passive vs Active-Active

```yaml
Active-Passive:
- 평상시: Primary만 활성
- 재해 시: Secondary로 전환
- 비용: 낮음
- 요구사항 충족

Active-Active:
- 평상시: 양쪽 모두 활성
- 지속적: 부하 분산
- 비용: 높음
- 요구사항 위반
```

### Aurora Cross-Region Replica

```yaml
장점:
- 자동 장애조치 지원
- 읽기 전용 복제본으로 비용 절약
- 수 분 내 승격 가능
- 30분 RTO 충족 가능
```

**정답: A**

Active-Passive 구성과 Aurora Cross-Region Replica를 통해 평상시 부하 처리 없이도 30분 RTO 요구사항을 충족하는 비용 효율적인 재해복구 솔루션을 제공합니다.