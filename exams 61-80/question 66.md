## Question #66
A company has an application that generates a large number of files, each approximately 5 MB in size. 
The files are stored in Amazon S3. 
Company policy requires the files to be stored for 4 years before they can be deleted. 
Immediate accessibility is always required as the files contain critical business data that is not easy to reproduce. 
The files are frequently accessed in the first 30 days of the object creation but are rarely accessed after the first 30 days.

Which storage solution is MOST cost-effective?

A. Create an S3 bucket lifecycle policy to move files from S3 Standard to S3 Glacier 30 days from object creation. Delete the files 4 years after object creation.

B. Create an S3 bucket lifecycle policy to move files from S3 Standard to S3 One Zone-Infrequent Access (S3 One Zone-IA) 30 days from object creation. Delete the files 4 years after object creation.

C. Create an S3 bucket lifecycle policy to move files from S3 Standard to S3 Standard-Infrequent Access (S3 Standard-IA) 30 days from object creation. Delete the files 4 years after object creation.

D. Create an S3 bucket lifecycle policy to move files from S3 Standard to S3 Standard-Infrequent Access (S3 Standard-IA) 30 days from object creation. Move the files to S3 Glacier 4 years after object creation.

## Question #66 분석

### ✅ 요구사항
- **대량 파일 생성** (각각 약 5MB)
- **4년간 보관** 후 삭제 가능
- **즉시 접근성 항상 필요** (중요 비즈니스 데이터)
- **재생산 어려운 데이터**
- **접근 패턴**: 최초 30일 자주 접근, 이후 거의 접근 안 함

### ✅ 선택지 분석

**A. S3 Standard → S3 Glacier (30일 후) → 삭제 (4년 후)**
- **Glacier 문제**: 즉시 접근성 불가능 ❌
- **검색 시간**: 분~시간 소요 (요구사항 위반) ❌
- **즉시 접근 필요**: "Immediate accessibility is always required" ❌

**B. S3 Standard → S3 One Zone-IA (30일 후) → 삭제 (4년 후)**
- **단일 AZ**: 가용성 및 내구성 위험 ❌
- **중요 데이터**: "재생산 어려운 데이터"에 부적합 ❌
- **즉시 접근**: 가능하지만 위험도 높음 ⚠️

**C. S3 Standard → S3 Standard-IA (30일 후) → 삭제 (4년 후)** ⭐
- **즉시 접근**: 밀리초 단위 접근 가능 ✅
- **Multi-AZ**: 높은 가용성 및 내구성 ✅
- **비용 효율**: 30일 후 스토리지 비용 절약 ✅
- **요구사항 충족**: 모든 조건 만족 ✅

**D. S3 Standard → S3 Standard-IA (30일) → S3 Glacier (4년)**
- **논리적 오류**: 4년 후 삭제해야 하는데 Glacier로 이동 ❌
- **불필요한 단계**: 삭제 시점에 추가 이동 ❌
- **요구사항 위반**: 4년 후 삭제 vs Glacier 이동 모순 ❌

### 📋 핵심 개념 정리

#### **S3 스토리지 클래스 비교**
```yaml
S3 Standard:
  접근성: 밀리초 단위 즉시 접근
  가용성: 99.99%
  내구성: 11 9s (99.999999999%)
  AZ: Multi-AZ
  비용: 가장 높음

S3 Standard-IA:
  접근성: 밀리초 단위 즉시 접근 ✅
  가용성: 99.9%
  내구성: 11 9s
  AZ: Multi-AZ ✅
  비용: Standard 대비 ~40% 절약

S3 One Zone-IA:
  접근성: 밀리초 단위 즉시 접근
  가용성: 99.5% (단일 AZ 위험)
  내구성: 11 9s (단일 AZ 내에서)
  AZ: Single-AZ ❌
  비용: Standard-IA 대비 ~20% 추가 절약

S3 Glacier:
  접근성: 분~시간 소요 ❌
  가용성: 99.99%
  내구성: 11 9s
  AZ: Multi-AZ
  비용: 매우 저렴
```

#### **즉시 접근성 요구사항**
```yaml
요구사항: "Immediate accessibility is always required"

충족하는 클래스:
  ✅ S3 Standard (밀리초)
  ✅ S3 Standard-IA (밀리초)
  ✅ S3 One Zone-IA (밀리초)

충족하지 않는 클래스:
  ❌ S3 Glacier (1분~12시간)
  ❌ S3 Glacier Deep Archive (12~48시간)
```

#### **데이터 중요성 고려**
```yaml
"Critical business data that is not easy to reproduce"

적합한 옵션:
  ✅ Multi-AZ 스토리지 클래스
  ✅ 높은 가용성 보장
  
부적합한 옵션:
  ❌ Single-AZ (One Zone-IA)
  ❌ 단일 장애점 위험
```

#### **정답 시나리오 (C번)**
```yaml
Lifecycle 정책 설정:

1. 객체 생성 (0일)
   - S3 Standard 저장
   - 빈번한 접근 대응
   
2. 30일 후 자동 전환
   - S3 Standard-IA로 이동
   - 스토리지 비용 ~40% 절약
   - 즉시 접근성 유지
   
3. 4년 후 자동 삭제
   - 객체 완전 삭제
   - 정책 요구사항 충족

비용 분석 (1TB 기준):
  S3 Standard (30일): $23 × 1개월 = $23
  S3 Standard-IA (47개월): $12.5 × 47개월 = $587.5
  총 비용: $610.5

vs 전체 Standard 유지:
  S3 Standard (48개월): $23 × 48개월 = $1,104
  
절약액: $493.5 (약 45% 절약)
```
