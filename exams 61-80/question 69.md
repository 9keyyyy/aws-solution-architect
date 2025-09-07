## Question #69
A company is running a business-critical web application on Amazon EC2 instances behind an Application Load Balancer. 
The EC2 instances are in an Auto Scaling group. 
The application uses an Amazon Aurora PostgreSQL database that is deployed in a single Availability Zone. The company wants the application to be highly available with minimum downtime and minimum loss of data.

Which solution will meet these requirements with the LEAST operational effort?

A. Place the EC2 instances in different AWS Regions. Use Amazon Route 53 health checks to redirect traffic. Use Aurora PostgreSQL Cross-Region Replication.

B. Configure the Auto Scaling group to use multiple Availability Zones. Configure the database as Multi-AZ. Configure an Amazon RDS Proxy instance for the database.

C. Configure the Auto Scaling group to use one Availability Zone. Generate hourly snapshots of the database. Recover the database from the snapshots in the event of a failure.

D. Configure the Auto Scaling group to use multiple AWS Regions. Write the data from the application to Amazon S3. Use S3 Event Notifications to launch an AWS Lambda function to write the data to the database.

## Question #69 분석

### ✅ 요구사항
- **비즈니스 크리티컬** 웹 애플리케이션
- **고가용성**: 최소 다운타임
- **데이터 손실 최소화**: 데이터 보호 필수
- **최소 운영 노력**: 관리 복잡성 최소화
- **현재 상황**: 단일 AZ Aurora PostgreSQL 사용

### ✅ 선택지 분석

**A. 멀티 리전 + Cross-Region Replication**
- **고가용성**: 리전 수준 내결함성 ✅
- **데이터 보호**: 교차 리전 복제로 안전 ✅
- **운영 복잡성**: Route 53 설정, 리전간 관리 ❌
- **비용 증가**: 멀티 리전 운영 비용 높음 ❌
- **과도한 설계**: 요구사항 대비 오버엔지니어링 ❌

**B. 멀티 AZ Auto Scaling + Multi-AZ DB + RDS Proxy** ⭐
- **Auto Scaling 멀티 AZ**: AZ 수준 장애 대응 ✅
- **Multi-AZ Aurora**: 자동 장애조치, 데이터 동기화 ✅
- **RDS Proxy**: 연결 풀링, 장애조치 가속화 ✅
- **최소 운영 노력**: AWS 관리형 서비스 활용 ✅
- **데이터 손실 방지**: 동기식 복제로 RPO=0 ✅

**C. 단일 AZ + 시간별 스냅샷**
- **단일 AZ**: 가용성 요구사항 미충족 ❌
- **스냅샷 복구**: 수동 복구, 긴 다운타임 ❌
- **데이터 손실**: 최대 1시간 데이터 손실 가능 ❌
- **고가용성 위배**: 핵심 요구사항 불만족 ❌

**D. 멀티 리전 + S3 중간 저장**
- **복잡한 아키텍처**: S3 → Lambda → DB 체인 ❌
- **데이터 일관성**: 비동기 처리로 일관성 문제 ❌
- **장애지점 증가**: 여러 서비스 의존성 ❌
- **과도한 설계**: 불필요한 복잡성 ❌

### 📋 핵심 개념 정리

#### **고가용성 설계 원칙**
```yaml
애플리케이션 계층:
  ✅ 멀티 AZ Auto Scaling
  ✅ Application Load Balancer
  ✅ 자동 장애 감지/복구
  ✅ 수평적 확장

데이터베이스 계층:
  ✅ Multi-AZ 배포
  ✅ 자동 장애조치
  ✅ 동기식 복제
  ✅ 백업 자동화

연결 최적화:
  ✅ RDS Proxy 연결 관리
  ✅ 장애조치 시간 단축
  ✅ 연결 풀링 효율화
```

#### **Aurora Multi-AZ 특징**
```yaml
고가용성:
  ✅ 3개 AZ에 6개 복사본
  ✅ 자동 장애 감지 (<30초)
  ✅ 자동 장애조치 (<2분)
  ✅ 읽기 전용 복제본 승격

데이터 보호:
  ✅ RPO = 0 (데이터 손실 없음)
  ✅ 동기식 복제
  ✅ 자동 백업
  ✅ 지속적 백업 to S3

운영 간소화:
  ✅ 완전 관리형 서비스
  ✅ 자동 패치
  ✅ 자동 모니터링
  ✅ 수동 개입 불필요
```

#### **RDS Proxy 장점**
```yaml
연결 관리:
  ✅ 연결 풀링으로 리소스 효율화
  ✅ 연결 한계 방지
  ✅ 데이터베이스 부하 감소

장애조치 개선:
  ✅ 66% 빠른 장애조치
  ✅ 애플리케이션 재시작 불필요
  ✅ 투명한 연결 유지

보안 강화:
  ✅ IAM 인증 지원
  ✅ 자격증명 중앙 관리
  ✅ SSL/TLS 암호화
```

#### **정답 시나리오 (B번)**
```yaml
1. Auto Scaling Group 설정
   - 여러 AZ에 EC2 인스턴스 분산
   - 헬스체크 기반 자동 교체
   - ALB와 연동한 트래픽 분산

2. Aurora Multi-AZ 구성
   - Primary: 쓰기 작업 처리
   - Reader: 여러 AZ에 분산 읽기
   - 자동 장애조치 설정

3. RDS Proxy 배포
   - Aurora 클러스터 앞단 배치
   - 애플리케이션 연결점 단일화
   - 장애조치 시간 최소화

4. 운영 결과
   ✅ AZ 장애 시 자동 복구
   ✅ 데이터 손실 없음 (RPO=0)
   ✅ 최소 다운타임 (RTO<2분)
   ✅ 관리 오버헤드 최소화
   ✅ 비용 효율적 솔루션
```
