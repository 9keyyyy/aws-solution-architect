## Question #241
An online learning company is migrating to the AWS Cloud. 
The company maintains its student records in a PostgreSQL database. 
The company needs a solution in which its data is available and online across multiple AWS Regions at all times.

Which solution will meet these requirements with the LEAST amount of operational overhead?

A. Migrate the PostgreSQL database to a PostgreSQL cluster on Amazon EC2 instances.

B. Migrate the PostgreSQL database to an Amazon RDS for PostgreSQL DB instance with the Multi-AZ feature turned on.

C. Migrate the PostgreSQL database to an Amazon RDS for PostgreSQL DB instance. Create a read replica in another Region.

D. Migrate the PostgreSQL database to an Amazon RDS for PostgreSQL DB instance. Set up DB snapshots to be copied to another Region.

## Question #241 분석

### 요구사항
- **PostgreSQL 데이터베이스** 마이그레이션
- **여러 AWS 리전**에서 데이터 가용
- **항상 온라인** 상태 유지
- **최소 운영 오버헤드**

### 선택지 분석

**A. EC2 PostgreSQL 클러스터**
- **수동 관리**: 데이터베이스 설치, 패치, 백업 등 직접 관리 ❌
- **복제 설정**: 멀티 리전 복제를 수동으로 구성 ❌
- **높은 운영 부담**: 가장 많은 관리 작업 필요 ❌

**B. RDS Multi-AZ**
- **단일 리전**: Multi-AZ는 동일 리전 내 가용 영역 간 복제만 ❌
- **요구사항 미충족**: 여러 리전에서 데이터 접근 불가 ❌

**C. RDS + Cross-Region Read Replica** ⭐
- **멀티 리전 지원**: 다른 리전에 읽기 전용 복제본 자동 생성 ✅
- **자동 복제**: AWS가 데이터 동기화 자동 관리 ✅
- **온라인 가용**: 두 리전에서 모두 데이터 접근 가능 ✅
- **최소 운영**: 완전 관리형 서비스로 운영 부담 최소 ✅

**D. RDS + 스냅샷 복사**
- **항상 온라인 아님**: 스냅샷은 정적 백업, 실시간 접근 불가 ❌
- **수동 복원**: 다른 리전에서 사용하려면 수동 복원 필요 ❌
- **가용성 부족**: 지속적인 온라인 서비스 제공 불가 ❌

### Cross-Region Read Replica 장점

```yaml
자동 관리:
- 데이터 자동 동기화
- 네트워크 설정 자동 구성
- 장애 시 자동 복구

다중 리전 가용성:
- 주 리전: 읽기/쓰기 가능
- 보조 리전: 읽기 전용 접근
- 글로벌 데이터 접근 보장

운영 효율성:
- 백업, 패치 자동 처리
- 모니터링 내장
- 확장 용이
```
