## Question #240
A company previously migrated its data warehouse solution to AWS. 
The company also has an AWS Direct Connect connection. 
Corporate office users query the data warehouse using a visualization tool. 
The average size of a query returned by the data warehouse is 50 MB and each webpage sent by the visualization tool is approximately 500 KB. 
Result sets returned by the data warehouse are not cached.

Which solution provides the LOWEST data transfer egress cost for the company?

A. Host the visualization tool on premises and query the data warehouse directly over the internet.

B. Host the visualization tool in the same AWS Region as the data warehouse. Access it over the internet.

C. Host the visualization tool on premises and query the data warehouse directly over a Direct Connect connection at a location in the same AWS Region.

D. Host the visualization tool in the same AWS Region as the data warehouse and access it over a Direct Connect connection at a location in the same Region.

## Question #240 분석

### 요구사항
- **데이터 웨어하우스**: AWS에 마이그레이션 완료
- **Direct Connect 연결** 보유
- **쿼리 결과**: 50MB (캐시 없음)
- **웹페이지**: 500KB
- **최저 데이터 전송 비용**

### 데이터 전송 비용 분석

```yaml
AWS 데이터 전송 비용:
인터넷 아웃바운드: $0.09/GB
Direct Connect 아웃바운드: $0.02/GB (약 75% 절약)
인바운드: 무료
동일 리전 내: 무료
```

### 선택지 분석

**A. 온프레미스 시각화 툴 + 인터넷 쿼리**
- **높은 비용**: 50MB 쿼리 결과 × 인터넷 요금 ❌
- **대용량 전송**: 가장 비싼 데이터 전송 경로 ❌

**B. AWS 시각화 툴 + 인터넷 접근**
- **웹페이지 전송**: 500KB × 인터넷 요금 ✅
- **내부 쿼리**: 데이터웨어하우스↔시각화툴 간 무료 ✅

**C. 온프레미스 시각화 툴 + Direct Connect 쿼리**
- **중간 비용**: 50MB × Direct Connect 요금 ⚡
- **Direct Connect 활용**: 인터넷 대비 75% 절약 ✅

**D. AWS 시각화 툴 + Direct Connect 접근** ⭐
- **최소 전송**: 500KB 웹페이지만 Direct Connect로 전송 ✅
- **내부 쿼리 무료**: 50MB 쿼리는 AWS 내부에서 무료 ✅
- **최적 조합**: 가장 적은 데이터를 가장 저렴한 경로로 ✅

### 비용 계산 예시

```yaml
옵션별 월간 비용 (1000회 쿼리 기준):

A안: 50MB × 1000 × $0.09/GB = $4.50
B안: 0.5MB × 1000 × $0.09/GB = $0.045
C안: 50MB × 1000 × $0.02/GB = $1.00
D안: 0.5MB × 1000 × $0.02/GB = $0.01

D안이 압도적으로 저렴
```
