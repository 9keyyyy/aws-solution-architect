## Question #997
A company is launching a new application that requires a structured database to store user profiles, application settings, and transactional data. The database must be scalable with application traffic and must offer backups.

Which solution will meet these requirements MOST cost-effectively?

A. Deploy a self-managed database on Amazon EC2 instances by using open source software. Use Spot Instances for cost optimization. Configure automated backups to Amazon S3.

B. Use Amazon RDS. Use on-demand capacity mode for the database with General Purpose SSD storage. Configure automatic backups with a retention period of 7 days.

C. Use Amazon Aurora Serverless for the database. Use serverless capacity scaling. Configure automated backups to Amazon S3.

D. Deploy a self-managed NoSQL database on Amazon EC2 instances. Use Reserved Instances for cost optimization. Configure automated backups directly to Amazon S3 Glacier Flexible Retrieval.

## Question #997 분석

### 문제 상황
- **새로운 애플리케이션** 출시
- **구조화된 데이터베이스** 필요 (사용자 프로필, 설정, 트랜잭션)
- **애플리케이션 트래픽에 따른 확장** 필요
- **백업** 기능 필요

### 핵심 요구사항
- **구조화된 데이터베이스**
- **확장성**
- **백업**
- **비용 효율성**

### 선택지 분석

**A. Self-managed DB + Spot Instances**
- 관리 부담 증가 ❌
- Spot Instance: 중단 위험 (DB에 부적합) ❌
- 운영 복잡성 ❌

**B. Amazon RDS + On-demand + GP2** ⭐
- 완전 관리형 ✅
- 구조화된 데이터베이스 ✅
- 자동 백업 ✅
- 관리 부담 최소 ✅
- 예측 가능한 비용 ✅

**C. Aurora Serverless**
- 서버리스 = 더 높은 비용 ❌
- 새 애플리케이션 = 트래픽 예측 어려움 → 서버리스 비용 부담 ❌

**D. Self-managed NoSQL + Reserved Instances**
- "구조화된 데이터베이스" 요구사항 ≠ NoSQL ❌
- 관리 부담 증가 ❌

### 비용 효율성 비교

**새 애플리케이션의 특성**
```yaml
초기 단계:
- 트래픽 예측 어려움
- 확장 필요성 불분명
- 운영 리소스 제한적

적합한 접근:
- 관리형 서비스 (운영 부담 최소)
- 온디맨드 (유연성)
- 표준 스토리지 (균형)
```

### RDS vs Aurora Serverless

**RDS (Option B)**
```yaml
장점:
- 낮은 기본 비용
- 예측 가능한 요금
- 표준 기능 충분

단점:
- 수동 스케일링
```

**Aurora Serverless (Option C)**
```yaml
장점:
- 자동 스케일링
- 사용량 기반 과금

단점:
- 높은 최소 비용
- 새 앱에는 과도한 기능
```
