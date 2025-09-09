## Question #229
A company manages its own Amazon EC2 instances that run MySQL databases. 
The company is manually managing replication and scaling as demand increases or decreases. 
The company needs a new solution that simplifies the process of adding or removing compute capacity to or from its database tier as needed. 
The solution also must offer improved performance, scaling, and durability with minimal effort from operations.

Which solution meets these requirements?

A. Migrate the databases to Amazon Aurora Serverless for Aurora MySQL.

B. Migrate the databases to Amazon Aurora Serverless for Aurora PostgreSQL.

C. Combine the databases into one larger MySQL database. Run the larger database on larger EC2 instances.

D. Create an EC2 Auto Scaling group for the database tier. Migrate the existing databases to the new environment.

## Question #229 분석

### 요구사항
- **MySQL 데이터베이스** EC2에서 수동 관리
- **복제 및 확장** 자동화 필요
- **컴퓨팅 용량** 동적 추가/제거
- **성능, 확장성, 내구성** 개선
- **운영 노력 최소화**

### 선택지 분석

**A. Aurora Serverless for Aurora MySQL** ⭐
- **MySQL 호환**: 기존 MySQL에서 직접 마이그레이션 가능 ✅
- **서버리스**: 용량 자동 확장/축소, 운영 부담 없음 ✅
- **성능 개선**: Aurora 엔진으로 MySQL 대비 5배 성능 ✅
- **내구성**: 6개 복사본 자동 관리로 99.999999999% 내구성 ✅
- **확장성**: 초 단위 자동 스케일링 ✅

**B. Aurora Serverless for Aurora PostgreSQL**
- **호환성 문제**: MySQL → PostgreSQL 마이그레이션 복잡 ❌
- **애플리케이션 수정**: 쿼리 문법 및 드라이버 변경 필요 ❌

**C. 단일 대형 MySQL 데이터베이스**
- **확장성 한계**: 단일 DB로는 수평 확장 불가 ❌
- **단일 장애점**: 고가용성 부족 ❌
- **수동 관리 지속**: 여전히 수동 운영 필요 ❌

**D. EC2 Auto Scaling + 데이터베이스**
- **복잡성**: 데이터베이스 상태 관리 복잡 ❌
- **데이터 일관성**: Auto Scaling으로 DB 관리 어려움 ❌
- **운영 부담**: 여전히 DB 관리 필요 ❌

### Aurora Serverless 장점

```yaml
자동 용량 관리:
- 트래픽에 따라 자동 확장/축소
- 미사용 시 일시 정지로 비용 절약
- 초 단위 반응 속도

운영 간소화:
- 복제 자동 관리
- 백업 자동화
- 패치 자동 적용
- 모니터링 내장

성능 및 내구성:
- MySQL 대비 5배 성능
- 자동 장애조치
- 스토리지 자동 복구
- 99.99% 가용성
```
