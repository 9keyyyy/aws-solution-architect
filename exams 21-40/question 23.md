## Question #23
A company is storing backup files by using Amazon S3 Standard storage. 
The files are accessed frequently for 1 month. 
However, the files are not accessed after 1 month. 
The company must keep the files indefinitely.

Which storage solution will meet these requirements MOST cost-effectively?

A. Configure S3 Intelligent-Tiering to automatically migrate objects.

B. Create an S3 Lifecycle configuration to transition objects from S3 Standard to S3 Glacier Deep Archive after 1 month.

C. Create an S3 Lifecycle configuration to transition objects from S3 Standard to S3 Standard-Infrequent Access (S3 Standard-IA) after 1 month.

D. Create an S3 Lifecycle configuration to transition objects from S3 Standard to S3 One Zone-Infrequent Access (S3 One Zone-IA) after 1 month.

## Question #23 분석

### ✅ 요구사항
- 백업 파일을 **S3 Standard**에 저장
- **첫 1개월**: 자주 액세스
- **1개월 후**: 액세스 안함
- **무기한 보관** 필요
- **가장 비용 효율적인** 솔루션

### ✅ 선택지 분석

**A. S3 Intelligent-Tiering**
- **예측 가능한 패턴**: 1개월 후 액세스 안함 ❌
- Intelligent-Tiering은 예측 불가능한 패턴용
- 명확한 패턴이 있을 때는 비효율적
- 모니터링 비용 불필요

**B. S3 Standard → S3 Glacier Deep Archive** ⭐
- **장기 보관**: Deep Archive는 최저 비용 스토리지 ✅
- **1개월 후 액세스 없음**: 완벽히 부합 ✅
- **무기한 보관**: 아카이브 스토리지 최적 ✅
- **비용 효율성**: 가장 저렴한 장기 보관 ✅

**C. S3 Standard → S3 Standard-IA**
- **비자주 액세스용**: 가끔 액세스하는 데이터용 ❌
- **1개월 후 액세스 없음**: IA는 여전히 비쌈
- 장기 보관에는 과도한 비용

**D. S3 Standard → S3 One Zone-IA**
- **단일 AZ**: 백업 데이터의 내구성 위험 ❌
- **비용**: Standard-IA보다 저렴하지만 여전히 높음
- 장기 보관에 부적합


### 📊 각 스토리지 클래스 특성 비교

### **액세스 특성**
```yaml
S3 Standard:
  - 즉시 액세스 가능
  - 검색 비용 없음
  - 첫 1개월에 최적

S3 Standard-IA:
  - 즉시 액세스 가능
  - 검색 비용 있음 ($0.01/GB)
  - 가끔 액세스용

S3 Glacier Deep Archive:
  - 복원 시간: 12-48시간
  - 검색 비용 있음 ($0.02/GB)
  - 장기 보관 전용 ✅
```
