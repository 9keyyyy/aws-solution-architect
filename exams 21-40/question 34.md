## Question #34
A company hosts its multi-tier applications on AWS. 
For compliance, governance, auditing, and security, the company must track configuration changes on its AWS resources and record a history of API calls made to these resources.

What should a solutions architect do to meet these requirements?

A. Use AWS CloudTrail to track configuration changes and AWS Config to record API calls.

B. Use AWS Config to track configuration changes and AWS CloudTrail to record API calls.

C. Use AWS Config to track configuration changes and Amazon CloudWatch to record API calls.

D. Use AWS CloudTrail to track configuration changes and Amazon CloudWatch to record API calls.


## Question #34 분석

### ✅ 요구사항
- **구성 변경 추적** (Configuration Changes)
- **API 호출 기록** (API Calls History)
- 컴플라이언스, 거버넌스, 감사, 보안 목적

### ✅ 선택지 분석

**A. CloudTrail (구성 변경) + Config (API 호출)**
- **역할이 뒤바뀜**: 각 서비스의 주요 기능과 반대 
- CloudTrail은 API 호출 전용
- Config는 구성 변경 전용

**B. Config (구성 변경) + CloudTrail (API 호출)** ⭐
- **정확한 역할 매칭**: 각 서비스의 전문 분야 ✅
- **AWS Config**: 리소스 구성 변경 추적 전문 ✅
- **AWS CloudTrail**: API 호출 기록 전문 ✅

**C. Config (구성 변경) + CloudWatch (API 호출)**
- **CloudWatch 역할 오해**: CloudWatch는 모니터링 서비스 
- API 호출 기록 기능 없음
- 로그 저장은 하지만 API 감사 전용 아님

**D. CloudTrail (구성 변경) + CloudWatch (API 호출)**
- **둘 다 잘못된 매칭**: 역할 완전 오해 
- CloudWatch는 API 호출 기록 기능 없음

### 📋 AWS 서비스별 전문 분야

### **AWS Config - 구성 변경 추적**
```yaml
전문 기능:
  ✅ AWS 리소스 구성 상태 기록
  ✅ 구성 변경 히스토리 추적
  ✅ 규정 준수 모니터링
  ✅ 구성 규칙 위반 감지

추적하는 것:
  - EC2 인스턴스 속성 변경
  - S3 버킷 정책 변경
  - 보안 그룹 규칙 변경
  - IAM 역할 권한 변경
  - VPC 네트워크 구성 변경
```

### **AWS CloudTrail - API 호출 기록**
```yaml
전문 기능:
  ✅ AWS API 호출 로그 기록
  ✅ 누가, 언제, 어떤 작업 수행했는지 추적
  ✅ 보안 감사 및 포렌식 분석
  ✅ 관리 콘솔, CLI, SDK 모든 호출 기록

기록하는 것:
  - 누가: user123@company.com
  - 언제: 2024-03-15T14:30:00Z
  - 어디서: 192.168.1.100
  - 무엇을: ec2:TerminateInstance
  - 결과: 성공/실패
```


### 🔍 CloudWatch의 역할 (혼동 방지)

### **CloudWatch ≠ 감사 도구**
```yaml
CloudWatch 전문 분야:
  ✅ 메트릭 모니터링 (CPU, 메모리, 디스크)
  ✅ 로그 수집 및 분석
  ✅ 알람 및 알림
  ✅ 대시보드 생성

CloudWatch가 하지 않는 것:
  ❌ API 호출 기록 (CloudTrail 영역)
  ❌ 구성 변경 추적 (Config 영역)
  ❌ 보안 감사 (별도 보안 서비스들)
  ❌ 컴플라이언스 모니터링 (Config 영역)
```

### **로그 vs 감사의 차이**
```yaml
CloudWatch Logs:
  - 애플리케이션 로그 저장
  - 에러 로그, 액세스 로그
  - 성능 분석 목적

CloudTrail (감사):
  - AWS API 호출 감사 추적
  - 보안 및 규정 준수 목적
  - 포렌식 분석 지원
```

## 💡 통합 감사 아키텍처

### **Config + CloudTrail 조합 장점**
```yaml
완전한 감사 추적:
  CloudTrail: "누가 무엇을 했는가?"
  Config: "그 결과 무엇이 어떻게 변했는가?"

상호 보완:
  - CloudTrail: 행위 기록
  - Config: 상태 변경 기록
  - 함께 사용 시 완전한 감사 체계
```
