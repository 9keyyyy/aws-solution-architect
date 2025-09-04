## Question #30
A development team runs monthly resource-intensive tests on its general purpose Amazon RDS for MySQL DB instance with Performance Insights enabled. 
The testing lasts for 48 hours once a month and is the only process that uses the database. 
The team wants to reduce the cost of running the tests without reducing the compute and memory attributes of the DB instance.

Which solution meets these requirements MOST cost-effectively?

A. Stop the DB instance when tests are completed. Restart the DB instance when required.

B. Use an Auto Scaling policy with the DB instance to automatically scale when tests are completed.

C. Create a snapshot when tests are completed. Terminate the DB instance and restore the snapshot when required.

D. Modify the DB instance to a low-capacity instance when tests are completed. Modify the DB instance again when required.


## Question #30 분석

### ✅ 요구사항
- **월 1회, 48시간** 리소스 집약적 테스트
- **나머지 시간에는 DB 사용 안함** (유일한 프로세스)
- **컴퓨팅/메모리 속성 유지** 필요
- **가장 비용 효율적** 솔루션

### ✅ 선택지 분석

**A. DB 인스턴스 중지/재시작** ⭐
- **완전 비용 절감**: 중지 시 컴퓨팅 비용 0 ✅
- **속성 유지**: 동일 인스턴스 타입으로 재시작 ✅
- **간단한 운영**: Stop/Start만으로 완료 ✅
- **데이터 보존**: 스토리지는 그대로 유지 ✅

**B. Auto Scaling 정책**
- **RDS 제한**: RDS는 Auto Scaling 미지원 
- EC2 기반 데이터베이스가 아님
- 기술적으로 불가능

**C. 스냅샷 생성 후 종료/복원**
- **높은 운영 오버헤드**: 복잡한 절차 
- **복원 시간**: 긴 다운타임 발생 
- **비용**: 스냅샷 스토리지 추가 비용 

**D. 인스턴스 타입 변경**
- **속성 변경**: 컴퓨팅/메모리 속성 변경 위배 
- **다운타임**: 변경 시 서비스 중단 
- **요구사항 위반**

### 📋 RDS 중지/시작 기능

### **RDS Stop 기능**
```yaml
지원 엔진:
  - MySQL, PostgreSQL, MariaDB
  - Oracle, SQL Server
  - Aurora는 별도 정책

중지 시 비용 절감:
  ✅ 컴퓨팅 비용: $0
  ✅ 메모리 비용: $0
  ❌ 스토리지 비용: 계속 과금 (데이터 보존)

자동 재시작:
  - 7일 후 자동 시작 (AWS 정책)
  - 수동으로 더 일찍 시작 가능
```


### 🔄 다른 옵션들의 문제점

### **Auto Scaling (B번)**
```yaml
RDS 제한사항:
  ❌ RDS는 수직 확장만 지원 (인스턴스 크기 변경)
  ❌ Auto Scaling Group 불가
  ❌ EC2 Auto Scaling과 다름

가능한 것:
  - Aurora Serverless (완전히 다른 서비스)
  - Read Replica Auto Scaling
  - 수동 인스턴스 타입 변경
```

### **스냅샷 방식 (C번)**
```yaml
복잡한 운영 절차:
  1. 테스트 완료 후 스냅샷 생성 (30분-2시간)
  2. DB 인스턴스 삭제
  3. 테스트 필요시 스냅샷에서 복원 (1-3시간)
  4. 새 엔드포인트로 애플리케이션 재설정

추가 비용:
  - 스냅샷 스토리지 비용
  - 복원 시간 동안 대기 비용
  - 운영 복잡성으로 인한 인건비
```

### **인스턴스 크기 변경 (D번)**
```yaml
요구사항 위배:
  ❌ "컴퓨팅/메모리 속성 유지" 위반
  ❌ 테스트 성능에 영향
  ❌ 리소스 집약적 테스트에 부적합

운영 복잡성:
  - 매월 2번 인스턴스 변경
  - 각 변경시 다운타임 발생
  - 성능 테스트 결과 신뢰성 저하
```
