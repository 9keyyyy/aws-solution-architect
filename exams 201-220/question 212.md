## Question #212
A company needs to export its database once a day to Amazon S3 for other teams to access. 
The exported object size varies between 2 GB and 5 GB. 
The S3 access pattern for the data is variable and changes rapidly. 
The data must be immediately available and must remain accessible for up to 3 months. 
The company needs the most cost-effective solution that will not increase retrieval time.

Which S3 storage class should the company use to meet these requirements?

A. S3 Intelligent-Tiering
B. S3 Glacier Instant Retrieval
C. S3 Standard
D. S3 Standard-Infrequent Access (S3 Standard-IA)

## Question #212 분석

### 요구사항
- **일일 데이터베이스 백업** → S3
- **파일 크기**: 2-5GB
- **접근 패턴**: 가변적이고 빠르게 변화
- **즉시 사용 가능** 필요
- **3개월간 접근** 필요
- **비용 효율적** + **검색 시간 증가 없음**

### 선택지 분석

**A. S3 Intelligent-Tiering** ⭐
- **가변 접근 패턴**: 자동으로 접근 빈도에 따라 계층 이동 ✅
- **즉시 접근**: 모든 계층에서 밀리초 검색 시간 ✅
- **비용 최적화**: 사용 패턴에 따라 자동 비용 절감 ✅
- **관리 부담 없음**: 완전 자동화 ✅

**B. S3 Glacier Instant Retrieval**
- **즉시 검색**: 밀리초 검색 지원 ✅
- **고정 비용**: 접근 패턴 변화 시 비효율 ❌
- **90일 최소 저장**: 조기 삭제 시 추가 비용 ❌

**C. S3 Standard**
- **즉시 접근**: 밀리초 검색 ✅
- **비용 비효율**: 가변 접근 패턴 시 과도한 비용 ❌
- **최적화 없음**: 사용 패턴과 무관한 고정 비용 ❌

**D. S3 Standard-IA**
- **30일 최소 저장**: 조기 삭제 시 추가 비용 ❌
- **접근 요금**: 빈번한 접근 시 비용 증가 ❌
- **가변 패턴 부적합**: 접근 빈도 예측 어려움 ❌

### Intelligent-Tiering 작동 방식

```yaml
자동 최적화:
- 30일 미접근 → Infrequent Access
- 90일 미접근 → Archive Instant Access
- 180일 미접근 → Archive Access (선택사항)

비용 효과:
- 접근 빈도 높음: Standard 수준 성능
- 접근 빈도 낮음: 자동 비용 절감
- 검색 시간: 항상 밀리초 유지
```
