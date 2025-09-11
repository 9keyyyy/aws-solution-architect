## Question #1018
A company needs to give a globally distributed development team secure access to the company's AWS resources in a way that complies with security policies.

The company currently uses an on-premises Active Directory for internal authentication. The company uses AWS Organizations to manage multiple AWS accounts that support multiple projects.

The company needs a solution to integrate with the existing infrastructure to provide centralized identity management and access control.

Which solution will meet these requirements with the LEAST operational overhead?

A. Set up AWS Directory Service to create an AWS managed Microsoft Active Directory on AWS. Establish a trust relationship with the on-premises Active Directory. Use IAM rotes that are assigned to Active Directory groups to access AWS resources within the company's AWS accounts.

B. Create an IAM user for each developer. Manually manage permissions for each IAM user based on each user's involvement with each project. Enforce multi-factor authentication (MFA) as an additional layer of security.

C. Use AD Connector in AWS Directory Service to connect to the on-premises Active Directory. Integrate AD Connector with AWS IAM Identity Center. Configure permissions sets to give each AD group access to specific AWS accounts and resources.

D. Use Amazon Cognito to deploy an identity federation solution. Integrate the identit

## Question #1018 분석

### 문제 상황
- **글로벌 분산 개발팀**의 AWS 리소스 접근 필요
- **온프레미스 Active Directory** 기존 사용 중
- **AWS Organizations**로 다중 계정 관리
- **보안 정책 준수** 필요
- **중앙화된 ID 관리 및 접근 제어** 요구

### 핵심 요구사항
- **기존 인프라와 통합**
- **중앙화된 ID 관리**
- **최소 운영 오버헤드**
- **다중 AWS 계정 접근 제어**
- **보안 정책 준수**

### 선택지 분석

**A. AWS Managed Microsoft AD + Trust Relationship**
```yaml
구성:
- AWS에 새로운 Microsoft AD 생성
- 온프레미스 AD와 신뢰 관계 설정
- IAM 역할을 AD 그룹에 할당

장점:
- 완전 관리형 AD 서비스
- 온프레미스 AD와 통합

단점:
- 이중 AD 관리 (온프레미스 + AWS)
- 추가 관리 복잡성
- 사용자 동기화 필요
- 운영 오버헤드 증가 ❌
```

**B. 개별 IAM 사용자 생성**
```yaml
구성:
- 각 개발자별 IAM 사용자 생성
- 수동 권한 관리
- MFA 강제 적용

문제점:
- 확장성 부족 ❌
- 수동 관리 부담 ❌
- 기존 AD 통합 불가 ❌
- 최대 운영 오버헤드 ❌
- 중앙화된 ID 관리 불가 ❌
```

**C. AD Connector + IAM Identity Center** ⭐
```yaml
구성:
- AD Connector로 온프레미스 AD 연결
- IAM Identity Center와 통합
- Permission Sets으로 계정별 접근 제어

장점:
- 기존 AD 그대로 사용 ✅
- 중앙화된 SSO ✅
- 다중 계정 지원 ✅
- 최소 운영 오버헤드 ✅
- Permission Sets으로 세밀한 제어 ✅
```

**D. Amazon Cognito (선택지 불완전)**
```yaml
용도:
- 주로 웹/모바일 앱 사용자 인증
- B2C 시나리오에 적합

문제점:
- 기업 내부 직원 관리에 부적합 ❌
- 온프레미스 AD 통합 복잡 ❌
- AWS 계정 접근 제어에 최적화 안됨 ❌
```

### AD Connector vs AWS Managed Microsoft AD

| 구분 | AD Connector | AWS Managed Microsoft AD |
|------|--------------|---------------------------|
| **AD 위치** | 온프레미스 AD 사용 | AWS에 새 AD 생성 |
| **관리 부담** | 최소 (기존 AD 활용) | 증가 (이중 관리) |
| **사용자 저장소** | 온프레미스 유지 | AWS + 온프레미스 |
| **운영 오버헤드** | 낮음 | 높음 |
| **적합 시나리오** | 기존 AD 유지하면서 AWS 통합 | 완전한 클라우드 마이그레이션 |
