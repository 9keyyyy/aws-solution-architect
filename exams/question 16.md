## Question #16
A company hosts a data lake on AWS. 
The data lake consists of data in Amazon S3 and Amazon RDS for PostgreSQL. 
The company needs a reporting solution that provides data visualization and includes all the data sources within the data lake. 
Only the company's management team should have full access to all the visualizations. 
The rest of the company should have only limited access.

Which solution will meet these requirements?

A. Create an analysis in Amazon QuickSight. Connect all the data sources and create new datasets. Publish dashboards to visualize the data. Share the dashboards with the appropriate IAM roles.

B. Create an analysis in Amazon QuickSight. Connect all the data sources and create new datasets. Publish dashboards to visualize the data. Share the dashboards with the appropriate users and groups.

C. Create an AWS Glue table and crawler for the data in Amazon S3. Create an AWS Glue extract, transform, and load (ETL) job to produce reports. Publish the reports to Amazon S3. Use S3 bucket policies to limit access to the reports.

D. Create an AWS Glue table and crawler for the data in Amazon S3. Use Amazon Athena Federated Query to access data within Amazon RDS for PostgreSQL. Generate reports by using Amazon Athena. Publish the reports to Amazon S3. Use S3 bucket policies to limit access to the reports.

## Question #16 분석

### ✅ 요구사항
- 데이터 레이크 (S3 + RDS PostgreSQL) 통합 리포팅
- **데이터 시각화** 제공
- **모든 데이터 소스** 포함
- **관리팀**: 모든 시각화 전체 접근
- **일반 직원**: 제한된 접근

### ✅ 선택지 분석

**A. QuickSight + IAM 역할 공유**
- QuickSight 시각화 ✅
- 모든 데이터 소스 연결 ✅
- **IAM 역할 공유는 부적절** ❌
- QuickSight는 사용자/그룹 기반 권한 관리

**B. QuickSight + 사용자/그룹 공유** ⭐
- QuickSight 네이티브 시각화 ✅
- S3 + RDS 직접 연결 ✅
- **사용자/그룹 기반 세밀한 권한 제어** ✅
- 대시보드 레벨 접근 제어

**C. Glue ETL + S3 리포트**
- 시각화 기능 없음 ❌
- 정적 리포트만 생성
- ETL 복잡성 증가
- 인터랙티브 대시보드 불가

**D. Athena + S3 리포트**
- 시각화 기능 없음 ❌
- 쿼리 결과만 제공
- 리포트 형태만 가능
- 대시보드 기능 부족

### 📋 서비스별 기능 비교

### **Amazon QuickSight**
```yaml
데이터 시각화:
  - 인터랙티브 대시보드
  - 차트, 그래프, 테이블
  - 드릴다운 기능
  - 실시간 데이터 새로고침

데이터 소스 연결:
  - Amazon S3 (직접 연결)
  - Amazon RDS PostgreSQL (직접 연결)
  - 여러 데이터 소스 통합
  - 실시간 쿼리 가능

권한 관리:
  - 사용자/그룹 기반
  - 대시보드 레벨 제어
  - 행/열 레벨 보안 (RLS)
  - 세밀한 접근 제어
```

### **AWS Glue vs QuickSight**
```yaml
AWS Glue:
  - ETL 처리 도구 ❌
  - 시각화 기능 없음
  - 데이터 변환/준비 전용
  - 정적 리포트 생성

QuickSight:
  - 비즈니스 인텔리전스 도구 ✅
  - 네이티브 시각화
  - 인터랙티브 대시보드
  - 실시간 분석
```

### **Amazon Athena vs QuickSight**
```yaml
Amazon Athena:
  - 서버리스 쿼리 엔진 ❌
  - SQL 쿼리 결과만 제공
  - 시각화 기능 없음
  - 개발자 중심 도구

QuickSight:
  - 비즈니스 사용자 친화적 ✅
  - 시각적 차트/대시보드
  - 클릭 기반 분석
  - 관리자/일반 사용자 모두 사용
```

### 🔄 QuickSight 권한 관리 구조

### **사용자/그룹 기반 권한**
```yaml
관리팀 그룹:
  - 모든 대시보드 접근
  - 편집 권한
  - 데이터셋 생성/수정
  - 고급 기능 사용

일반 직원 그룹:
  - 제한된 대시보드만 접근
  - 읽기 전용 권한
  - 필터링된 데이터 뷰
  - 기본 시각화만 조회
```

### **IAM 역할 vs 사용자/그룹**
```yaml
IAM 역할 공유 (부적절):
  - AWS 리소스 접근용
  - QuickSight 내부 권한 관리 불가
  - 세밀한 제어 어려움
  - 사용자별 개인화 불가

QuickSight 사용자/그룹 (적절):
  - QuickSight 네이티브 권한
  - 대시보드별 세밀한 제어
  - 행/열 레벨 보안 가능
  - 사용자 경험 개인화
```
