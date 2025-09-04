## Question #13
A company performs monthly maintenance on its AWS infrastructure. 
During these maintenance activities, the company needs to rotate the credentials for its Amazon RDS for MySQL databases across multiple AWS Regions.

Which solution will meet these requirements with the LEAST operational overhead?

A. Store the credentials as secrets in AWS Secrets Manager. Use multi-Region secret replication for the required Regions. Configure Secrets Manager to rotate the secrets on a schedule.

B. Store the credentials as secrets in AWS Systems Manager by creating a secure string parameter. Use multi-Region secret replication for the required Regions. Configure Systems Manager to rotate the secrets on a schedule.

C. Store the credentials in an Amazon S3 bucket that has server-side encryption (SSE) enabled. Use Amazon EventBridge (Amazon CloudWatch Events) to invoke an AWS Lambda function to rotate the credentials.

D. Encrypt the credentials as secrets by using AWS Key Management Service (AWS KMS) multi-Region customer managed keys. Store the secrets in an Amazon DynamoDB global table. Use an AWS Lambda function to retrieve the secrets from DynamoDB. Use the RDS API to rotate the secrets.


### ✅ 요구사항
- AWS 인프라의 월간 유지보수 중 자격증명 교체
- 여러 AWS 리전의 Amazon RDS MySQL 데이터베이스
- **최소한의 운영 오버헤드** 필요

### ✅ 선택지 분석

**A. AWS Secrets Manager + 다중 리전 복제 + 스케줄 교체** ⭐
- Secrets Manager의 **네이티브 RDS 통합**
- 자동 스케줄링 지원
- 다중 리전 복제 기능 내장
- **Zero-downtime rotation** 지원
- 최소 운영 오버헤드

**B. Systems Manager Parameter Store + 다중 리전 복제**
- Parameter Store는 **자동 rotation 미지원**
- RDS 네이티브 통합 없음
- 수동 Lambda 함수 구현 필요
- 운영 오버헤드 증가

**C. S3 + EventBridge + Lambda**
- 완전 수동 구현 필요
- 다중 리전 동기화 복잡
- RDS 통합 없음
- 높은 운영 오버헤드

**D. KMS + DynamoDB Global Table + Lambda**
- 과도하게 복잡한 아키텍처
- 완전 수동 구현
- 가장 높은 운영 오버헤드

### 📋 서비스별 기능 비교

### **AWS Secrets Manager**
```yaml
RDS 통합 기능:
  - 네이티브 MySQL/PostgreSQL 지원
  - 자동 사용자 생성/삭제
  - Zero-downtime rotation
  - 자동 연결 문자열 업데이트

다중 리전 지원:
  - Primary-Replica 복제
  - Cross-region 자동 동기화
  - 리전별 독립적 접근

스케줄링:
  - Cron 표현식 지원
  - 자동 rotation 실행
  - 장애 재시도 내장
```

### **Systems Manager Parameter Store**
```yaml
제한사항:
  - 자동 rotation 미지원 ❌
  - RDS 네이티브 통합 없음 ❌
  - 수동 Lambda 구현 필요 ❌

장점:
  - 저렴한 비용
  - 간단한 key-value 저장
  - 다중 리전 복제 지원
```

### 🔄 Secrets Manager Rotation 프로세스

### **RDS MySQL Rotation 시나리오**
```
1단계: 새 사용자 생성
   Current: app_user_A (활성)
   New:     app_user_B (생성 중)

2단계: 권한 복사
   app_user_B ← app_user_A 권한 복사
   
3단계: 테스트 연결
   app_user_B로 RDS 연결 테스트
   
4단계: 자격증명 전환
   Secret → app_user_B 정보로 업데이트
   
5단계: 이전 사용자 정리
   app_user_A 삭제 (다음 rotation 시)
```

### **Multi-Region 동기화**
```yaml
Primary Region (us-east-1):
  Secret: rds-mysql-creds
  Status: Rotation 완료
  
Replica Regions:
  us-west-2: ← 자동 복제 (30초 내)
  eu-west-1: ← 자동 복제 (30초 내)
  ap-south-1: ← 자동 복제 (30초 내)
```
