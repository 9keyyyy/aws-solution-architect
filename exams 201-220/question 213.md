## Question #213
A company is developing a new mobile app. 
The company must implement proper traffic filtering to protect its Application Load Balancer (ALB) against common application-level attacks, such as cross-site scripting or SQL injection. 
The company has minimal infrastructure and operational staff. 
The company needs to reduce its share of the responsibility in managing, updating, and securing servers for its AWS environment.

What should a solutions architect recommend to meet these requirements?

A. Configure AWS WAF rules and associate them with the ALB.

B. Deploy the application using Amazon S3 with public hosting enabled.

C. Deploy AWS Shield Advanced and add the ALB as a protected resource.

D. Create a new ALB that directs traffic to an Amazon EC2 instance running a third-party firewall, which then passes the traffic to the current ALB.

## Question #213 분석

### 요구사항
- **모바일 앱** 개발
- **ALB 보호**: XSS, SQL injection 등 애플리케이션 레벨 공격 차단
- **최소 인프라/운영 인력**
- **서버 관리 책임 최소화**

### 선택지 분석

**A. AWS WAF 규칙 + ALB 연결** ⭐
- **애플리케이션 레벨 보호**: XSS, SQL injection 전용 차단 ✅
- **완전 관리형**: AWS가 인프라 및 업데이트 담당 ✅
- **운영 부담 없음**: 규칙 설정 후 자동 운영 ✅
- **즉시 통합**: ALB와 직접 연결 가능 ✅

**B. S3 퍼블릭 호스팅**
- **용도 불일치**: 정적 웹사이트용, 모바일 앱 백엔드 부적합 ❌
- **ALB 보호 무관**: 기존 ALB 문제 해결 안됨 ❌

**C. AWS Shield Advanced**
- **DDoS 보호**: 네트워크 레벨 공격 차단용 ❌
- **애플리케이션 공격 미대응**: XSS, SQL injection 차단 불가 ❌
- **요구사항 불일치**: 애플리케이션 레벨 보호 아님ㅇ ❌

**D. 써드파티 방화벽 EC2**
- **관리 부담 증가**: EC2 인스턴스 직접 관리 필요 ❌
- **복잡성**: 이중 ALB 구조로 아키텍처 복잡화 ❌
- **서버 책임 증가**: 요구사항과 정반대 ❌

### AWS WAF 핵심 기능

```yaml
애플리케이션 보호:
✅ SQL Injection 차단
✅ Cross-site Scripting (XSS) 차단
✅ 관리형 규칙 세트 제공
✅ 커스텀 규칙 생성 가능

운영 효율성:
✅ 완전 관리형 서비스
✅ 자동 업데이트
✅ 실시간 모니터링
✅ 최소 설정으로 즉시 보호
```

### ALB + WAF 통합

```yaml
설정 과정:
1. AWS WAF Web ACL 생성
2. 관리형 규칙 세트 활성화
3. ALB에 Web ACL 연결
4. 즉시 보호 시작

관리 부담:
- 서버 관리: 불필요
- 업데이트: 자동
- 모니터링: 내장
```
