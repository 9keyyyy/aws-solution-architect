## Question #71
A company runs a shopping application that uses Amazon DynamoDB to store customer information. 
In case of data corruption, a solutions architect needs to design a solution that meets a recovery point objective (RPO) of 15 minutes and a recovery time objective (RTO) of 1 hour.

What should the solutions architect recommend to meet these requirements?

A. Configure DynamoDB global tables. For RPO recovery, point the application to a different AWS Region.

B. Configure DynamoDB point-in-time recovery. For RPO recovery, restore to the desired point in time.

C. Export the DynamoDB data to Amazon S3 Glacier on a daily basis. For RPO recovery, import the data from S3 Glacier to DynamoDB.

D. Schedule Amazon Elastic Block Store (Amazon EBS) snapshots for the DynamoDB table every 15 minutes. For RPO recovery, restore the DynamoDB table by using the EBS snapshot.

## Question #71 분석

### ✅ 요구사항
- **DynamoDB 고객 정보** 저장
- **데이터 손상 시 복구** 솔루션 필요
- **RPO 15분**: 최대 15분 데이터 손실 허용
- **RTO 1시간**: 1시간 내 서비스 복구 필요

### ✅ 선택지 분석

**A. DynamoDB Global Tables + 리전 전환**
- **Global Tables**: 멀티 리전 자동 복제 ✅
- **RPO 성능**: 초 단위 복제로 15분 요구사항 충족 ✅
- **RTO 문제**: 리전 전환은 데이터 손상 복구 아님 ❌
- **목적 불일치**: 데이터 손상 시 모든 리전에 손상 전파 ❌
- **잘못된 접근**: 재해복구용이지 데이터 손상 복구용 아님 ❌

**B. DynamoDB Point-in-Time Recovery (PITR)** ⭐
- **PITR 기능**: 35일간 지속적 백업 ✅
- **RPO 달성**: 초 단위 복구로 15분 요구사항 충족 ✅
- **RTO 달성**: 복원 시간 보통 30-60분으로 1시간 내 ✅
- **데이터 손상 대응**: 손상 발생 이전 시점으로 정확 복구 ✅
- **완전 관리형**: 추가 설정 없이 자동 백업 ✅

**C. S3 Glacier 일일 백업**
- **일일 백업**: 24시간 RPO로 15분 요구사항 미충족 ❌
- **복원 시간**: Glacier 복원에 시간 소요 ❌
- **RTO 초과**: 복원 + 임포트로 1시간 초과 가능성 ❌
- **수동 프로세스**: 자동화 복잡, 운영 부담 ❌

**D. EBS 스냅샷 15분마다**
- **근본적 오류**: DynamoDB는 EBS 사용하지 않음 ❌
- **기술적 불가능**: DynamoDB는 관리형 서비스 ❌
- **잘못된 이해**: DynamoDB 아키텍처 오해 ❌
- **실행 불가**: 물리적으로 불가능한 방법 ❌

### 📋 핵심 개념 정리

#### **DynamoDB Point-in-Time Recovery**
```yaml
PITR 특징:
  ✅ 지속적 백업 (Continuous Backup)
  ✅ 35일간 보존
  ✅ 초 단위 복구 지점 선택
  ✅ 자동 백업 (설정 후)
  ✅ 추가 스토리지 비용만

복구 프로세스:
  1. 복구 시점 선택 (최대 35일 전)
  2. 새 테이블로 복원
  3. 애플리케이션 엔드포인트 변경
  4. 기존 손상된 테이블 삭제

RPO/RTO 성능:
  ✅ RPO: 초 단위 (15분 요구사항 충족)
  ✅ RTO: 30-60분 (1시간 요구사항 충족)
```

#### **백업/복구 옵션 비교**
```yaml
Point-in-Time Recovery:
  - RPO: 초 단위
  - RTO: 30-60분
  - 용도: 데이터 손상 복구
  - 비용: 낮음 (스토리지만)

On-Demand Backup:
  - RPO: 백업 시점까지
  - RTO: 복원 시간
  - 용도: 특정 시점 백업
  - 비용: 보통

Global Tables:
  - RPO: 초 단위
  - RTO: 거의 즉시
  - 용도: 재해복구, 고가용성
  - 비용: 높음 (복제 비용)

S3 Export:
  - RPO: 백업 간격
  - RTO: 긴 복원 시간
  - 용도: 데이터 아카이빙
  - 비용: 낮음
```

#### **RPO/RTO 요구사항 매핑**
```yaml
요구사항 분석:
  RPO 15분: 최대 15분 데이터 손실 허용
  RTO 1시간: 1시간 내 서비스 복구

PITR 성능:
  ✅ RPO: 1초 (15분 << 1초)
  ✅ RTO: 30-60분 (≤ 1시간)
  ✅ 요구사항 완전 충족

다른 옵션들:
  Global Tables: RPO ✅, RTO ✅, 목적 ❌
  S3 Glacier: RPO ❌, RTO ❌
  EBS Snapshot: 기술적 불가능 ❌
```

#### **데이터 손상 vs 재해복구**
```yaml
데이터 손상 시나리오:
  - 애플리케이션 버그로 잘못된 데이터 쓰기
  - 사용자 실수로 중요 데이터 삭제
  - 소프트웨어 오류로 데이터 변조

복구 요구사항:
  ✅ 손상 이전 시점으로 정확한 복구
  ✅ 신속한 복구 시간
  ✅ 최소한의 데이터 손실

PITR이 최적인 이유:
  ✅ 원하는 시점 정확 선택
  ✅ 손상되지 않은 데이터로 복원
  ✅ 자동화된 프로세스
  ✅ 추가 인프라 불필요
```

#### **정답 시나리오 (B번)**
```yaml
1. PITR 활성화
   - DynamoDB 콘솔에서 활성화
   - 35일간 지속적 백업 시작
   - 추가 설정 없이 자동 운영

2. 데이터 손상 발생
   - 애플리케이션 오류로 데이터 손상
   - 손상 시점 확인 (예: 오전 10:30)

3. 복구 프로세스
   - 손상 이전 시점 선택 (오전 10:25)
   - 새 테이블로 복원 시작
   - 복원 완료 (30-45분 소요)

4. 서비스 복구
   - 애플리케이션을 새 테이블로 연결
   - 손상된 기존 테이블 삭제
   - 정상 서비스 재개

결과:
✅ RPO: 5분 (15분 이내)
✅ RTO: 45분 (1시간 이내)
✅ 완전한 데이터 복구
✅ 최소 운영 부담
```
