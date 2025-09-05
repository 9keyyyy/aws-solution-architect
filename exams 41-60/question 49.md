## Question #49
A company stores call transcript files on a monthly basis. 
Users access the files randomly within 1 year of the call, but users access the files infrequently after 1 year. 
The company wants to optimize its solution by giving users the ability to query and retrieve files that are less than 1-year-old as quickly as possible. 
A delay in retrieving older files is acceptable.

Which solution will meet these requirements MOST cost-effectively?

A. Store individual files with tags in Amazon S3 Glacier Instant Retrieval. Query the tags to retrieve the files from S3 Glacier Instant Retrieval.

B. Store individual files in Amazon S3 Intelligent-Tiering. Use S3 Lifecycle policies to move the files to S3 Glacier Flexible Retrieval after 1 year. Query and retrieve the files that are in Amazon S3 by using Amazon Athena. Query and retrieve the files that are in S3 Glacier by using S3 Glacier Select.

C. Store individual files with tags in Amazon S3 Standard storage. Store search metadata for each archive in Amazon S3 Standard storage. Use S3 Lifecycle policies to move the files to S3 Glacier Instant Retrieval after 1 year. Query and retrieve the files by searching for metadata from Amazon S3.

D. Store individual files in Amazon S3 Standard storage. Use S3 Lifecycle policies to move the files to S3 Glacier Deep Archive after 1 year. Store search metadata in Amazon RDS. Query the files from Amazon RDS. Retrieve the files from S3 Glacier Deep Archive.

## Question #49 분석

### ✅ 요구사항
- **월별 통화 기록 파일** 저장
- **1년 내**: 랜덤 액세스, 빠른 조회 필요
- **1년 후**: 비자주 액세스, 지연 허용
- **가장 비용 효율적** 솔루션

### ✅ 선택지 분석

**A. S3 Glacier Instant Retrieval + 태그 쿼리**
- **Glacier Instant Retrieval**: 1년 내 파일에 과도하게 비싼 스토리지 
- **태그 쿼리 한계**: 복잡한 검색 기능 제한적 
- **비용 비효율**: 빈번한 액세스에 아카이브 스토리지 부적합

**B. S3 Intelligent-Tiering + Lifecycle + Athena** ⭐
- **Intelligent-Tiering**: 1년 내 액세스 패턴 자동 최적화 ✅
- **Lifecycle 전환**: 1년 후 자동으로 Glacier로 이동 ✅
- **Athena 쿼리**: 빠른 검색 및 조회 기능 ✅
- **Glacier Select**: 아카이브된 파일도 쿼리 가능 ✅

**C. S3 Standard + 메타데이터 + Glacier Instant Retrieval**
- **Standard 1년**: 비자주 액세스 파일에 과도한 비용 
- **복잡한 메타데이터 관리**: 별도 저장 및 동기화 필요 
- **Glacier Instant Retrieval**: 아카이브 치고는 비쌈

**D. S3 Standard + Glacier Deep Archive + RDS**
- **Deep Archive**: 12-48시간 복원 시간 
- **RDS 운영**: 불필요한 데이터베이스 관리 오버헤드 
- **복잡한 아키텍처**: 과도한 구성 요소

### 📋 S3 Intelligent-Tiering 최적화

#### **액세스 패턴 기반 자동 전환**
```yaml
1년 내 파일:
  - 빈번한 액세스: Frequent Access Tier (Standard 가격)
  - 30일 미액세스: Infrequent Access Tier (40% 할인)
  - 90일 미액세스: Archive Instant Access (70% 할인)

1년 후 파일:
  - Lifecycle으로 Glacier Flexible Retrieval 이동
  - 90% 비용 절약
  - 복원 시간: 1-5분 (지연 허용 범위)
```

#### **Athena를 통한 쿼리 최적화**
```yaml
S3 파일 쿼리:
  - 파일 메타데이터 기반 SQL 쿼리
  - 날짜, 고객 ID, 통화 시간 등으로 검색
  - 서버리스, 사용량 기반 과금
  - 빠른 응답 시간 (초 단위)

Glacier Select:
  - 아카이브된 파일 내용 직접 쿼리
  - 전체 파일 복원 없이 일부만 추출
  - 비용 효율적인 아카이브 액세스
```




### 🔍 다른 옵션들의 문제점

#### **Deep Archive 지연 시간 (D번)**
```yaml
복원 시간:
  - Standard: 12시간
  - Bulk: 48시간
  
사용자 경험:
  - 1년 후에도 당일 접근 불가
  - 비즈니스 요구사항 위배
  - 과도한 지연으로 실용성 떨어짐
```

#### **RDS 메타데이터 관리 (D번)**
```yaml
운영 복잡성:
  - 데이터베이스 패치, 백업
  - 파일-메타데이터 동기화 관리
  - 고가용성 구성 필요
  - 불필요한 운영 오버헤드
```

#### **태그 기반 검색 한계 (A번)**
```yaml
검색 제약:
  - 단순한 키-값 쌍만 지원
  - 복잡한 조건 검색 어려움
  - 범위 검색, 정렬 기능 제한
  - SQL 대비 기능성 부족
```
