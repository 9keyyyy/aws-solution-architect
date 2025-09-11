## Question #999
A company is creating a prototype of an ecommerce website on AWS. The website consists of an Application Load Balancer, an Auto Scaling group of Amazon EC2 instances for web servers, and an Amazon RDS for MySQL DB instance that runs with the Single-AZ configuration.

The website is slow to respond during searches of the product catalog. The product catalog is a group of tables in the MySQL database that the company does not update frequently. A solutions architect has determined that the CPU utilization on the DB instance is high when product catalog searches occur.

What should the solutions architect recommend to improve the performance of the website during searches of the product catalog?

A. Migrate the product catalog to an Amazon Redshift database. Use the COPY command to load the product catalog tables.

B. Implement an Amazon ElastiCache for Redis cluster to cache the product catalog. Use lazy loading to populate the cache.

C. Add an additional scaling policy to the Auto Scaling group to launch additional EC2 instances when database response is slow.

D. Turn on the Multi-AZ configuration for the DB instance. Configure the EC2 instances to throttle the product catalog queries that are sent to the database.

## Question #999 분석

### 문제 상황
- **이커머스 웹사이트** 프로토타입
- **제품 카탈로그 검색** 시 느린 응답
- **MySQL DB의 CPU 사용률 높음**
- 제품 카탈로그는 **자주 업데이트되지 않음**

### 핵심 요구사항
- **제품 검색 성능 향상**
- **DB CPU 부하 감소**
- **읽기 집약적 워크로드** 최적화

### 선택지 분석

**A. Redshift로 마이그레이션**
- Redshift: 데이터 웨어하우스 (분석용) ❌
- 실시간 웹사이트 쿼리에 부적합 ❌
- 과도한 솔루션 ❌

**B. ElastiCache Redis + Lazy Loading** ⭐
- 자주 업데이트 안되는 데이터 → 캐싱 최적 ✅
- DB CPU 부하 감소 ✅
- 응답 시간 대폭 개선 ✅
- 비용 효율적 ✅

**C. EC2 Auto Scaling 추가**
- 문제는 DB 성능인데 웹서버 확장 → 근본 해결 안됨 ❌
- DB 병목은 그대로 유지 ❌

**D. Multi-AZ + 쿼리 throttling**
- Multi-AZ: 고가용성용, 성능 향상 아님 ❌
- Throttling: 성능 저하 가능성 ❌

### ElastiCache 캐싱 전략

**Lazy Loading**
```yaml
동작:
1. 애플리케이션이 캐시 확인
2. 캐시 미스 → DB 조회
3. 결과를 캐시에 저장
4. 다음 요청시 캐시에서 반환

장점:
- 필요한 데이터만 캐싱
- 메모리 효율적
- 자주 접근하는 데이터 최적화
```
