## Question #269
An ecommerce company has noticed performance degradation of its Amazon RDS based web application. 
The performance degradation is attributed to an increase in the number of read-only SQL queries triggered by business analysts. 
A solutions architect needs to solve the problem with minimal changes to the existing web application.

What should the solutions architect recommend?

A. Export the data to Amazon DynamoDB and have the business analysts run their queries.

B. Load the data into Amazon ElastiCache and have the business analysts run their queries.

C. Create a read replica of the primary database and have the business analysts run their queries.

D. Copy the data into an Amazon Redshift cluster and have the business analysts run their queries.

## Question #269 분석

### 요구사항
- **RDS 기반 웹 애플리케이션** 성능 저하
- **비즈니스 애널리스트의 읽기 전용 쿼리** 증가가 원인
- **기존 웹 애플리케이션 최소 변경**

### 선택지 분석

**A. DynamoDB로 데이터 내보내기**
- **대규모 변경**: 관계형 → NoSQL 데이터 변환 복잡 ❌
- **실시간 동기화**: 지속적인 데이터 동기화 문제 ❌
- **쿼리 한계**: 복잡한 분석 쿼리에 부적합 ❌

**B. ElastiCache로 데이터 로드**
- **용도 불일치**: 캐시는 분석 쿼리용이 아님 ❌
- **데이터 크기 제한**: 전체 데이터셋 로드에 부적합 ❌
- **분석 기능 부족**: 복잡한 SQL 분석 불가 ❌

**C. Read Replica 생성 + 애널리스트 쿼리 분리** ⭐
- **완벽한 분리**: 운영 DB와 분석 워크로드 분리 ✅
- **최소 변경**: 웹 애플리케이션 코드 변경 불필요 ✅
- **동일 기능**: MySQL Read Replica로 모든 SQL 쿼리 지원 ✅
- **자동 동기화**: 실시간 데이터 복제로 최신 데이터 보장 ✅

**D. Redshift 클러스터 복사**
- **과도한 솔루션**: 단순 읽기 분리에 데이터 웨어하우스 과잉 ❌
- **지속적 복사**: 실시간 동기화 복잡성 ❌
- **비용 증가**: 불필요한 고비용 솔루션 ❌

### Read Replica 솔루션

```yaml
구현 과정:
1. Primary RDS에서 Read Replica 생성
2. 비즈니스 애널리스트에게 Read Replica 엔드포인트 제공
3. 웹 애플리케이션은 Primary DB 그대로 사용
4. 분석 쿼리는 Read Replica에서 실행

효과:
- Primary DB 부하 완전 분리
- 웹 애플리케이션 성능 복구
- 애널리스트는 동일한 SQL 환경 유지
```
