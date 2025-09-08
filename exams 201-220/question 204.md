## Question #204
An online retail company has more than 50 million active customers and receives more than 25,000 orders each day. 
The company collects purchase data for customers and stores this data in Amazon S3. 
Additional customer data is stored in Amazon RDS.

The company wants to make all the data available to various teams so that the teams can perform analytics. 
The solution must provide the ability to manage fine-grained permissions for the data and must minimize operational overhead.

Which solution will meet these requirements?

A. Migrate the purchase data to write directly to Amazon RDS. Use RDS access controls to limit access.

B. Schedule an AWS Lambda function to periodically copy data from Amazon RDS to Amazon S3. Create an AWS Glue crawler. Use Amazon Athena to query the data. Use S3 policies to limit access.

C. Create a data lake by using AWS Lake Formation. Create an AWS Glue JDBC connection to Amazon RDS. Register the S3 bucket in Lake Formation. Use Lake Formation access controls to limit access.

D. Create an Amazon Redshift cluster. Schedule an AWS Lambda function to periodically copy data from Amazon S3 and Amazon RDS to Amazon Redshift. Use Amazon Redshift access controls to limit access.

## Question #204 분석

### 요구사항
- **대규모 데이터**: 5천만+ 고객, 일일 25,000+ 주문
- **다중 데이터 소스**: S3 (구매 데이터) + RDS (고객 데이터)
- **여러 팀의 분석** 접근 필요
- **세밀한 권한 관리**
- **최소 운영 오버헤드**

### 선택지 분석

**A. RDS로 모든 데이터 통합**
- **확장성 한계**: RDS로 대용량 분석 데이터 처리 부적절 ❌
- **비용 증가**: 분석용 RDS 인스턴스 크기 대폭 증가 ❌
- **성능 저하**: OLTP와 OLAP 워크로드 혼재 ❌

**B. Lambda + Glue + Athena + S3 정책**
- **복잡한 권한**: S3 정책만으로 세밀한 제어 어려움 ❌
- **운영 부담**: Lambda 스케줄링 및 ETL 관리 필요 ❌
- **일관성 부족**: 여러 시스템 권한 분산 관리 ❌

**C. AWS Lake Formation + Glue** ⭐
- **통합 데이터 레이크**: S3와 RDS 데이터 중앙 집중화 ✅
- **세밀한 권한**: 테이블/컬럼/행 수준 권한 제어 ✅
- **완전 관리형**: ETL, 카탈로그, 권한 자동 관리 ✅
- **최소 오버헤드**: 설정 후 자동 운영 ✅
- **확장성**: 페타바이트급 데이터 처리 지원 ✅

**D. Redshift + Lambda**
- **데이터웨어하우스**: 분석 성능 우수하지만 비용 높음 ❌
- **운영 복잡성**: 클러스터 관리 + Lambda ETL 필요 ❌
- **권한 제한**: Redshift 권한은 Lake Formation 대비 제한적 ❌

### Lake Formation 장점

```yaml
데이터 통합:
✅ S3 + RDS 통합 뷰
✅ Glue JDBC 커넥션으로 RDS 연결
✅ 자동 스키마 발견 및 카탈로그

권한 관리:
✅ 테이블/컬럼 수준 제어
✅ 행 기반 필터링
✅ 팀별 데이터 접근 제어
✅ 중앙 집중식 권한 관리

분석 도구 통합:
✅ Athena, Redshift Spectrum
✅ EMR, SageMaker
✅ QuickSight, Tableau
```

### 아키텍처

```yaml
1. S3 버킷을 Lake Formation에 등록
2. Glue JDBC로 RDS 연결
3. Glue Crawler로 스키마 자동 발견
4. Lake Formation에서 팀별 권한 설정
5. Athena/EMR로 통합 분석
```
