## Question #78
A company needs to keep user transaction data in an Amazon DynamoDB table. 
The company must retain the data for 7 years.

What is the MOST operationally efficient solution that meets these requirements?

A. Use DynamoDB point-in-time recovery to back up the table continuously.

B. Use AWS Backup to create backup schedules and retention policies for the table.

C. Create an on-demand backup of the table by using the DynamoDB console. Store the backup in an Amazon S3 bucket. Set an S3 Lifecycle configuration for the S3 bucket.

D. Create an Amazon EventBridge (Amazon CloudWatch Events) rule to invoke an AWS Lambda function. Configure the Lambda function to back up the table and to store the backup in an Amazon S3 bucket. Set an S3 Lifecycle configuration for the S3 bucket.

## Question #78 분석

### ✅ 요구사항
- **DynamoDB 사용자 트랜잭션 데이터** 저장
- **7년간 데이터 보존** 필수
- **최고 운영 효율성** 필요

### ✅ 핵심 고려사항
```yaml
장기 보존 요구사항:
- 7년 = 2,555일
- 규정 준수 목적 가능성
- 비용 효율성 중요
- 자동화된 관리 필요

운영 효율성:
✅ 자동화된 백업
✅ 최소 수동 개입
✅ 안정적인 장기 보존
✅ 비용 최적화
```

### ✅ 선택지 분석

**A. DynamoDB Point-in-Time Recovery (PITR)**
- **보존 기간**: 최대 35일만 지원 ❌
- **7년 요구사항**: 완전히 미충족 ❌
- **용도**: 단기 데이터 복구용 서비스 ❌
- **장기 보존**: 설계 목적과 부합하지 않음 ❌

**B. AWS Backup으로 스케줄 및 정책 관리** ⭐
- **DynamoDB 지원**: AWS Backup 완전 지원 ✅
- **자동 스케줄링**: 정기적 백업 자동화 ✅
- **보존 정책**: 7년 설정 가능 ✅
- **완전 관리형**: 운영 부담 최소 ✅
- **규정 준수**: 감사 추적 및 암호화 ✅
- **비용 최적화**: 자동 스토리지 클래스 전환 ✅

**C. 온디맨드 백업 + S3 + 수명주기 정책**
- **수동 프로세스**: 온디맨드는 수동 실행 필요 ❌
- **자동화 부족**: 정기적 백업 스케줄링 없음 ❌
- **운영 부담**: 수동 백업 관리 필요 ❌
- **확장성 제한**: 대규모 환경에서 비효율적 ❌

**D. EventBridge + Lambda + S3 백업**
- **복잡한 구성**: 여러 서비스 통합 필요 ❌
- **커스텀 개발**: Lambda 함수 개발 및 유지보수 ❌
- **운영 복잡성**: 에러 처리, 모니터링 등 직접 구현 ❌
- **AWS Backup 대비**: 불필요한 복잡성 ❌

### 📋 핵심 개념 정리

#### **DynamoDB 백업 옵션 비교**
```yaml
Point-in-Time Recovery (PITR):
- 용도: 단기 복구 (최대 35일)
- 특징: 지속적 백업, 초 단위 복구
- 한계: 장기 보존 불가
- 비용: 테이블 크기 기반

On-Demand Backup:
- 용도: 특정 시점 백업
- 특징: 수동 트리거, 영구 보존
- 한계: 자동화 어려움
- 비용: 백업 크기 기반

AWS Backup:
- 용도: 중앙 집중식 백업 관리
- 특징: 스케줄링, 정책, 자동화
- 장점: 완전 관리형
- 비용: 백업 크기 + 관리 기능
```

#### **AWS Backup 장점**
```yaml
중앙 집중 관리:
✅ 다중 서비스 백업 통합
✅ 정책 기반 관리
✅ 태그 기반 자동 백업
✅ 크로스 리전 복사

자동화 기능:
✅ 스케줄 기반 백업
✅ 보존 정책 자동 적용
✅ 스토리지 클래스 전환
✅ 만료된 백업 자동 삭제

규정 준수:
✅ 감사 로그 제공
✅ 암호화 지원
✅ 액세스 제어
✅ 백업 상태 모니터링
```


#### **정답 시나리오 (B번)**
```yaml
1. AWS Backup 설정
   - DynamoDB 테이블 선택
   - 백업 vault 생성
   - KMS 키 설정 (암호화)

2. 백업 계획 생성
   Schedule:
   - 빈도: 일일 (또는 주간)
   - 시작 시간: 저사용량 시간대
   - 백업 윈도우: 8시간

3. 보존 정책 설정
   Lifecycle:
   - 즉시: Standard
   - 30일 후: Standard IA
   - 90일 후: Glacier
   - 1년 후: Deep Archive
   - 7년 후: 삭제

4. 모니터링 설정
   - CloudWatch 알람
   - 백업 성공/실패 알림
   - 보존 정책 추적
   - 비용 모니터링

운영 결과:
✅ 완전 자동화된 7년 보존
✅ 최소 운영 개입
✅ 비용 최적화
✅ 규정 준수 지원
✅ 중앙 집중식 관리
```


#### **운영 복잡성 비교**
```yaml
B안 (AWS Backup):
설정: 한 번 설정 후 자동 운영
관리: 정책 기반 자동 관리
모니터링: 내장 대시보드
문제 해결: AWS 지원

C안 (수동):
설정: 매번 수동 백업 실행
관리: 수동 스케줄 관리
모니터링: 별도 설정 필요
문제 해결: 직접 해결

D안 (커스텀):
설정: 복잡한 다중 서비스 구성
관리: Lambda 함수 유지보수
모니터링: 커스텀 구현 필요
문제 해결: 모든 컴포넌트 직접 관리
```
