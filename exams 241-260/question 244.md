## Question #244
A company is using a content management system that runs on a single Amazon EC2 instance. 
The EC2 instance contains both the web server and the database software. 
The company must make its website platform highly available and must enable the website to scale to meet user demand.

What should a solutions architect recommend to meet these requirements?

A. Move the database to Amazon RDS, and enable automatic backups. Manually launch another EC2 instance in the same Availability Zone. Configure an Application Load Balancer in the Availability Zone, and set the two instances as targets.

B. Migrate the database to an Amazon Aurora instance with a read replica in the same Availability Zone as the existing EC2 instance. Manually launch another EC2 instance in the same Availability Zone. Configure an Application Load Balancer, and set the two EC2 instances as targets.

C. Move the database to Amazon Aurora with a read replica in another Availability Zone. Create an Amazon Machine Image (AMI) from the EC2 instance. Configure an Application Load Balancer in two Availability Zones. Attach an Auto Scaling group that uses the AMI across two Availability Zones.

D. Move the database to a separate EC2 instance, and schedule backups to Amazon S3. Create an Amazon Machine Image (AMI) from the original EC2 instance. Configure an Application Load Balancer in two Availability Zones. Attach an Auto Scaling group that uses the AMI across two Availability Zones.

## Question #244 분석

### 요구사항
- **단일 EC2** (웹서버 + 데이터베이스)
- **고가용성** 달성
- **확장성** 지원

### 선택지 분석

**A. RDS + 단일 AZ 수동 EC2**
- **단일 AZ**: 가용 영역 장애 시 전체 서비스 중단 ❌
- **수동 확장**: Auto Scaling 없어 확장성 부족 ❌
- **고가용성 미달**: AZ 장애 내성 없음 ❌

**B. Aurora + 단일 AZ 수동 EC2**
- **동일한 문제**: 단일 AZ로 고가용성 미달 ❌
- **Read Replica 동일 AZ**: 가용성 개선 효과 없음 ❌
- **확장성 부족**: 수동 인스턴스 관리 ❌

**C. Aurora + 멀티 AZ + AMI + Auto Scaling** ⭐
- **고가용성**: 멀티 AZ 배치로 가용 영역 장애 내성 ✅
- **자동 확장**: Auto Scaling으로 수요에 따른 자동 확장 ✅
- **관리형 DB**: Aurora로 데이터베이스 관리 부담 감소 ✅
- **완전한 해결**: 모든 요구사항 충족 ✅

**D. EC2 DB + 멀티 AZ + Auto Scaling**
- **DB 관리 부담**: 여전히 EC2에서 DB 직접 관리 ❌
- **확장성 부족**: DB가 분리되어 있어도 EC2 기반 관리 복잡 ❌
- **백업 복잡성**: S3 백업을 수동 관리 ❌

### C안 아키텍처

```yaml
고가용성:
- 웹서버: 멀티 AZ Auto Scaling Group
- 데이터베이스: Aurora + 다른 AZ Read Replica
- 로드밸런서: 멀티 AZ ALB

확장성:
- Auto Scaling: CPU/메모리 기반 자동 확장
- Aurora: 읽기 성능 자동 확장
- AMI: 일관된 인스턴스 배포
```
