## Question #987
A company recently launched a new application for its customers. The application runs on multiple Amazon EC2 instances across two Availability Zones. End users use TCP to communicate with the application.

The application must be highly available and must automatically scale as the number of users increases.

Which combination of steps will meet these requirements MOST cost-effectively? (Choose two.)

A. Add a Network Load Balancer in front of the EC2 instances.

B. Configure an Auto Scaling group for the EC2 instances.

C. Add an Application Load Balancer in front of the EC2 instances.

D. Manually add more EC2 instances for the application.

E. Add a Gateway Load Balancer in front of the EC2 instances.

## Question #987 분석

### 문제 상황
- **새 애플리케이션** 다중 EC2, 2개 AZ
- **TCP 통신** 사용
- **고가용성** + **자동 확장** 필요
- **비용 효율성** 중요

### 핵심 요구사항
- **TCP 프로토콜 지원**
- **자동 확장**
- **고가용성**
- **비용 효율성**

### 선택지 분석

**A. Network Load Balancer** ⭐
- TCP 프로토콜 지원 ✅
- Layer 4 로드 밸런서 ✅
- 고가용성 제공 ✅

**B. Auto Scaling Group** ⭐
- 자동 확장/축소 ✅
- 사용자 증가에 따른 스케일링 ✅
- 비용 효율성 ✅

**C. Application Load Balancer**
- HTTP/HTTPS 전용 (Layer 7) ❌
- TCP 애플리케이션에 부적합 ❌

**D. 수동 EC2 추가**
- 자동 확장 아님 ❌
- 운영 부담 증가 ❌

**E. Gateway Load Balancer**
- 보안 어플라이언스용 ❌
- 일반 애플리케이션에 부적합 ❌

### Load Balancer 선택

**TCP 통신** → **Network Load Balancer** 필수

| Load Balancer | 프로토콜 | 적용 |
|---------------|----------|------|
| **NLB** | TCP/UDP | ✅ 적합 |
| **ALB** | HTTP/HTTPS | ❌ 부적합 |
| **GWLB** | 보안 어플라이언스 | ❌ 부적합 |

