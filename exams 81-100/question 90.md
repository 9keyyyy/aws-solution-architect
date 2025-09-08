## Question #90
A company is using a SQL database to store movie data that is publicly accessible. 
The database runs on an Amazon RDS Single-AZ DB instance. 
A script runs queries at random intervals each day to record the number of new movies that have been added to the database. 
The script must report a final total during business hours.
The company's development team notices that the database performance is inadequate for development tasks when the script is running. 
A solutions architect must recommend a solution to resolve this issue.

Which solution will meet this requirement with the LEAST operational overhead?

A. Modify the DB instance to be a Multi-AZ deployment.

B. Create a read replica of the database. Configure the script to query only the read replica.

C. Instruct the development team to manually export the entries in the database at the end of each day.

D. Use Amazon ElastiCache to cache the common queries that the script runs against the database.

## Question #90 분석

### ✅ 요구사항
- **SQL 데이터베이스**: RDS Single-AZ에서 영화 데이터 저장
- **스크립트 쿼리**: 무작위 간격으로 신규 영화 수 집계
- **성능 문제**: 스크립트 실행 중 개발팀 작업 성능 저하
- **최소 운영 오버헤드** 솔루션 필요

### ✅ 핵심 문제
```yaml
현재 상황:
❌ 단일 DB 인스턴스에 읽기 집중
❌ 스크립트와 개발 작업이 리소스 경합
❌ 성능 병목 현상

해결 방향:
✅ 읽기 워크로드 분산
✅ 운영과 개발 작업 분리
✅ 최소한의 변경으로 최대 효과
```

### ✅ 선택지 분석

**A. Multi-AZ 배포로 변경**
- **고가용성 개선**: 장애조치 기능 추가 ✅
- **성능 개선 없음**: 여전히 단일 인스턴스에 모든 부하 ❌
- **문제 미해결**: 읽기 경합 상황 지속 ❌

**B. Read Replica 생성 + 스크립트 분리** ⭐
- **읽기 워크로드 분리**: 스크립트는 Read Replica 사용 ✅
- **성능 개선**: 개발팀은 Primary DB 독점 사용 ✅
- **최소 변경**: 스크립트 연결 문자열만 수정 ✅
- **운영 오버헤드 최소**: 완전 관리형 서비스 ✅

**C. 수동 데이터 내보내기**
- **수동 작업**: 매일 개발팀 수동 개입 필요 ❌
- **운영 부담 증가**: 자동화 대비 비효율적 ❌
- **확장성 없음**: 장기적 솔루션 부적절 ❌

**D. ElastiCache 캐싱**
- **복잡성 증가**: 캐시 무효화 로직 구현 필요 ❌
- **데이터 일관성**: 실시간 데이터 정확성 이슈 ❌
- **운영 오버헤드**: 캐시 관리 부담 추가 ❌

### 📋 Read Replica 솔루션

**아키텍처 변경**
```yaml
Before:
Primary DB ← 개발팀 + 스크립트 (경합)

After:  
Primary DB ← 개발팀 (전용)
Read Replica ← 스크립트 (분리)

결과:
✅ 성능 문제 해결
✅ 워크로드 분산
✅ 최소 변경
```

**구현 과정**
```yaml
1. Read Replica 생성 (15-30분)
2. 스크립트 연결 문자열 변경
3. 즉시 성능 개선 체감

변경 사항:
- 개발팀: 변경 없음
- 스크립트: DB 엔드포인트만 변경
- 인프라: Read Replica 추가만
```
