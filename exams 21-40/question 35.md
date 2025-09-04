## Question #35
A company is preparing to launch a public-facing web application in the AWS Cloud. 
The architecture consists of Amazon EC2 instances within a VPC behind an Elastic Load Balancer (ELB). 
A third-party service is used for the DNS. 
The company's solutions architect must recommend a solution to detect and protect against large-scale DDoS attacks.

Which solution meets these requirements?

A. Enable Amazon GuardDuty on the account.

B. Enable Amazon Inspector on the EC2 instances.

C. Enable AWS Shield and assign Amazon Route 53 to it.

D. Enable AWS Shield Advanced and assign the ELB to it.

## Question #35 분석

### ✅ 요구사항
- **퍼블릭 웹 애플리케이션** (인터넷 노출)
- EC2 + VPC + ELB 아키텍처
- **대규모 DDoS 공격 탐지 및 방어**
- 서드파티 DNS 사용 중

### ✅ 선택지 분석

**A. Amazon GuardDuty**
- **위협 탐지 서비스**: 악성 활동 감지 전문 
- DDoS 방어 기능 없음
- 탐지는 하지만 실제 공격 차단 불가

**B. Amazon Inspector**
- **애플리케이션 보안 평가**: EC2/컨테이너 취약점 스캔 
- 네트워크 공격 방어 기능 없음
- DDoS와 완전 다른 영역

**C. AWS Shield + Route 53**
- **서드파티 DNS 사용 중**: Route 53 사용하지 않음 
- Shield Basic은 제한적 보호
- 아키텍처와 맞지 않음

**D. AWS Shield Advanced + ELB** ⭐
- **DDoS 전용 방어 서비스**: Shield Advanced ✅
- **ELB 보호**: 아키텍처의 핵심 구성요소 ✅
- **대규모 공격 대응**: Advanced 기능으로 완벽 대응 ✅
- **24/7 DDoS Response Team** 지원 ✅

### 📋 DDoS 공격과 방어 이해

### **DDoS 공격의 특성**
```yaml
대규모 분산 서비스 거부 공격:
  - 수만-수십만 개의 봇넷 동원
  - 초당 수십만-수백만 요청
  - 네트워크/애플리케이션 계층 동시 공격
  - 정상 사용자 서비스 마비 목적

공격 계층:
  Layer 3/4: 네트워크 플러딩 (UDP, SYN Flood)
  Layer 7: 애플리케이션 공격 (HTTP GET/POST)
```

### **ELB가 주요 타겟인 이유**
```yaml
퍼블릭 웹 애플리케이션 구조:
  인터넷 → ELB → EC2 인스턴스

ELB = 단일 진입점:
  - 모든 트래픽이 ELB를 거쳐감
  - DDoS 공격의 1차 목표
  - ELB 마비 시 전체 서비스 중단
```

## 🛡️ AWS Shield 서비스 비교

### **AWS Shield Standard (무료)**
```yaml
기본 제공 기능:
  - Layer 3/4 DDoS 보호
  - CloudFront, Route 53 자동 적용
  - ELB 기본 보호
  - 일반적인 네트워크 공격 차단

한계:
  ❌ Layer 7 애플리케이션 공격 보호 제한
  ❌ 대규모 공격 시 완전 방어 어려움
  ❌ 실시간 공격 분석 없음
  ❌ DDoS 전문가 지원 없음
```

### **AWS Shield Advanced (유료)**
```yaml
고급 DDoS 보호:
  ✅ Layer 3/4/7 모든 계층 보호
  ✅ 대규모 공격 완전 방어
  ✅ ELB, CloudFront, Route 53, Global Accelerator 지원
  ✅ 실시간 공격 가시성 및 분석

전문가 지원:
  - 24/7 DDoS Response Team (DRT)
  - 공격 시 즉시 대응 지원
  - 맞춤형 방어 규칙 생성

비용 보호:
  - DDoS 공격으로 인한 추가 비용 보상
  - 스케일링 비용 걱정 없음
```


### **Shield Advanced vs 기본 보호**
```yaml
Shield Advanced 환경:
  ✅ 웹사이트 정상 운영 지속
  ✅ 응답 시간 < 200ms 유지
  ✅ 99.9% 가용성 보장
  ✅ 공격 종료까지 완전 방어

기본 보호만 있는 경우:
  ❌ ELB 과부하로 서비스 중단
  ❌ EC2 Auto Scaling 한계 도달
  ❌ 높은 인프라 비용 발생
  ❌ 고객 서비스 불가
```

### 🔍 다른 서비스들의 한계

#### **GuardDuty 한계 (A번)**
```yaml
GuardDuty 역할:
  ✅ 위협 탐지 (Threat Detection)
  ✅ 악성 IP, 멀웨어 감지
  ✅ 비정상 활동 알림

DDoS 대응 불가:
  ❌ 공격 차단 기능 없음
  ❌ 트래픽 필터링 불가
  ❌ 탐지만 하고 방어하지 못함

```

#### **Inspector 한계 (B번)**
```yaml
Inspector 역할:
  ✅ EC2/컨테이너 취약점 스캔
  ✅ 애플리케이션 보안 평가
  ✅ 패치 관리 권장사항

DDoS와 무관:
  ❌ 네트워크 공격 대응 불가
  ❌ 런타임 보호 기능 없음
  ❌ 완전 다른 보안 영역
```

#### **Route 53 없는 Shield (C번)**
```yaml
문제점:
  ❌ 서드파티 DNS 사용 중
  ❌ Route 53 사용하지 않음
  ❌ 아키텍처와 맞지 않음

Shield Basic 한계:
  - 제한적인 Layer 7 보호
  - 대규모 공격 시 불충분
  - 전문가 지원 없음
```
