## Question #20
A company wants to improve its ability to clone large amounts of production data into a test environment in the same AWS Region. 
The data is stored in Amazon EC2 instances on Amazon Elastic Block Store (Amazon EBS) volumes. 
Modifications to the cloned data must not affect the production environment. 
The software that accesses this data requires consistently high I/O performance.
A solutions architect needs to minimize the time that is required to clone the production data into the test environment.

Which solution will meet these requirements?

A. Take EBS snapshots of the production EBS volumes. Restore the snapshots onto EC2 instance store volumes in the test environment.

B. Configure the production EBS volumes to use the EBS Multi-Attach feature. Take EBS snapshots of the production EBS volumes. Attach the production EBS volumes to the EC2 instances in the test environment.

C. Take EBS snapshots of the production EBS volumes. Create and initialize new EBS volumes. Attach the new EBS volumes to EC2 instances in the test environment before restoring the volumes from the production EBS snapshots.

D. Take EBS snapshots of the production EBS volumes. Turn on the EBS fast snapshot restore feature on the EBS snapshots. Restore the snapshots into new EBS volumes. Attach the new EBS volumes to EC2 instances in the test environment.

## Question #20 분석

### ✅ 요구사항
- **대용량 프로덕션 데이터**를 테스트 환경으로 복제
- 동일 AWS 리전 내에서 진행
- **복제 데이터 수정이 프로덕션에 영향 없어야 함**
- **일관되게 높은 I/O 성능** 필요
- **복제 시간 최소화** 필요

### ✅ 선택지 분석

**A. 스냅샷 → EC2 Instance Store 복원**
- **Instance Store**: 임시 스토리지 ❌
- EC2 재시작 시 데이터 손실
- 테스트 환경에 부적합
- 높은 I/O 성능은 제공하지만 영속성 없음

**B. EBS Multi-Attach + 동일 볼륨 공유**
- **동일 볼륨 공유**: 격리 위반 ❌
- 테스트 수정이 프로덕션 영향
- 요구사항 위배 (독립성 보장 불가)

**C. 스냅샷 → 새 EBS 볼륨 생성 → 복원**
- **Cold 복원**: 초기 성능 저하 ❌
- 첫 액세스 시 S3에서 데이터 로드 필요
- 복원 후 즉시 높은 I/O 성능 보장 불가

**D. 스냅샷 + EBS Fast Snapshot Restore + 새 볼륨** ⭐
- **Fast Snapshot Restore**: 즉시 높은 I/O 성능
- **독립된 새 볼륨**: 프로덕션 격리 보장
- **최소 복제 시간**: 사전 워밍으로 지연 없음

### 📋 EBS 스냅샷 복원 방식 비교

### **일반 EBS 스냅샷 복원 (C번)**
```yaml
복원 과정:
  1. 스냅샷에서 새 EBS 볼륨 생성
  2. 볼륨이 즉시 사용 가능 상태
  3. 하지만 데이터는 S3에 저장된 상태
  
성능 문제:
  - 첫 액세스: S3에서 블록 다운로드 (지연 발생)
  - Cold Start: 초기 I/O 성능 저하
  - 점진적 성능 향상: 블록별로 로컬 캐시
  
복원 시간:
  - 볼륨 생성: 즉시
  - 실제 사용 가능: 데이터 액세스 시점
```

### **EBS Fast Snapshot Restore (D번)**
```yaml
사전 워밍 과정:
  1. 스냅샷에 FSR 활성화
  2. 백그라운드에서 전체 데이터 사전 로드
  3. 모든 블록이 EBS 로컬에 준비 완료
  
성능 장점:
  - 즉시 최대 I/O 성능 제공
  - Cold Start 없음
  - 첫 액세스부터 일관된 성능
  
복원 시간:
  - 볼륨 생성: 즉시
  - 최대 성능: 즉시 (사전 워밍 완료)
```

### 🔄 데이터 복제 시나리오 비교

### **Fast Snapshot Restore 워크플로우**
```
프로덕션 환경:
  EBS 볼륨 (1TB) → 스냅샷 생성
         ↓
  Fast Snapshot Restore 활성화
         ↓ (백그라운드 사전 워밍)
  테스트 환경:
    새 EBS 볼륨 생성 ← 즉시 최대 성능
    EC2 인스턴스 연결 ← 지연 없는 I/O
```

### **일반 스냅샷 복원 워크플로우**
```
프로덕션 환경:
  EBS 볼륨 (1TB) → 스냅샷 생성
         ↓
  테스트 환경:
    새 EBS 볼륨 생성 ← 즉시 생성
    EC2 인스턴스 연결 ← Cold Start
    첫 데이터 액세스 ← S3에서 다운로드 지연
    점진적 성능 향상 ← 블록별 캐싱
```

### **잘못된 접근법들**
```yaml
Instance Store 사용 (A번):
  ❌ 데이터 영속성 없음
  ❌ EC2 중지/재시작 시 데이터 손실
  ❌ 테스트 환경 부적합
  ❌ 백업/복원 불가

EBS Multi-Attach (B번):
  ❌ 동일 볼륨 공유 = 격리 위반
  ❌ 테스트 수정이 프로덕션 영향
  ❌ 파일 시스템 충돌 위험
  ❌ 요구사항 위배
```
