## Question #14
A company runs an ecommerce application on Amazon EC2 instances behind an Application Load Balancer. 
The instances run in an Amazon EC2 Auto Scaling group across multiple Availability Zones. 
The Auto Scaling group scales based on CPU utilization metrics. 
The ecommerce application stores the transaction data in a MySQL 8.0 database that is hosted on a large EC2 instance.
The database's performance degrades quickly as application load increases. 
The application handles more read requests than write transactions. 
The company wants a solution that will automatically scale the database to meet the demand of unpredictable read workloads while maintaining high availability.

Which solution will meet these requirements?

A. Use Amazon Redshift with a single node for leader and compute functionality.

B. Use Amazon RDS with a Single-AZ deployment Configure Amazon RDS to add reader instances in a different Availability Zone.

C. Use Amazon Aurora with a Multi-AZ deployment. Configure Aurora Auto Scaling with Aurora Replicas.

D. Use Amazon ElastiCache for Memcached with EC2 Spot Instances.


### ✅ 요구사항
- 자동으로 확장되는 데이터베이스 솔루션
- 예측 불가능한 **읽기 워크로드** 대응
- **고가용성** 유지
- **읽기 요청이 쓰기보다 많음**

### ✅ 선택지 분석

**A. Amazon Redshift (단일 노드)**
- 데이터 웨어하우스 솔루션 (OLTP 부적합)
- 단일 노드는 고가용성 제공 불가 ❌
- 트랜잭션 처리 최적화 되지 않음

**B. Amazon RDS Single-AZ + Reader 인스턴스**
- Single-AZ는 **고가용성 부족** ❌
- 수동 스케일링 필요
- 자동 확장 미지원

**C. Amazon Aurora Multi-AZ + Auto Scaling + Aurora Replicas** ⭐
- **Aurora Auto Scaling** 지원
- 최대 15개 읽기 복제본 자동 생성
- Multi-AZ 고가용성 보장
- **읽기 워크로드에 최적화**

**D. ElastiCache for Memcached + EC2 Spot**
- 캐싱 솔루션 (영구 저장소 아님) ❌
- Spot 인스턴스는 가용성 불안정
- 트랜잭션 데이터 저장 부적합

### 📋 서비스별 기능 비교

### **Amazon Aurora Auto Scaling**
```yaml
읽기 복제본 자동 확장:
  - 최소: 1개 복제본
  - 최대: 15개 복제본
  - 메트릭 기반 자동 스케일링
  - CPU, 연결 수, 평균 응답시간 기반

고가용성:
  - Multi-AZ 배포
  - 자동 장애 조치 (30초 이내)
  - 스토리지 자동 복구

읽기 성능:
  - 읽기 전용 엔드포인트
  - 자동 로드 밸런싱
  - 쓰기와 읽기 분리
```

### **Amazon RDS vs Aurora 비교**
```yaml
RDS MySQL:
  - 수동 읽기 복제본 관리 ❌
  - 최대 5개 읽기 복제본
  - Single-AZ 옵션은 고가용성 부족

Aurora MySQL:
  - 자동 스케일링 지원 ✅
  - 최대 15개 읽기 복제본 ✅
  - 내장 Multi-AZ 고가용성 ✅
  - 더 빠른 장애 복구
```

### 🔄 Aurora Auto Scaling 프로세스

### **읽기 워크로드 증가 시나리오**
```
1단계: 메트릭 모니터링
   CPU 사용률: 70% → 80% → 90%
   연결 수: 100 → 200 → 300
   
2단계: 스케일링 트리거
   CloudWatch 임계값 초과 감지
   
3단계: 복제본 자동 생성
   Aurora Replica-3 생성 시작
   
4단계: 로드 밸런싱
   읽기 트래픽 자동 분산
   
5단계: 부하 감소 시
   불필요한 복제본 자동 제거
```

### **Multi-AZ 고가용성 구성**
```yaml
Primary Cluster (AZ-1a):
  Writer: aurora-writer-instance
  Reader: aurora-reader-1
  
Secondary Cluster (AZ-1b):
  Reader: aurora-reader-2
  Reader: aurora-reader-3 (Auto Scaling)
  
Tertiary Cluster (AZ-1c):
  Reader: aurora-reader-4 (Auto Scaling)
  
자동 장애 조치:
  Writer 장애 시 → Reader가 Writer로 승격
  복구 시간: 30초 이내
```
