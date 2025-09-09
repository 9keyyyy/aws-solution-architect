## Question #246
A company runs a web application on Amazon EC2 instances in multiple Availability Zones. 
The EC2 instances are in private subnets. 
A solutions architect implements an internet-facing Application Load Balancer (ALB) and specifies the EC2 instances as the target group. 
However, the internet traffic is not reaching the EC2 instances.

How should the solutions architect reconfigure the architecture to resolve this issue?

A. Replace the ALB with a Network Load Balancer. Configure a NAT gateway in a public subnet to allow internet traffic.

B. Move the EC2 instances to public subnets. Add a rule to the EC2 instances’ security groups to allow outbound traffic to 0.0.0.0/0.

C. Update the route tables for the EC2 instances’ subnets to send 0.0.0.0/0 traffic through the internet gateway route. Add a rule to the EC2 instances’ security groups to allow outbound traffic to 0.0.0.0/0.

D. Create public subnets in each Availability Zone. Associate the public subnets with the ALB. Update the route tables for the public subnets with a route to the private subnets.

## Question #246 분석

### 현재 상황
- **EC2 인스턴스**: 프라이빗 서브넷
- **Internet-facing ALB** 구현
- **타겟 그룹**: EC2 인스턴스 지정
- **문제**: 인터넷 트래픽이 EC2에 도달하지 않음

### 핵심 문제
```yaml
ALB 배치 오류:
- Internet-facing ALB는 퍼블릭 서브넷에 배치되어야 함
- 현재 ALB가 어디에 있는지 명시되지 않았지만
- 트래픽이 도달하지 않는 것으로 보아 잘못 배치된 것으로 추정
```

### 선택지 분석

**A. NLB로 교체 + NAT Gateway**
- **로드밸런서 교체 불필요**: ALB 자체는 문제없음 ❌
- **NAT Gateway 용도 오해**: 아웃바운드 트래픽용, 인바운드와 무관 ❌

**B. EC2를 퍼블릭 서브넷으로 이동**
- **보안 위험**: EC2를 퍼블릭에 노출시키는 것은 부적절 ❌
- **아키텍처 원칙 위반**: 웹 서버는 프라이빗 서브넷 유지가 권장 ❌

**C. EC2 서브넷 라우팅 테이블 수정**
- **프라이빗 서브넷 목적 위반**: IGW 라우팅으로 퍼블릭화 ❌
- **보안 리스크**: 프라이빗 서브넷의 의미 상실 ❌

**D. 퍼블릭 서브넷 생성 + ALB 연결** ⭐
- **올바른 ALB 배치**: Internet-facing ALB는 퍼블릭 서브넷에 위치 ✅
- **보안 유지**: EC2는 프라이빗 서브넷에 그대로 유지 ✅
- **정상 트래픽 흐름**: 인터넷 → 퍼블릭 ALB → 프라이빗 EC2 ✅

### 올바른 아키텍처

```yaml
수정 후:
1. 각 AZ에 퍼블릭 서브넷 생성
2. 퍼블릭 서브넷에 IGW 라우팅 설정
3. ALB를 퍼블릭 서브넷에 연결
4. ALB가 프라이빗 서브넷의 EC2를 타겟으로 지정

트래픽 흐름:
인터넷 → IGW → 퍼블릭 서브넷 ALB → 프라이빗 서브넷 EC2
```
