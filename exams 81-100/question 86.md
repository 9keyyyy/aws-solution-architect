## Question #86
A company has several web servers that need to frequently access a common Amazon RDS MySQL Multi-AZ DB instance. 
The company wants a secure method for the web servers to connect to the database while meeting a security requirement to rotate user credentials frequently.

Which solution meets these requirements?

A. Store the database user credentials in AWS Secrets Manager. Grant the necessary IAM permissions to allow the web servers to access AWS Secrets Manager.

B. Store the database user credentials in AWS Systems Manager OpsCenter. Grant the necessary IAM permissions to allow the web servers to access OpsCenter.

C. Store the database user credentials in a secure Amazon S3 bucket. Grant the necessary IAM permissions to allow the web servers to retrieve credentials and access the database.

D. Store the database user credentials in files encrypted with AWS Key Management Service (AWS KMS) on the web server file system. The web server should be able to decrypt the files and access the database.

## Question #86 분석

### ✅ 요구사항
- **웹서버 → RDS MySQL Multi-AZ** 연결
- **보안 연결** 방식 필요
- **자격증명 자동 로테이션** 요구사항
- **빈번한 자격증명 교체** 지원

### ✅ 핵심 고려사항
```yaml
자격증명 관리:
✅ 안전한 저장
✅ 자동 로테이션
✅ 중앙 집중 관리
✅ 감사 추적

보안 요구사항:
✅ 암호화된 저장
✅ 접근 제어 (IAM)
✅ 전송 중 암호화
✅ 최소 권한 원칙
```

### ✅ 선택지 분석

**A. AWS Secrets Manager + IAM 권한** ⭐
- **자격증명 전용 서비스**: Secrets Manager는 자격증명 관리 전용 ✅
- **자동 로테이션**: RDS 네이티브 로테이션 지원 ✅
- **중앙 집중 관리**: 모든 자격증명을 한 곳에서 관리 ✅
- **보안**: 전송/저장 시 암호화, IAM 통합 ✅
- **감사**: CloudTrail 통합, 접근 로그 자동 기록 ✅

**B. Systems Manager OpsCenter + IAM**
- **용도 불일치**: OpsCenter는 운영 이슈 관리용 ❌
- **자격증명 저장소 아님**: 자격증명 관리 목적 아님 ❌
- **로테이션 미지원**: 자동 로테이션 기능 없음 ❌

**C. S3 버킷 + IAM 권한**
- **수동 관리**: 자격증명 로테이션 수동 구현 필요 ❌
- **용도 불일치**: S3는 파일 저장소, 자격증명 관리용 아님 ❌
- **복잡성**: 커스텀 로테이션 로직 개발 필요 ❌
- **보안 위험**: 잘못된 접근 권한 설정 위험 ❌

**D. KMS 암호화 파일 + 로컬 저장**
- **분산 관리**: 각 서버별 개별 관리 필요 ❌
- **로테이션 복잡**: 모든 서버 파일 동기화 필요 ❌
- **확장성 부족**: 웹서버 증가 시 관리 복잡도 증가 ❌
- **단일 장애점**: 서버별 파일 손상 위험 ❌

### 📋 Secrets Manager 장점

**자동 로테이션**
```yaml
RDS 통합:
✅ RDS MySQL 네이티브 지원
✅ Lambda 기반 자동 로테이션
✅ 다운타임 없는 로테이션
✅ 스케줄 기반 자동 실행

로테이션 프로세스:
1. 새 자격증명 생성
2. 데이터베이스에 새 사용자 생성
3. 애플리케이션 테스트
4. 기존 자격증명 비활성화
5. Secrets Manager 업데이트
```

**보안 기능**
```yaml
암호화:
✅ 저장 시: KMS 키로 암호화
✅ 전송 시: TLS 1.2 이상
✅ 키 관리: AWS KMS 통합

접근 제어:
✅ IAM 정책 기반
✅ 리소스 기반 정책
✅ VPC 엔드포인트 지원
✅ 최소 권한 원칙
```

### 💡 구현 예시

**Secrets Manager 설정**
```yaml
1. Secret 생성
- Secret Type: RDS database credentials
- Database: MySQL Multi-AZ instance
- Username/Password: 초기 자격증명

2. 자동 로테이션 활성화
- Rotation schedule: 30일마다
- Lambda function: AWS 제공 템플릿
- Immediate rotation: 테스트 목적

3. IAM 정책 생성
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Action": [
      "secretsmanager:GetSecretValue"
    ],
    "Resource": "arn:aws:secretsmanager:region:account:secret:db-credentials-*"
  }]
}
```



### 📊 다른 옵션들의 한계

**B안 (OpsCenter) 문제점:**
```yaml
OpsCenter 목적:
- 운영 이슈 중앙 집중 관리
- 인시던트 추적
- 자동화 실행 상태 모니터링

자격증명 관리 부적합:
❌ 자격증명 저장 설계 아님
❌ 로테이션 기능 없음
❌ 보안 특화 기능 부족
```

**C안 (S3) 문제점:**
```yaml
수동 구현 필요:
❌ 로테이션 로직 개발
❌ 애플리케이션 동기화
❌ 에러 처리 및 복구
❌ 감사 로그 구현

운영 복잡성:
❌ 버전 관리
❌ 동시성 제어
❌ 롤백 메커니즘
❌ 모니터링 구현
```

**D안 (로컬 파일) 문제점:**
```yaml
확장성 문제:
❌ 서버 추가 시 수동 배포
❌ 로테이션 시 모든 서버 업데이트
❌ 동기화 실패 위험

보안 위험:
❌ 파일 시스템 접근 위험
❌ 백업 시 평문 노출
❌ 서버 침해 시 자격증명 노출
```

