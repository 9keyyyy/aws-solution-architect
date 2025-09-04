## Question #22
A solutions architect is using Amazon S3 to design the storage architecture of a new digital media application. 
The media files must be resilient to the loss of an Availability Zone. 
Some files are accessed frequently while other files are rarely accessed in an unpredictable pattern. 
The solutions architect must minimize the costs of storing and retrieving the media files.

Which storage option meets these requirements?

A. S3 Standard

B. S3 Intelligent-Tiering

C. S3 Standard-Infrequent Access (S3 Standard-IA)

D. S3 One Zone-Infrequent Access (S3 One Zone-IA)

## Question #22 분석

### ✅ 요구사항
- 디지털 미디어 애플리케이션용 스토리지
- **AZ 손실에 대한 복원력** 필요
- **예측 불가능한 액세스 패턴** (자주/거의 액세스 혼재)
- **저장 및 검색 비용 최소화**

### ✅ 선택지 분석

**A. S3 Standard**
- **AZ 복원력**: 3개 AZ에 복제 ✅
- **예측 불가능한 패턴**: 처리 가능 ✅
- **비용 최적화**: 비자주 액세스 파일에 비효율적 
- 모든 파일에 동일한 높은 비용

**B. S3 Intelligent-Tiering** ⭐
- **AZ 복원력**: 3개 AZ에 복제 ✅
- **예측 불가능한 패턴**: 자동 최적화 ✅
- **비용 최적화**: 액세스 패턴 기반 자동 이동 ✅
- 스토리지 클래스 자동 관리

**C. S3 Standard-IA**
- **AZ 복원력**: 3개 AZ에 복제 ✅
- **예측 불가능한 패턴**: 자주 액세스 파일에 비효율적 
- 모든 파일을 IA로 고정 (유연성 부족)

**D. S3 One Zone-IA**
- **AZ 복원력**: 단일 AZ만 사용 
- AZ 손실 시 데이터 완전 손실
- 요구사항 위배

### 📋 S3 스토리지 클래스 비교

### **AZ 복원력 비교**
```yaml
Multi-AZ 스토리지:
  - S3 Standard: 3개 AZ 복제 ✅
  - S3 Intelligent-Tiering: 3개 AZ 복제 ✅
  - S3 Standard-IA: 3개 AZ 복제 ✅

Single-AZ 스토리지:
  - S3 One Zone-IA: 1개 AZ만 ❌
  - AZ 장애 시 데이터 손실
  - 복원력 요구사항 미충족
```

### **액세스 패턴별 비용 효율성**
```yaml
자주 액세스하는 파일:
  - S3 Standard: 최적 ✅
  - S3 Standard-IA: 검색 비용 높음 ❌
  - S3 One Zone-IA: 검색 비용 높음 ❌

거의 액세스하지 않는 파일:
  - S3 Standard: 스토리지 비용 높음 ❌
  - S3 Standard-IA: 최적 ✅
  - S3 One Zone-IA: 더 저렴하지만 복원력 부족
```

### 🔄 S3 Intelligent-Tiering 동작 원리

### **자동 계층 이동**
```yaml
액세스 모니터링:
  - 각 객체의 액세스 패턴 30일간 추적
  - 자주 액세스: Frequent Access Tier 유지
  - 30일 미액세스: Infrequent Access Tier로 이동
  - 90일 미액세스: Archive Instant Access Tier
  - 180일 미액세스: Archive Access Tier (선택사항)

자동 복원:
  - IA 계층 파일 액세스 시 즉시 Frequent로 이동
  - 다운타임 없는 투명한 이동
  - 사용자 개입 불필요
```

### **비용 구조**
```yaml
스토리지 비용:
  - Frequent Tier: Standard와 동일
  - Infrequent Tier: Standard-IA와 동일
  - 자동 최적화로 평균 20-30% 절약

관리 비용:
  - 모니터링 비용: $0.0025 per 1,000 objects
  - 매우 저렴한 관리 비용
  
검색 비용:
  - Frequent Tier: 무료
  - Infrequent Tier: $0.01 per GB
```
