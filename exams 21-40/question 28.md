## Question #28

A company is migrating applications to AWS. 
The applications are deployed in different accounts. 
The company manages the accounts centrally by using AWS Organizations. 
The company's security team needs a single sign-on (SSO) solution across all the company's accounts. 
The company must continue managing the users and groups in its on-premises self-managed Microsoft Active Directory.

Which solution will meet these requirements?

A. Enable AWS Single Sign-On (AWS SSO) from the AWS SSO console. Create a one-way forest trust or a one-way domain trust to connect the company's self-managed Microsoft Active Directory with AWS SSO by using AWS Directory Service for Microsoft Active Directory.

B. Enable AWS Single Sign-On (AWS SSO) from the AWS SSO console. Create a two-way forest trust to connect the company's self-managed Microsoft Active Directory with AWS SSO by using AWS Directory Service for Microsoft Active Directory.

C. Use AWS Directory Service. Create a two-way trust relationship with the company's self-managed Microsoft Active Directory.

D. Deploy an identity provider (IdP) on premises. Enable AWS Single Sign-On (AWS SSO) from the AWS SSO console.

## Question #28 분석

### ✅ 요구사항
- **여러 AWS 계정** 간 SSO 솔루션
- AWS Organizations로 중앙 관리
- **온프레미스 AD**에서 계속 사용자/그룹 관리
- **기존 AD 유지** (마이그레이션하지 않음)

### ✅ 선택지 분석

**A. AWS SSO + 단방향 신뢰** ⭐
- **AWS SSO**: 다중 계정 SSO 완벽 지원 ✅
- **단방향 신뢰**: 온프레미스 AD → AWS 방향만 ✅
- **AD DS + 신뢰 관계**: 표준 하이브리드 솔루션 ✅
- **사용자 관리**: 온프레미스 AD에서 유지 ✅

**B. AWS SSO + 양방향 신뢰**
- **양방향 신뢰**: 불필요한 복잡성 
- AWS → 온프레미스 신뢰 불필요
- 보안상 과도한 신뢰 관계

**C. AWS Directory Service만 사용**
- **AWS SSO 없음**: 다중 계정 SSO 불가 
- Organizations 통합 부족
- 계정별 개별 설정 필요

**D. 온프레미스 IdP + AWS SSO**
- **별도 IdP 배포**: 불필요한 추가 인프라 
- 기존 AD 활용하지 않음
- 운영 복잡성 증가

### 📋 AWS SSO + AD 통합 아키텍처

### **하이브리드 신뢰 관계**
```yaml
온프레미스 환경:
  - 자체 관리 Microsoft AD
  - 기존 사용자/그룹 유지
  - 계속 중앙 사용자 관리

AWS 환경:
  - AWS Managed Microsoft AD (Directory Service)
  - 온프레미스 AD와 단방향 신뢰
  - AWS SSO와 통합
```

### **인증 흐름**
```yaml
1. 사용자가 AWS SSO 로그인 페이지 접속
2. 온프레미스 AD 자격증명으로 로그인
3. AWS Managed AD가 신뢰 관계를 통해 인증 확인
4. AWS SSO가 SAML 어설션 생성
5. 사용자가 원하는 AWS 계정/역할로 접근
```

### 🔄 신뢰 관계 방향성 이해

### **단방향 신뢰 (A번 - 정답)**
```yaml
신뢰 방향: 온프레미스 AD → AWS Managed AD

의미:
  - AWS 사용자가 온프레미스 AD 사용자를 신뢰
  - 온프레미스 AD 사용자가 AWS 리소스 접근 가능
  - AWS에서 온프레미스로의 역방향 접근 불가

장점:
  ✅ 보안상 최소 권한 원칙
  ✅ 필요한 방향만 신뢰
  ✅ 공격 표면 최소화
```

### **양방향 신뢰 (B번)**
```yaml
신뢰 방향: 온프레미스 AD ↔ AWS Managed AD

문제점:
  ❌ 불필요한 역방향 신뢰
  ❌ AWS 사용자가 온프레미스 리소스 접근 가능
  ❌ 보안 위험 증가
  ❌ 복잡성 증가

언제 필요한가:
  - AWS와 온프레미스 간 양방향 접근 필요시
```

### 📊 다른 옵션들의 한계

### **Directory Service만 사용 (C번)**
```yaml
제공하는 것:
  - AWS Managed Microsoft AD
  - 온프레미스와 신뢰 관계

제공하지 않는 것:
  ❌ 다중 AWS 계정 SSO
  ❌ Organizations 통합
  ❌ 중앙집중식 액세스 관리
  ❌ SAML 기반 연합 인증
```

### **별도 IdP 배포 (D번)**
```yaml
문제점:
  ❌ 기존 AD 인프라 활용하지 않음
  ❌ 추가 서버 및 소프트웨어 필요
  ❌ 중복된 사용자 관리
  ❌ 높은 운영 오버헤드

예시:
  - ADFS 서버 별도 구축
  - Okta, Ping Identity 등 서드파티 솔루션
```

### 💡 AWS Organizations 통합

### **중앙집중식 SSO 관리**
```yaml
AWS SSO + Organizations:
  - 마스터 계정에서 SSO 활성화
  - 모든 멤버 계정 자동 통합
  - 중앙에서 권한 집합(Permission Sets) 관리
  - 사용자별 계정/역할 할당

권한 집합 예시:
  - DevOps-FullAccess (개발 계정)
  - ReadOnly-Access (모든 계정)
  - BillingAdmin (결제 계정)
```

### **사용자 관리 워크플로우**
```yaml
온프레미스 AD 관리자:
  1. 새 직원을 AD에 추가
  2. 적절한 AD 그룹에 할당

AWS SSO 관리자:
  1. AD 그룹을 AWS SSO 그룹과 매핑
  2. 권한 집합을 그룹에 할당
  3. 계정별 접근 권한 구성

자동 동기화:
  - AD 변경사항 자동 반영
  - 퇴사자 자동 접근 차단
```
