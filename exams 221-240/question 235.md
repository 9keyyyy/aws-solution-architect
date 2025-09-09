## Question #235
A company is moving its on-premises Oracle database to Amazon Aurora PostgreSQL. 
The database has several applications that write to the same tables. 
The applications need to be migrated one by one with a month in between each migration. 
Management has expressed concerns that the database has a high number of reads and writes. 
The data must be kept in sync across both databases throughout the migration.

What should a solutions architect recommend?

A. Use AWS DataSync for the initial migration. Use AWS Database Migration Service (AWS DMS) to create a change data capture (CDC) replication task and a table mapping to select all tables.

B. Use AWS DataSync for the initial migration. Use AWS Database Migration Service (AWS DMS) to create a full load plus change data capture (CDC) replication task and a table mapping to select all tables.

C. Use the AWS Schema Conversion Tool with AWS Database Migration Service (AWS DMS) using a memory optimized replication instance. Create a full load plus change data capture (CDC) replication task and a table mapping to select all tables.

D. Use the AWS Schema Conversion Tool with AWS Database Migration Service (AWS DMS) using a compute optimized replication instance. Create a full load plus change data capture (CDC) replication task and a table mapping to select the largest tables.

## Question #235 분석

### 요구사항
- **Oracle → Aurora PostgreSQL** 마이그레이션
- **단계적 애플리케이션 마이그레이션** (월간 간격)
- **높은 읽기/쓰기** 부하
- **실시간 데이터 동기화** 필수

### 선택지 분석

**A. DataSync 초기 + DMS CDC만**
- **DataSync 부적합**: 데이터베이스 마이그레이션용 아님 ❌
- **초기 로드 누락**: CDC만으로는 기존 데이터 마이그레이션 불가 ❌

**B. DataSync 초기 + DMS Full Load + CDC**
- **DataSync 부적합**: 데이터베이스에는 DMS 사용해야 함 ❌
- **도구 오해**: DataSync는 파일 동기화 서비스 ❌

**C. SCT + DMS 메모리 최적화 + Full Load + CDC** ⭐
- **SCT**: Oracle → PostgreSQL 스키마 변환 필수 ✅
- **Full Load + CDC**: 기존 데이터 + 실시간 변경분 동기화 ✅
- **메모리 최적화**: 높은 읽기/쓰기 처리에 적합 ✅
- **모든 테이블**: 전체 데이터베이스 마이그레이션 ✅

**D. SCT + DMS 컴퓨팅 최적화 + 대형 테이블만**
- **부분 마이그레이션**: 대형 테이블만 선택하면 불완전 ❌
- **컴퓨팅 최적화**: 메모리 집약적 작업에 부적합 ❌

### 마이그레이션 아키텍처

```yaml
1. 스키마 변환:
   - SCT로 Oracle → PostgreSQL 스키마 변환
   - 데이터 타입 및 함수 매핑

2. 데이터 마이그레이션:
   - DMS Full Load: 기존 모든 데이터 일괄 복사
   - DMS CDC: 실시간 변경분 지속 동기화

3. 단계적 전환:
   - 월별로 애플리케이션 하나씩 Aurora로 전환
   - 전환 중에도 데이터 동기화 유지
```
