## Question #1008
A company hosts a website analytics application on a single Amazon EC2 On-Demand Instance. The analytics application is highly resilient and is designed to run in stateless mode.

The company notices that the application is showing signs of performance degradation during busy times and is presenting 5xx errors. The company needs to make the application scale seamlessly.

Which solution will meet these requirements MOST cost-effectively?

A. Create an Amazon Machine Image (AMI) of the web application. Use the AMI to launch a second EC2 On-Demand Instance. Use an Application Load Balancer to distribute the load across the two EC2 instances.

B. Create an Amazon Machine Image (AMI) of the web application. Use the AMI to launch a second EC2 On-Demand Instance. Use Amazon Route 53 weighted routing to distribute the load across the two EC2 instances.

C. Create an AWS Lambda function to stop the EC2 instance and change the instance type. Create an Amazon CloudWatch alarm to invoke the Lambda function when CPU utilization is more than 75%.

D. Create an Amazon Machine Image (AMI) of the web application. Apply the AMI to a launch template. Create an Auto Scaling group that includes the launch template. Configure the launch template to use a Spot Fleet. Attach an Application Load Balancer to the Auto Scaling group.

## Question #1008 분석

### 문제 상황
- **단일 EC2 인스턴스**에서 웹사이트 분석 애플리케이션
- **Stateless** 애플리케이션
- 피크 시간에 **성능 저하 + 5xx 에러**
- **원활한 확장** 필요

### 핵심 요구사항
- **자동 확장**
- **비용 효율성**
- **고가용성**

### 선택지 분석

**A. 수동으로 두 번째 인스턴스 + ALB**
- 수동 확장 → 자동 확장 아님 ❌
- 피크 시간에만 필요한데 항상 2개 실행 → 비효율 ❌

**B. 수동으로 두 번째 인스턴스 + Route 53**
- 수동 확장 → 자동 확장 아님 ❌
- Route 53은 로드 밸런싱보다 DNS 라우팅용 ❌

**C. Lambda로 인스턴스 타입 변경**
- 수직 확장 → 재시작 필요 (다운타임) ❌
- 원활한 확장 아님 ❌

**D. Auto Scaling + Spot Fleet + ALB** ⭐
- 자동 수평 확장 ✅
- Spot 인스턴스로 비용 절약 ✅
- ALB로 고가용성 ✅
- 원활한 확장 ✅

### Auto Scaling + Spot Fleet 장점

```yaml
Auto Scaling:
- 트래픽에 따른 자동 확장/축소
- 피크 시간에만 인스턴스 추가

Spot Fleet:
- On-Demand 대비 90% 비용 절약
- Stateless 앱에 적합 (중단 허용 가능)

ALB:
- 고가용성 보장
- 헬스 체크로 장애 인스턴스 제외
```
