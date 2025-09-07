# Question #70
A company's HTTP application is behind a Network Load Balancer (NLB). 
The NLB's target group is configured to use an Amazon EC2 Auto Scaling group with multiple EC2 instances that run the web service.
The company notices that the NLB is not detecting HTTP errors for the application. 
These errors require a manual restart of the EC2 instances that run the web service. 
The company needs to improve the application's availability without writing custom scripts or code.

What should a solutions architect do to meet these requirements?

A. Enable HTTP health checks on the NLB, supplying the URL of the company's application.

B. Add a cron job to the EC2 instances to check the local application's logs once each minute. If HTTP errors are detected. the application will restart.

C. Replace the NLB with an Application Load Balancer. Enable HTTP health checks by supplying the URL of the company's application. Configure an Auto Scaling action to replace unhealthy instances.

D. Create an Amazon Cloud Watch alarm that monitors the UnhealthyHostCount metric for the NLB. Configure an Auto Scaling action to replace unhealthy instances when the alarm is in the ALARM state.

## Question #70 분석

### ✅ 요구사항
- **HTTP 애플리케이션** 서비스 중
- **NLB 뒤에서** 운영
- **HTTP 에러 감지 불가**: NLB가 애플리케이션 에러 탐지 못함
- **수동 재시작 필요**: EC2 인스턴스 수동 개입 현황
- **가용성 개선**: 자동화된 장애 대응
- **커스텀 코드 금지**: 스크립트나 코드 작성 불가

### ✅ 선택지 분석

**A. NLB에서 HTTP 헬스체크 활성화**
- **기술적 제약**: NLB는 L4 로드밸런서 ❌
- **HTTP 헬스체크 미지원**: NLB는 TCP/UDP만 지원 ❌
- **애플리케이션 레벨 감지 불가**: 포트만 확인 가능 ❌
- **근본적 한계**: NLB 특성상 불가능한 요구사항 ❌

**B. Cron Job으로 로그 모니터링**
- **커스텀 스크립트**: 명시적으로 금지된 접근법 ❌
- **수동 설정**: 각 인스턴스별 개별 설정 필요 ❌
- **관리 복잡성**: 스크립트 유지보수 부담 ❌
- **요구사항 위배**: "커스텀 코드 작성 금지" 위반 ❌

**C. ALB로 교체 + HTTP 헬스체크 + Auto Scaling** ⭐
- **ALB HTTP 헬스체크**: L7 애플리케이션 상태 정확 감지 ✅
- **애플리케이션 에러 탐지**: HTTP 상태코드 기반 판단 ✅
- **자동 인스턴스 교체**: 불건전 인스턴스 자동 종료/생성 ✅
- **코드 불필요**: AWS 관리형 기능만 사용 ✅
- **완전 자동화**: 수동 개입 없는 장애 대응 ✅

**D. CloudWatch + UnhealthyHostCount 알람**
- **근본 문제 미해결**: NLB는 여전히 HTTP 에러 감지 불가 ❌
- **잘못된 메트릭**: NLB UnhealthyHostCount는 TCP 기반만 ❌
- **애플리케이션 레벨 감지 불가**: HTTP 500 에러 등 탐지 못함 ❌
- **부분적 해결**: 인프라 장애만 감지 가능 ❌

### 📋 핵심 개념 정리

#### **NLB vs ALB 헬스체크 비교**
```yaml
Network Load Balancer (L4):
  ❌ TCP/UDP 포트 상태만 확인
  ❌ HTTP 상태코드 확인 불가
  ❌ 애플리케이션 로직 에러 감지 불가
  ✅ 초고성능, 저지연
  ✅ 고정 IP 지원

Application Load Balancer (L7):
  ✅ HTTP/HTTPS 상태코드 확인
  ✅ 특정 URL 경로 헬스체크
  ✅ 응답 본문 내용 검증 가능
  ✅ 애플리케이션 레벨 장애 감지
  ✅ 다양한 라우팅 규칙
```

#### **헬스체크 메커니즘**
```yaml
NLB 헬스체크:
  - 대상: TCP 포트 연결성
  - 판단: 포트 응답 여부만
  - 한계: 애플리케이션 상태 무관
  
  예시:
  ✅ 포트 80 열림 = Healthy
  ❌ HTTP 500 에러여도 = Healthy

ALB 헬스체크:
  - 대상: HTTP/HTTPS 응답
  - 판단: 상태코드 + 응답시간
  - 정확성: 실제 애플리케이션 상태
  
  예시:
  ✅ HTTP 200 OK = Healthy
  ❌ HTTP 500 Error = Unhealthy
```

#### **Auto Scaling 헬스체크 통합**
```yaml
ELB 헬스체크 연동:
  1. ALB가 HTTP 에러 감지
  2. 인스턴스를 Unhealthy 마킹
  3. Auto Scaling이 감지
  4. 불건전 인스턴스 종료
  5. 새 인스턴스 자동 시작
  6. 정상 상태 복원

설정 예시:
- Health Check Grace Period: 300초
- Health Check Type: ELB
- Health Check Path: /health
- Success Codes: 200
```

#### **정답 시나리오 (C번)**
```yaml
1. ALB로 로드밸런서 교체
   - HTTP/HTTPS 지원
   - 애플리케이션 레벨 헬스체크
   - 대상 그룹 설정

2. HTTP 헬스체크 구성
   - Path: /health 또는 /
   - Success Code: 200
   - Interval: 30초
   - Timeout: 5초
   - Threshold: 2/5

3. Auto Scaling 헬스체크 연동
   - Health Check Type: ELB
   - Grace Period: 300초
   - 자동 인스턴스 교체 활성화

4. 운영 결과
   ✅ HTTP 500 에러 자동 감지
   ✅ 불건전 인스턴스 자동 종료
   ✅ 새 인스턴스 자동 시작
   ✅ 수동 개입 불필요
   ✅ 애플리케이션 가용성 향상
```

#### **실제 장애 시나리오**
```yaml
Before (NLB):
1. 애플리케이션에서 HTTP 500 에러 발생
2. NLB: "포트 80 열림 = Healthy" ✅
3. 트래픽 계속 라우팅 → 에러 지속
4. 수동 감지 → 수동 재시작 필요

After (ALB):
1. 애플리케이션에서 HTTP 500 에러 발생
2. ALB: "HTTP 500 = Unhealthy" ❌
3. Auto Scaling: 불건전 인스턴스 종료
4. 새 인스턴스 자동 시작
5. 정상 서비스 자동 복원
```
