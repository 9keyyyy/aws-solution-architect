## Question #93
A company runs an on-premises application that is powered by a MySQL database. 
The company is migrating the application to AWS to increase the application's elasticity and availability.
The current architecture shows heavy read activity on the database during times of normal operation. 
Every 4 hours, the company's development team pulls a full export of the production database to populate a database in the staging environment. 
During this period, users experience unacceptable application latency. 
The development team is unable to use the staging environment until the procedure completes.
A solutions architect must recommend replacement architecture that alleviates the application latency issue. 
The replacement architecture also must give the development team the ability to continue using the staging environment without delay.

Which solution meets these requirements?

A. Use Amazon Aurora MySQL with Multi-AZ Aurora Replicas for production. Populate the staging database by implementing a backup and restore process that uses the mysqldump utility.

B. Use Amazon Aurora MySQL with Multi-AZ Aurora Replicas for production. Use database cloning to create the staging database on-demand.

C. Use Amazon RDS for MySQL with a Multi-AZ deployment and read replicas for production. Use the standby instance for the staging database.

D. Use Amazon RDS for MySQL with a Multi-AZ deployment and read replicas for production. Populate the staging database by implementing a backup and restore process that uses the mysqldump utility.

## Question #93 분석

### ✅ 요구사항
- **MySQL 애플리케이션** AWS 마이그레이션
- **읽기 집약적 워크로드** 처리 필요
- **4시간마다 full export** 시 성능 저하 문제
- **스테이징 환경** 지연 없이 사용 가능해야 함

### ✅ 핵심 문제
```yaml
현재 문제:
❌ Full export 중 프로덕션 성능 저하
❌ 스테이징 환경 사용 불가 기간 발생
❌ 개발팀 생산성 저하

해결 방향:
✅ 프로덕션 영향 최소화
✅ 빠른 스테이징 환경 생성
✅ 읽기 성능 최적화
```

### ✅ 선택지 분석

**A. Aurora + Multi-AZ + mysqldump**
- **Aurora 장점**: 읽기 성능 우수 ✅
- **mysqldump 문제**: 여전히 프로덕션 부하 및 시간 소요 ❌

**B. Aurora + Multi-AZ + Database Cloning** ⭐
- **Aurora 읽기 성능**: Multi-AZ Replicas로 읽기 분산 ✅
- **Database Cloning**: 몇 분 내 즉시 스테이징 생성 ✅
- **프로덕션 무영향**: 클로닝 중 성능 저하 없음 ✅
- **개발팀 효율성**: 대기 시간 없이 즉시 사용 ✅

**C. RDS Multi-AZ + Standby를 스테이징으로**
- **Multi-AZ 오해**: Standby는 장애조치용, 접근 불가 ❌
- **기술적 불가능**: Standby 인스턴스 직접 사용 불가 ❌

**D. RDS Multi-AZ + mysqldump**
- **mysqldump 한계**: 여전히 시간 소요 및 프로덕션 영향 ❌
- **근본 문제 미해결**: 4시간 대기 및 성능 저하 지속 ❌

### 📋 Aurora Database Cloning 핵심

**클로닝 특징**
```yaml
생성 시간: 수 분 (TB 데이터도)
프로덕션 영향: 없음 (Copy-on-Write)
비용: 변경된 데이터만 과금
사용법: 즉시 읽기/쓰기 가능

4시간 → 수 분으로 단축
```

**Copy-on-Write 메커니즘**
```yaml
초기: 프로덕션과 스토리지 공유
변경 시: 변경된 블록만 별도 저장
장점: 빠른 생성, 낮은 비용
```

### 💡 솔루션 효과

**B안 구현 후:**
```yaml
읽기 성능: Aurora Multi-AZ Replicas로 분산
스테이징 생성: 몇 분 내 Database Cloning
개발팀: 대기 없이 즉시 작업 가능
프로덕션: 영향 없음

결과: 모든 문제 해결
```

### 🚫 다른 옵션들의 한계

**A, D안 (mysqldump) 문제:**
```yaml
시간 소요: 여전히 시간 단위
프로덕션 부하: export 중 성능 저하
개발팀 대기: 완료까지 사용 불가
```

**C안 (Standby 사용) 불가능:**
```yaml
Multi-AZ Standby:
- 장애조치 전용 대기 인스턴스
- 평상시 접근 완전 불가
- 읽기/쓰기 모두 차단
- 스테이징 용도로 사용 불가능
```
