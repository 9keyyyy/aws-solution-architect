## Question #989
A company runs database workloads on AWS that are the backend for the company's customer portals. The company runs a Multi-AZ database cluster on Amazon RDS for PostgreSQL.

The company needs to implement a 30-day backup retention policy. The company currently has both automated RDS backups and manual RDS backups. The company wants to maintain both types of existing RDS backups that are less than 30 days old.

Which solution will meet these requirements MOST cost-effectively?

A. Configure the RDS backup retention policy to 30 days for automated backups by using AWS Backup. Manually delete manual backups that are older than 30 days.

B. Disable RDS automated backups. Delete automated backups and manual backups that are older than 30 days. Configure the RDS backup retention policy to 30 days for automated backups.

C. Configure the RDS backup retention policy to 30 days for automated backups. Manually delete manual backups that are older than 30 days.

D. Disable RDS automated backups. Delete automated backups and manual backups that are older than 30 days automatically by using AWS CloudFormation. Configure the RDS backup retention policy to 30 days for automated backups.


## Question #989 분석

### 문제 상황
- **RDS PostgreSQL Multi-AZ** 클러스터
- **30일 백업 보존 정책** 구현 필요
- 현재 **자동 백업 + 수동 백업** 모두 존재
- **30일 미만 백업 유지** 원함

### 핵심 요구사항
- **30일 보존 정책**
- **기존 백업 유지** (30일 미만)
- **비용 효율성**

### RDS 백업 메커니즘

**자동 백업**: RDS 보존 정책으로 관리 (1-35일)
**수동 백업**: 사용자가 직접 삭제해야 함

### 선택지 분석

**A. AWS Backup 사용 + 수동 삭제**
- AWS Backup: 추가 비용 발생 ❌
- RDS 자동 백업만으로 충분한데 불필요한 오버헤드 ❌

**B. 자동 백업 비활성화 후 재설정**
- 기존 백업 삭제 → 요구사항 위배 ❌
- 불필요한 작업 ❌

**C. RDS 보존 정책 30일 + 수동 백업 수동 삭제** ⭐
- 자동 백업: 정책으로 자동 관리 ✅
- 수동 백업: 필요한 것만 수동 삭제 ✅
- 기존 백업 유지 ✅
- 가장 간단하고 비용 효율적 ✅

**D. CloudFormation으로 자동 삭제**
- 과도한 복잡성 ❌
- CloudFormation은 백업 삭제 자동화에 부적합 ❌

### 백업 관리 모범 사례

```yaml
자동 백업:
- RDS 보존 정책으로 관리
- 1-35일 설정 가능
- 자동 삭제/유지

수동 백업:
- 사용자가 직접 관리
- 보존 기간 제한 없음
- 수동 삭제 필요
```

### 비용 효율성

**Option C**가 가장 비용 효율적인 이유:
- 추가 서비스(AWS Backup) 불필요
- 기본 RDS 기능만 사용
- 불필요한 백업 재생성 없음
