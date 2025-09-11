## Question #1006
A company uses AWS Systems Manager for routine management and patching of Amazon EC2 instances. The EC2 instances are in an IP address type target group behind an Application Load Balancer (ALB).

New security protocols require the company to remove EC2 instances from service during a patch. When the company attempts to follow the security protocol during the next patch, the company receives errors during the patching window.

Which combination of solutions will resolve the errors? (Choose two.)

A. Change the target type of the target group from IP address type to instance type.

B. Continue to use the existing Systems Manager document without changes because it is already optimized to handle instances that are in an IP address type target group behind an ALB.

C. Implement the AWSEC2-PatchLoadBalanacerInstance Systems Manager Automation document to manage the patching process.

D. Use Systems Manager Maintenance Windows to automatically remove the instances from service to patch the instances.

E. Configure Systems Manager State Manager to remove the instances from service and manage the patching schedule. Use ALB health checks to re-route traffic.

## Question #1006 분석

### 문제 상황
- **Systems Manager**로 EC2 패칭
- EC2가 **IP 타입 타겟 그룹** + ALB 뒤에 위치
- **패칭 중 인스턴스 제거** 필요 (보안 프로토콜)
- 패칭 중 **에러 발생**

### 핵심 요구사항
- **패칭 중 서비스에서 제거**
- **에러 해결**
- **ALB 연동**

### 에러 원인 분석
**IP 타입 타겟 그룹**에서는 인스턴스 자동 제거가 복잡함

### 선택지 분석

**A. IP 타입 → Instance 타입 변경** ⭐
- Instance 타입에서 ALB 연동이 더 쉬움 ✅
- 자동 드레이닝 지원 ✅

**B. 기존 문서 그대로 사용**
- 이미 에러 발생했는데 변경 없이는 해결 안됨 ❌

**C. PatchLoadBalancerInstance 문서** ⭐
- ALB + 패칭 전용 자동화 문서 ✅
- 자동으로 인스턴스 제거 후 패칭 ✅

**D. Maintenance Windows**
- 스케줄링만 담당, ALB 제거 기능 없음 ❌

**E. State Manager + ALB Health Check**
- State Manager는 원하는 상태 유지용 ❌
- 패칭 워크플로우에 부적합 ❌

### AWSEC2-PatchLoadBalancerInstance

```yaml
기능:
- ALB에서 인스턴스 자동 제거
- 패칭 수행
- 패칭 완료 후 다시 ALB에 추가
- 에러 처리 및 롤백
```

### Target Group 타입 차이

**Instance 타입**: 인스턴스 ID로 등록 → 자동 관리 쉬움
**IP 타입**: IP 주소로 등록 → 수동 관리 필요

