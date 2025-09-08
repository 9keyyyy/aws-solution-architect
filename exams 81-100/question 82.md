## Question #82
A company hosts its web applications in the AWS Cloud. 
The company configures Elastic Load Balancers to use certificates that are imported into AWS Certificate Manager (ACM). 
The company's security team must be notified 30 days before the expiration of each certificate.

What should a solutions architect recommend to meet this requirement?

A. Add a rule in ACM to publish a custom message to an Amazon Simple Notification Service (Amazon SNS) topic every day, beginning 30 days before any certificate will expire.

B. Create an AWS Config rule that checks for certificates that will expire within 30 days. Configure Amazon EventBridge (Amazon CloudWatch Events) to invoke a custom alert by way of Amazon Simple Notification Service (Amazon SNS) when AWS Config reports a noncompliant resource.

C. Use AWS Trusted Advisor to check for certificates that will expire within 30 days. Create an Amazon CloudWatch alarm that is based on Trusted Advisor metrics for check status changes. Configure the alarm to send a custom alert by way of Amazon Simple Notification Service (Amazon SNS).

D. Create an Amazon EventBridge (Amazon CloudWatch Events) rule to detect any certificates that will expire within 30 days. Configure the rule to invoke an AWS Lambda function. Configure the Lambda function to send a custom alert by way of Amazon Simple Notification Service (Amazon SNS).

## Question #82 분석

### ✅ 요구사항
- **ACM 인증서** 만료 모니터링
- **30일 전 알림** 필요
- **보안팀 알림** 전송
- **자동화된 솔루션** 구현

### ✅ 핵심 고려사항
```yaml
인증서 만료 관리:
✅ 자동 감지 메커니즘
✅ 정확한 타이밍 (30일 전)
✅ 신뢰할 수 있는 알림
✅ 운영 부담 최소화

AWS 서비스 특성:
- ACM: 인증서 관리 서비스
- EventBridge: 이벤트 기반 트리거
- AWS Config: 리소스 규정 준수 모니터링
- Trusted Advisor: 모범 사례 체크
```

### ✅ 선택지 분석

**A. ACM 규칙 + SNS 일일 발행**
- **ACM 기능 오해**: ACM에는 커스텀 규칙 생성 기능 없음 ❌
- **일일 메시지**: 불필요한 스팸 알림 생성 ❌
- **기술적 불가능**: ACM은 이벤트 발행 기능 제공 안함 ❌
- **서비스 이해 부족**: ACM 기능 범위 오해 ❌

**B. AWS Config 규칙 + EventBridge + SNS** ⭐
- **Config 규칙**: 인증서 만료 체크 가능 ✅
- **EventBridge 연동**: Config 상태 변경 감지 ✅
- **SNS 알림**: 보안팀 알림 전송 ✅
- **자동화**: 완전 자동화된 워크플로우 ✅
- **정확성**: 30일 전 정확한 감지 ✅

**C. Trusted Advisor + CloudWatch + SNS**
- **Trusted Advisor**: SSL 인증서 체크 제공 ✅
- **제한된 접근**: Business/Enterprise 플랜만 ⚠️
- **체크 빈도**: 주 1회만 실행 ❌
- **정확성 부족**: 30일 정확한 타이밍 어려움 ❌
- **세밀함 부족**: 개별 인증서 세부 추적 제한 ❌

**D. EventBridge 직접 규칙 + Lambda + SNS** 
- **EventBridge 규칙**: 스케줄 기반 트리거 ✅
- **Lambda 로직**: 커스텀 인증서 체크 로직 ✅
- **SNS 알림**: 유연한 알림 설정 ✅
- **완전 제어**: 정확한 30일 전 체크 ✅
- **확장성**: 다양한 알림 로직 구현 가능 ✅

### 📋 핵심 개념 정리

#### **ACM 인증서 모니터링 방법**
```yaml
AWS Config 접근법:
- 미리 정의된 규칙 사용
- 지속적 모니터링
- 규정 준수 관점

EventBridge + Lambda 접근법:
- 커스텀 로직 구현
- 정확한 타이밍 제어
- 유연한 알림 설정

Trusted Advisor 접근법:
- 제한된 계정 타입만
- 주기적 체크만
- 일반적 모범 사례 중심
```

#### **B안 vs D안 상세 비교**
```yaml
B안 (AWS Config):
장점:
✅ 미리 구성된 규칙 (acm-certificate-expiration-check)
✅ 지속적 모니터링
✅ 규정 준수 추적
✅ 개발 부담 없음

단점:
⚠️ Config 비용 발생
⚠️ 제한된 커스터마이징
⚠️ 추가 구성 복잡성

D안 (EventBridge + Lambda):
장점:
✅ 정확한 타이밍 제어
✅ 커스텀 로직 구현
✅ 비용 효율적
✅ 유연한 알림 설정

단점:
⚠️ 개발 및 유지보수 필요
⚠️ 커스텀 코드 관리
```

#### **실제 구현 복잡도 고려**
```yaml
운영 효율성:
B안: 설정 후 자동 운영 (높음)
D안: 코드 개발 및 유지보수 (보통)

정확성:
B안: AWS Config 규칙 신뢰성 (높음)
D안: 커스텀 로직 정확성 (개발 품질 의존)

비용 효율성:
B안: Config 비용 + EventBridge + SNS
D안: Lambda + EventBridge + SNS (더 저렴)

확장성:
B안: Config 규칙 기능 범위 내
D안: 무제한 커스터마이징 가능
```

#### **B안이 더 적절한 이유**
```yaml
1. 운영 안정성
   ✅ AWS 관리형 서비스 활용
   ✅ 검증된 Config 규칙 사용
   ✅ 지속적 모니터링 보장

2. 구현 복잡도
   ✅ 코드 개발 불필요
   ✅ 설정 기반 구성
   ✅ 유지보수 부담 없음

3. 신뢰성
   ✅ AWS 서비스 SLA 보장
   ✅ 검증된 기능 사용
   ✅ 장애 위험 최소화
```

#### **정답 시나리오 (B번)**
```yaml
1. AWS Config 활성화
   - Config 서비스 설정
   - S3 버킷 구성 (설정 기록)
   - 서비스 링크 역할 생성

2. Config 규칙 생성
   Rule Name: acm-certificate-expiration-check
   Parameters:
   - daysToExpiration: 30
   - Scope: ACM 인증서 리소스

3. EventBridge 규칙 설정
   {
     "Name": "ACM-Certificate-Expiry-Alert",
     "EventPattern": {
       "source": ["aws.config"],
       "detail-type": ["Config Rules Compliance Change"],
       "detail": {
         "configRuleName": ["acm-certificate-expiration-check"],
         "newEvaluationResult": {
           "complianceType": ["NON_COMPLIANT"]
         }
       }
     },
     "Targets": [{
       "Id": "SNS-Alert-Target",
       "Arn": "arn:aws:sns:region:account:security-alerts"
     }]
   }

4. SNS 토픽 설정
   - 보안팀 이메일 구독
   - 메시지 포맷 설정
   - 접근 권한 구성

5. 테스트 및 검증
   - Config 규칙 동작 확인
   - EventBridge 트리거 테스트
   - SNS 알림 수신 확인
```