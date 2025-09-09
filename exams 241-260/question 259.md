## Question #259
A company is implementing new data retention policies for all databases that run on Amazon RDS DB instances. 
The company must retain daily backups for a minimum period of 2 years. 
The backups must be consistent and restorable.

Which solution should a solutions architect recommend to meet these requirements?

A. Create a backup vault in AWS Backup to retain RDS backups. Create a new backup plan with a daily schedule and an expiration period of 2 years after creation. Assign the RDS DB instances to the backup plan.

B. Configure a backup window for the RDS DB instances for daily snapshots. Assign a snapshot retention policy of 2 years to each RDS DB instance. Use Amazon Data Lifecycle Manager (Amazon DLM) to schedule snapshot deletions.

C. Configure database transaction logs to be automatically backed up to Amazon CloudWatch Logs with an expiration period of 2 years.

D. Configure an AWS Database Migration Service (AWS DMS) replication task. Deploy a replication instance, and configure a change data capture (CDC) task to stream database changes to Amazon S3 as the target. Configure S3 Lifecycle policies to delete the snapshots after 2 years.

## Question #259 분석

### 요구사항
- **RDS DB 인스턴스** 새로운 데이터 보존 정책
- **일일 백업** 최소 2년 보존
- **일관성 있고 복원 가능한** 백업

### 선택지 분석

**A. AWS Backup 볼트 + 백업 계획** ⭐
- **중앙 집중식 관리**: 모든 RDS 인스턴스를 하나의 정책으로 관리 ✅
- **일관성 보장**: AWS Backup이 트랜잭션 일관성 있는 백업 제공 ✅
- **자동화**: 일일 스케줄과 2년 보존 자동 적용 ✅
- **복원 기능**: 완전한 복원 기능 내장 ✅

**B. RDS 스냅샷 + DLM**
- **보존 기간 제한**: RDS 자동 백업은 최대 35일 보존만 지원 ❌
- **수동 스냅샷 필요**: 2년 보존을 위해서는 수동 스냅샷 관리 필요 ❌
- **DLM 호환성**: DLM은 EBS 스냅샷용, RDS 스냅샷 직접 관리 불가 ❌

**C. CloudWatch Logs로 트랜잭션 로그 백업**
- **백업 용도 부적합**: 트랜잭션 로그는 완전한 데이터베이스 백업 아님 ❌
- **복원 불가능**: 로그만으로는 전체 데이터베이스 복원 불가 ❌
- **용도 오해**: CloudWatch Logs는 모니터링용, 백업용 아님 ❌

**D. DMS 복제 + S3 + Lifecycle**
- **용도 불일치**: DMS는 마이그레이션용, 백업용 아님 ❌
- **지속적 복제**: CDC는 지속적 변경 스트리밍, 일일 백업과 다름 ❌
- **복원 복잡성**: S3 데이터로 RDS 복원은 매우 복잡 ❌

### AWS Backup 장점

```yaml
RDS 통합:
- 모든 RDS 엔진 지원
- 자동 백업 스케줄링
- 교차 리전 복사 지원

일관성 보장:
- 트랜잭션 일관성 있는 백업
- 자동 스냅샷 생성
- 백업 무결성 검증

관리 효율성:
- 중앙 집중식 정책 관리
- 자동 보존 기간 적용
- 규정 준수 보고서
```

