## Question #265
A solutions architect needs to design a highly available application consisting of web, application, and database tiers. 
HTTPS content delivery should be as close to the edge as possible, with the least delivery time.

Which solution meets these requirements and is MOST secure?

A. Configure a public Application Load Balancer (ALB) with multiple redundant Amazon EC2 instances in public subnets. Configure Amazon CloudFront to deliver HTTPS content using the public ALB as the origin.

B. Configure a public Application Load Balancer with multiple redundant Amazon EC2 instances in private subnets. Configure Amazon CloudFront to deliver HTTPS content using the EC2 instances as the origin.

C. Configure a public Application Load Balancer (ALB) with multiple redundant Amazon EC2 instances in private subnets. Configure Amazon CloudFront to deliver HTTPS content using the public ALB as the origin.

D. Configure a public Application Load Balancer with multiple redundant Amazon EC2 instances in public subnets. Configure Amazon CloudFront to deliver HTTPS content using the EC2 instances as the origin.

## Question #265 분석

### 요구사항
- **고가용성** 3계층 애플리케이션 (웹, 앱, DB)
- **HTTPS 콘텐츠 전송**을 엣지에 최대한 가깝게
- **최소 전송 시간**
- **최고 보안**

### 선택지 분석

**A. 퍼블릭 ALB + 퍼블릭 서브넷 EC2 + CloudFront → ALB 오리진**
- **보안 취약**: EC2가 퍼블릭 서브넷에 직접 노출 ❌
- **불필요한 노출**: 웹/앱 계층이 인터넷에 직접 접근 가능 ❌

**B. 퍼블릭 ALB + 프라이빗 서브넷 EC2 + CloudFront → EC2 오리진**
- **오리진 설정 오류**: CloudFront가 프라이빗 EC2에 직접 접근 불가 ❌
- **네트워크 문제**: 프라이빗 인스턴스는 CloudFront 오리진으로 부적절 ❌

**C. 퍼블릭 ALB + 프라이빗 서브넷 EC2 + CloudFront → ALB 오리진** ⭐
- **최고 보안**: EC2가 프라이빗 서브넷에서 완전 보호 ✅
- **올바른 아키텍처**: CloudFront → ALB → 프라이빗 EC2 ✅
- **엣지 전송**: CloudFront가 전 세계 엣지에서 HTTPS 제공 ✅
- **고가용성**: 멀티 AZ ALB와 중복 EC2 인스턴스 ✅

**D. 퍼블릭 ALB + 퍼블릭 서브넷 EC2 + CloudFront → EC2 오리진**
- **이중 보안 문제**: 퍼블릭 EC2 + 직접 오리진 접근 ❌
- **복잡한 오리진**: ALB 우회하여 EC2 직접 접근 부적절 ❌

### 최적 보안 아키텍처 (C안)

```yaml
트래픽 흐름:
사용자 → CloudFront (엣지) → ALB (퍼블릭) → EC2 (프라이빗)

보안 계층:
- CloudFront: DDoS 보호, WAF 통합 가능
- ALB: 퍼블릭 서브넷에서 인터넷 트래픽 수신
- EC2: 프라이빗 서브넷에서 완전 격리
- DB: 프라이빗 서브넷에서 최대 보호

성능:
- CloudFront 엣지 캐싱으로 최소 지연시간
- ALB를 통한 효율적 로드 밸런싱
```
