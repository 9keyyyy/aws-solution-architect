## Question #268
A gaming company has a web application that displays scores. 
The application runs on Amazon EC2 instances behind an Application Load Balancer. 
The application stores data in an Amazon RDS for MySQL database. 
Users are starting to experience long delays and interruptions that are caused by database read performance. 
The company wants to improve the user experience while minimizing changes to the application’s architecture.

What should a solutions architect do to meet these requirements?

A. Use Amazon ElastiCache in front of the database.

B. Use RDS Proxy between the application and the database.

C. Migrate the application from EC2 instances to AWS Lambda.

D. Migrate the database from Amazon RDS for MySQL to Amazon DynamoDB.

## Question #268 분석

### 요구사항
- **게임 점수 웹 애플리케이션**
- **데이터베이스 읽기 성능** 문제로 지연/중단 발생
- **사용자 경험 개선**
- **아키텍처 변경 최소화**

### 선택지 분석

**A. ElastiCache를 데이터베이스 앞단에 배치** ⭐
- **읽기 성능 개선**: 자주 조회되는 점수 데이터를 메모리에 캐싱 ✅
- **최소 변경**: 애플리케이션 코드에 캐시 로직만 추가 ✅
- **게임 특성 적합**: 점수 데이터는 읽기 중심이고 캐싱 효과 높음 ✅
- **즉시 효과**: 캐시 히트 시 밀리초 응답 시간 ✅

**B. RDS Proxy 사용**
- **연결 관리**: 연결 풀링으로 연결 오버헤드 감소 ⚡
- **제한적 효과**: 읽기 성능 자체 개선 효과는 제한적 ❌
- **근본 해결 부족**: 데이터베이스 부하 자체는 지속 ❌

**C. Lambda로 마이그레이션**
- **대규모 변경**: 전체 애플리케이션 재작성 필요 ❌
- **요구사항 위반**: 아키텍처 변경 최소화와 상충 ❌
- **DB 문제 미해결**: 여전히 동일한 읽기 성능 문제 ❌

**D. DynamoDB로 마이그레이션**
- **대규모 변경**: 데이터베이스 전체 마이그레이션 ❌
- **스키마 재설계**: MySQL → NoSQL 전환으로 대폭 수정 ❌
- **요구사항 위반**: 최소 변경과 정반대 ❌

### ElastiCache 게임 애플리케이션 적용

```yaml
캐싱 전략:
- 리더보드: 상위 점수 목록 캐싱
- 사용자 점수: 개별 점수 정보 캐싱
- 통계 데이터: 게임 통계 캐싱

구현:
1. ElastiCache Redis 클러스터 배치
2. 애플리케이션에 캐시 확인 로직 추가
3. 캐시 미스 시 DB 조회 후 캐시 저장
4. 점수 업데이트 시 캐시 무효화
```
