## Question #247
A company has deployed a database in Amazon RDS for MySQL. 
Due to increased transactions, the database support team is reporting slow reads against the DB instance and recommends adding a read replica.

Which combination of actions should a solutions architect take before implementing this change? (Choose two.)

A. Enable binlog replication on the RDS primary node.

B. Choose a failover priority for the source DB instance.

C. Allow long-running transactions to complete on the source DB instance.

D. Create a global table and specify the AWS Regions where the table will be available.

E. Enable automatic backups on the source instance by setting the backup retention period to a value other than 0.

## Question #247 분석 (Choose Two)

### 요구사항
- **RDS MySQL** 데이터베이스
- **Read Replica 추가** 전 필요 조건
- **트랜잭션 증가**로 인한 성능 저하

### 선택지 분석

**A. RDS Primary 노드에서 binlog 복제 활성화**
- **RDS 자동 관리**: RDS가 Read Replica 생성 시 자동으로 binlog 활성화 ❌
- **수동 설정 불필요**: RDS에서 직접 binlog 설정할 필요 없음 ❌

**B. 소스 DB 인스턴스 장애조치 우선순위 선택**
- **장애조치와 무관**: Read Replica 생성과 failover priority 별개 ❌
- **Multi-AZ 설정**: Read Replica가 아닌 Multi-AZ 관련 설정 ❌

**C. 소스 DB에서 장기 실행 트랜잭션 완료 대기** ⭐
- **일관성 보장**: 진행 중인 트랜잭션이 Read Replica 생성에 영향 ✅
- **데이터 정합성**: 불완전한 트랜잭션으로 인한 복제 오류 방지 ✅
- **안전한 복제**: 깨끗한 상태에서 스냅샷 생성 ✅

**D. 글로벌 테이블 생성 및 리전 지정**
- **DynamoDB 기능**: RDS와 무관한 DynamoDB 글로벌 테이블 ❌
- **서비스 혼동**: RDS MySQL에는 적용되지 않는 기능 ❌

**E. 자동 백업 활성화 (보존 기간 0이 아닌 값)** ⭐
- **Read Replica 필수 조건**: 자동 백업이 활성화되어야 Read Replica 생성 가능 ✅
- **RDS 요구사항**: 백업 보존 기간이 0이면 Read Replica 생성 불가 ✅
- **기본 전제조건**: Read Replica 생성의 핵심 요구사항 ✅

### Read Replica 생성 조건

```yaml
RDS Read Replica 필수 조건:
1. 자동 백업 활성화 (backup retention > 0)
2. 소스 DB가 Available 상태
3. 장기 실행 트랜잭션 정리
4. 충분한 CPU/메모리 리소스

선택적 조건:
- Multi-AZ 설정 (권장)
- 적절한 보안 그룹 설정
```
