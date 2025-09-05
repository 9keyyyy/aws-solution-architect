## Question #60
A company has a website hosted on AWS. 
The website is behind an Application Load Balancer (ALB) that is configured to handle HTTP and HTTPS separately. 
The company wants to forward all requests to the website so that the requests will use HTTPS.

What should a solutions architect do to meet this requirement?

A. Update the ALB's network ACL to accept only HTTPS traffic.

B. Create a rule that replaces the HTTP in the URL with HTTPS.

C. Create a listener rule on the ALB to redirect HTTP traffic to HTTPS.

D. Replace the ALB with a Network Load Balancer configured to use Server Name Indication (SNI).

## Question #60 분석

### ✅ 요구사항
- **AWS에서 웹사이트 호스팅**
- **ALB**가 HTTP와 HTTPS를 **별도로 처리**
- **모든 요청을 HTTPS로 전달** 원함
- HTTP 요청을 HTTPS로 리디렉션 필요

### ✅ 선택지 분석

**A. ALB의 네트워크 ACL을 HTTPS 트래픽만 허용**
- **네트워크 ACL**: 서브넷 레벨 방화벽, ALB 설정과 무관 ❌
- **트래픽 차단**: HTTP 요청을 완전 차단 (리디렉션 불가능) ❌
- **사용자 경험**: HTTP 접속 시 연결 거부로 나쁜 UX ❌
- **잘못된 레이어**: L3/L4 보안 vs L7 리디렉션 ❌

**B. URL에서 HTTP를 HTTPS로 교체하는 규칙**
- **모호한 설명**: 구체적인 구현 방법 불명확 ❌
- **ALB 기능 아님**: ALB에서 URL 문자열 교체 기능 없음 ❌
- **클라이언트 측 처리**: 서버 측 리디렉션과 다른 접근 ❌

**C. ALB에서 HTTP를 HTTPS로 리디렉션하는 리스너 규칙** ⭐
- **ALB 네이티브 기능**: 리디렉션 액션 지원 ✅
- **HTTP 301/302**: 표준 HTTP 리디렉션 응답 ✅
- **사용자 경험**: 자동으로 HTTPS로 리디렉션 ✅
- **보안 강화**: 모든 트래픽이 암호화됨 ✅
- **간단한 구현**: ALB 콘솔에서 쉽게 설정 ✅

**D. ALB를 SNI 지원 NLB로 교체**
- **불필요한 변경**: 기존 ALB로 충분히 해결 가능 ❌
- **SNI**: 여러 SSL 인증서 지원 기능, 리디렉션과 무관 ❌
- **NLB 한계**: L4 로드밸런서로 HTTP 리디렉션 불가능 ❌
- **복잡성 증가**: 기존 구성 재설계 필요 ❌

### 📋 핵심 개념 정리

#### **ALB 리디렉션 기능**
```yaml
리스너 규칙:
  ✅ 조건별 트래픽 라우팅
  ✅ 리디렉션 액션 지원
  ✅ HTTP → HTTPS 자동 변환
  
리디렉션 액션:
  ✅ 301 (Permanent) / 302 (Temporary)
  ✅ 포트 변경 (80 → 443)
  ✅ 프로토콜 변경 (HTTP → HTTPS)
  ✅ 경로 유지 또는 변경
```

#### **Load Balancer 비교**
```yaml
Application Load Balancer (ALB):
  ✅ L7 로드밸런서
  ✅ HTTP/HTTPS 프로토콜 처리
  ✅ 리디렉션, 경로 기반 라우팅
  ✅ SSL 터미네이션

Network Load Balancer (NLB):
  ✅ L4 로드밸런서
  ✅ TCP/UDP 프로토콜 처리
  ❌ HTTP 리디렉션 불가능
  ✅ 고성능, 낮은 지연시간
```

#### **정답 시나리오 (C번)**
```yaml
1. ALB 리스너 구성 확인
   - HTTP 리스너 (포트 80)
   - HTTPS 리스너 (포트 443)
   
2. HTTP 리스너에 리디렉션 규칙 추가
   - 조건: 모든 HTTP 요청
   - 액션: HTTPS로 리디렉션
   - 상태 코드: 301 (영구 리디렉션)
   
3. 리디렉션 설정
   - Protocol: HTTPS
   - Port: 443
   - Path: #{path} (기존 경로 유지)
   - Query: #{query} (기존 쿼리 유지)
   
4. 사용자 경험
   http://example.com/page
   ↓ (301 Redirect)
   https://example.com/page
   
장점:
  ✅ 자동 HTTPS 강제
  ✅ SEO 친화적 (301 리디렉션)
  ✅ 보안 강화
  ✅ 간단한 설정
  ✅ 추가 비용 없음
```
