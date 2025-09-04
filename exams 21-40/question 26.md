## Question #26
A company needs to review its AWS Cloud deployment to ensure that its Amazon S3 buckets do not have unauthorized configuration changes.

What should a solutions architect do to accomplish this goal?

A. Turn on AWS Config with the appropriate rules.

B. Turn on AWS Trusted Advisor with the appropriate checks.

C. Turn on Amazon Inspector with the appropriate assessment template.

D. Turn on Amazon S3 server access logging. Configure Amazon EventBridge (Amazon Cloud Watch Events).


## Question #26 분석

### ✅ 요구사항
- S3 버킷의 **무단 구성 변경** 모니터링
- **구성 변경 검토** 및 탐지
- 컴플라이언스 확인

### ✅ 선택지 분석

**A. AWS Config + 적절한 규칙** ⭐
- **구성 변경 추적**: Config의 핵심 기능 ✅
- **S3 버킷 모니터링**: 네이티브 지원 ✅
- **규칙 기반 평가**: 무단 변경 자동 탐지 ✅
- **히스토리 추적**: 변경 내역 완전 기록 ✅

**B. AWS Trusted Advisor**
- **모범 사례 권장**: 일회성 체크 도구 
- **실시간 모니터링 없음**: 변경 추적 불가 
- 구성 변경 탐지 기능 부재

**C. Amazon Inspector**
- **보안 취약점 스캔**: EC2/컨테이너 전용 
- **S3 구성 모니터링 불가**: 서비스 범위 외 
- 애플리케이션 보안 평가 도구

**D. S3 Access Logging + EventBridge**
- **액세스 로깅**: 데이터 접근 기록만 
- **구성 변경 추적 불가**: API 호출 로그 없음 
- CloudTrail이 필요한 접근법

### 📋 AWS Config 핵심 기능

### **구성 변경 추적**
```yaml
모니터링 대상:
  - S3 버킷 정책 변경
  - 퍼블릭 액세스 설정 변경
  - 암호화 설정 변경
  - 버전 관리 설정 변경
  - 로깅 구성 변경

자동 탐지:
  - 실시간 구성 변경 감지
  - 규칙 위반 시 즉시 알림
  - SNS 통합으로 알림 전송
```

### **S3 관련 Config 규칙 예시**
```yaml
기본 제공 규칙:
  - s3-bucket-public-read-prohibited
  - s3-bucket-public-write-prohibited  
  - s3-bucket-server-side-encryption-enabled
  - s3-bucket-logging-enabled
  - s3-bucket-versioning-enabled
```

### 🔄 다른 서비스들의 한계

### **Trusted Advisor 한계 (B번)**
```yaml
제공하는 것:
  - S3 버킷 권한 일회성 체크
  - 보안 모범 사례 권장사항
  - 비용 최적화 제안

제공하지 않는 것:
  ❌ 실시간 변경 모니터링
  ❌ 구성 변경 히스토리
  ❌ 자동 규정 준수 체크
  ❌ 알림 및 대응 기능
```

### **Inspector 한계 (C번)**
```yaml
서비스 범위:
  - EC2 인스턴스 보안 평가
  - 컨테이너 이미지 스캔
  - 애플리케이션 취약점 분석

S3 관련 기능 없음:
  ❌ S3 구성 평가 불가
  ❌ 버킷 정책 분석 불가
  ❌ 권한 설정 체크 불가
```

### **S3 Access Logging 한계 (D번)**
```yaml
기록하는 것:
  - S3 객체 접근 로그
  - GET, PUT, DELETE 요청
  - 접근자 IP, 시간 등

기록하지 않는 것:
  ❌ 버킷 구성 변경 (API 호출 아님)
  ❌ 정책 변경 사항
  ❌ 권한 설정 변경
  ❌ 암호화 설정 변경
```
