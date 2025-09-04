## Question #29
A company provides a Voice over Internet Protocol (VoIP) service that uses UDP connections. 
The service consists of Amazon EC2 instances that run in an Auto Scaling group. 
The company has deployments across multiple AWS Regions.
The company needs to route users to the Region with the lowest latency. 
The company also needs automated failover between Regions.

Which solution will meet these requirements?

A. Deploy a Network Load Balancer (NLB) and an associated target group. Associate the target group with the Auto Scaling group. Use the NLB as an AWS Global Accelerator endpoint in each Region.

B. Deploy an Application Load Balancer (ALB) and an associated target group. Associate the target group with the Auto Scaling group. Use the ALB as an AWS Global Accelerator endpoint in each Region.

C. Deploy a Network Load Balancer (NLB) and an associated target group. Associate the target group with the Auto Scaling group. Create an Amazon Route 53 latency record that points to aliases for each NLB. Create an Amazon CloudFront distribution that uses the latency record as an origin.

D. Deploy an Application Load Balancer (ALB) and an associated target group. Associate the target group with the Auto Scaling group. Create an Amazon Route 53 weighted record that points to aliases for each ALB. Deploy an Amazon CloudFront distribution that uses the weighted record as an origin.

## Question #29 분석

### ✅ 요구사항
- **VoIP 서비스** (UDP 연결)
- **다중 리전** 배포
- **최저 지연 시간** 리전으로 라우팅
- **자동 장애 조치** (Automated Failover)

### ✅ 선택지 분석

**A. NLB + Global Accelerator** ⭐
- **UDP 지원**: NLB는 UDP 완벽 지원 ✅
- **최저 지연**: Global Accelerator의 핵심 기능 ✅
- **자동 장애 조치**: Global Accelerator 내장 ✅
- **VoIP 최적화**: 실시간 트래픽에 최적 ✅

**B. ALB + Global Accelerator**
- **UDP 미지원**: ALB는 HTTP/HTTPS만 지원 
- VoIP 서비스에 부적합
- Layer 7 로드밸런서 불필요

**C. NLB + Route 53 + CloudFront**
- **UDP 지원**: NLB 사용 ✅
- **CloudFront 문제**: UDP 미지원 
- HTTP/HTTPS 캐싱 서비스로 VoIP 부적합

**D. ALB + Route 53 + CloudFront**
- **UDP 미지원**: ALB, CloudFront 모두 
- VoIP 서비스에 완전 부적합
- HTTP 기반 솔루션

### 📋 UDP vs HTTP 프로토콜 차이

### **VoIP 서비스 특성**
```yaml
프로토콜: UDP (User Datagram Protocol)
특징:
  - 실시간 음성/비디오 전송
  - 낮은 지연 시간 필수
  - 연결 상태 없음 (Connectionless)
  - 패킷 순서 보장 불필요
  - 빠른 전송 우선 (신뢰성보다 속도)
```

### **로드밸런서별 프로토콜 지원**
```yaml
Network Load Balancer (NLB):
  지원 프로토콜: TCP, UDP, TLS
  레이어: Layer 4 (전송 계층)
  VoIP 적합성: ✅ 완벽 지원

Application Load Balancer (ALB):
  지원 프로토콜: HTTP, HTTPS, gRPC
  레이어: Layer 7 (애플리케이션 계층)
  VoIP 적합성: ❌ UDP 미지원
```

### 🔄 Global Accelerator vs 기타 솔루션

### **Global Accelerator 장점 (A번)**
```yaml
최적화된 라우팅:
  - AWS 글로벌 네트워크 활용
  - Anycast IP로 최적 엔트리 포인트
  - 지연 시간 최소화 (30-60% 개선)

자동 장애 조치:
  - 엔드포인트 상태 모니터링
  - 장애 시 트래픽 자동 재라우팅
  - 1-3분 내 장애 조치

실시간 트래픽 최적화:
  - UDP 트래픽 네이티브 지원
  - 패킷 레벨 최적화
  - VoIP 품질 향상
```

### **Route 53 + CloudFront 한계 (C, D번)**
```yaml
CloudFront 제한사항:
  ❌ UDP 프로토콜 미지원
  ❌ 웹 콘텐츠 캐싱 전용
  ❌ 실시간 스트리밍 부적합
  ❌ VoIP 트래픽 처리 불가

Route 53만의 한계:
  - DNS 기반 라우팅 (느림)
  - 클라이언트 DNS 캐싱으로 지연
  - 실시간 장애 조치 어려움
```

### 📊 실시간 성능 비교

### **지연 시간 최적화**
```yaml
Global Accelerator:
  - AWS 백본 네트워크 활용
  - 최적 엔트리 포인트 자동 선택
  - 평균 지연 시간: 50-100ms 개선

Route 53 + CloudFront:
  - 공용 인터넷 경로
  - DNS 확인 지연
  - VoIP에 부적합한 HTTP 캐싱
```

### **장애 조치 속도**
```yaml
Global Accelerator:
  - 엔드포인트 실시간 모니터링
  - 30초마다 상태 확인
  - 장애 감지 시 즉시 트래픽 이동

Route 53 Health Check:
  - DNS TTL 만료 대기 필요
  - 클라이언트 DNS 캐시 의존
  - 수분 소요 가능
```
