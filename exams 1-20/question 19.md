## Question #19
A company has a three-tier web application that is deployed on AWS. 
The web servers are deployed in a public subnet in a VPC. 
The application servers and database servers are deployed in private subnets in the same VPC. 
The company has deployed a third-party virtual firewall appliance from AWS Marketplace in an inspection VPC. 
The appliance is configured with an IP interface that can accept IP packets.
A solutions architect needs to integrate the web application with the appliance to inspect all traffic to the application before the traffic reaches the web server.

Which solution will meet these requirements with the LEAST operational overhead?

A. Create a Network Load Balancer in the public subnet of the application's VPC to route the traffic to the appliance for packet inspection.

B. Create an Application Load Balancer in the public subnet of the application's VPC to route the traffic to the appliance for packet inspection.

C. Deploy a transit gateway in the inspection VPConfigure route tables to route the incoming packets through the transit gateway.

D. Deploy a Gateway Load Balancer in the inspection VPC. Create a Gateway Load Balancer endpoint to receive the incoming packets and forward the packets to the appliance.

## Question #19 분석

### ✅ 요구사항
- 3계층 웹 애플리케이션 (웹서버-앱서버-DB)
- **모든 트래픽**이 웹서버 도달 **전에** 검사 필요
- 서드파티 방화벽 어플라이언스 (별도 검사 VPC)
- **최소한의 운영 오버헤드**

### ✅ 선택지 분석

**A. Network Load Balancer (NLB)**
- **L4 로드밸런서**: 패킷 검사 기능 없음 ❌
- 단순 트래픽 분산만 제공
- 방화벽 어플라이언스 통합 불가
- 검사 기능 구현 불가

**B. Application Load Balancer (ALB)**
- **L7 로드밸런서**: 패킷 검사 기능 없음 ❌
- HTTP/HTTPS 트래픽 처리 전용
- 방화벽 어플라이언스 연동 불가
- 네트워크 레벨 검사 불가

**C. Transit Gateway**
- **복잡한 네트워킹**: 높은 운영 오버헤드 ❌
- VPC 간 라우팅 복잡성
- 수동 라우트 테이블 관리 필요
- 방화벽 통합 복잡함

**D. Gateway Load Balancer (GWLB)** ⭐
- **네트워크 어플라이언스 전용** 설계
- **투명한 패킷 포워딩**
- 서드파티 보안 어플라이언스 최적화
- **최소 운영 오버헤드**

### 📋 Gateway Load Balancer 개념

### **Gateway Load Balancer (GWLB)**
```yaml
서비스 목적:
  - 네트워크 보안 어플라이언스 통합
  - 투명한 트래픽 검사
  - 서드파티 방화벽/IPS/IDS 지원

핵심 특징:
  - L3 Gateway + L4 Load Balancer 결합
  - GENEVE 터널링 프로토콜 사용
  - 소스/목적지 IP 보존
  - 상태 유지 로드 밸런싱
```

### **GWLB 작동 원리**
```yaml
패킷 흐름:
  1. 인터넷 → IGW
  2. IGW → GWLB Endpoint
  3. GWLB Endpoint → GWLB
  4. GWLB → 방화벽 어플라이언스
  5. 어플라이언스 → GWLB (검사 완료)
  6. GWLB → GWLB Endpoint
  7. GWLB Endpoint → 웹서버

투명성:
  - 원본 패킷 보존
  - NAT 없이 처리
  - 어플라이언스가 실제 IP 확인 가능
```

### 🔄 아키텍처 비교

### **Gateway Load Balancer 방식 (권장)**
```
인터넷 트래픽
    ↓
Internet Gateway
    ↓
GWLB Endpoint (App VPC)
    ↓ (GENEVE 터널)
Gateway Load Balancer (Inspection VPC)
    ↓
방화벽 어플라이언스 (검사)
    ↓
Gateway Load Balancer
    ↓ (GENEVE 터널)
GWLB Endpoint
    ↓
웹서버 (Public Subnet)
```

### **Transit Gateway 방식 (복잡함)**
```
인터넷 트래픽
    ↓
Internet Gateway
    ↓
Transit Gateway
    ↓ (라우팅 복잡)
Inspection VPC
    ↓
방화벽 어플라이언스
    ↓
Transit Gateway
    ↓ (라우팅 복잡)
Application VPC
    ↓
웹서버

운영 복잡성:
- 라우트 테이블 관리
- VPC 간 라우팅 설정
- 보안 그룹 관리
- 네트워크 ACL 설정
```

### **로드밸런서 유형별 비교**
```yaml
Application Load Balancer (ALB):
  - L7 (HTTP/HTTPS) 전용
  - 컨텐츠 기반 라우팅
  - 웹 애플리케이션 최적화
  - 패킷 레벨 검사 불가 ❌

Network Load Balancer (NLB):
  - L4 (TCP/UDP) 처리
  - 고성능 트래픽 분산
  - IP 보존 가능
  - 패킷 검사 기능 없음 ❌

Gateway Load Balancer (GWLB):
  - L3 Gateway + L4 Load Balancer
  - 네트워크 어플라이언스 전용
  - 투명한 패킷 포워딩 ✅
  - 보안 어플라이언스 최적화 ✅
```

### **운영 오버헤드 비교**
```yaml
GWLB 방식:
  ✅ 관리형 서비스
  ✅ 자동 상태 확인
  ✅ 자동 확장
  ✅ 간단한 설정

Transit Gateway 방식:
  ❌ 복잡한 라우팅 관리
  ❌ 수동 라우트 테이블 설정
  ❌ VPC 피어링 복잡성
  ❌ 네트워킹 전문성 필요

ALB/NLB 방식:
  ❌ 패킷 검사 기능 부재
  ❌ 어플라이언스 통합 불가
  ❌ 추가 구성 요소 필요
```

### **GWLB Endpoint**
```yaml
기능:
  - VPC 내 GWLB 진입점
  - PrivateLink 기반 연결
  - GENEVE 터널링 처리
  - 라우팅 테이블 통합

장점:
  - 크로스 VPC 연결
  - 보안 격리 유지
  - 확장성 있는 설계
  - AWS 네이티브 통합
```
