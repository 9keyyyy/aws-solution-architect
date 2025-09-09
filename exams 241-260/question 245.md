## Question #245
A company is launching an application on AWS. 
The application uses an Application Load Balancer (ALB) to direct traffic to at least two Amazon EC2 instances in a single target group. 
The instances are in an Auto Scaling group for each environment. 
The company requires a development environment and a production environment. 
The production environment will have periods of high traffic.

Which solution will configure the development environment MOST cost-effectively?

A. Reconfigure the target group in the development environment to have only one EC2 instance as a target.

B. Change the ALB balancing algorithm to least outstanding requests.

C. Reduce the size of the EC2 instances in both environments.

D. Reduce the maximum number of EC2 instances in the development environment’s Auto Scaling group.

## Question #245 분석

### 요구사항
- **개발 환경 + 프로덕션 환경**
- **ALB + 최소 2개 EC2** 인스턴스 구성
- **Auto Scaling Group** 각 환경별
- **개발 환경 최대 비용 효율성**

### 선택지 분석

**A. 개발 환경 타겟 그룹을 1개 인스턴스로 재구성**
- **ALB 제약**: ALB는 최소 2개 AZ 필요, 단일 인스턴스로는 고가용성 불가 ❌
- **기술적 문제**: 프로덕션급 로드 밸런서 구성 유지 어려움 ❌

**B. ALB 밸런싱 알고리즘 변경**
- **비용 절약 무관**: 알고리즘 변경은 성능 최적화일 뿐 비용 절감 효과 없음 ❌
- **요구사항 불일치**: 개발 환경 비용과 직접 관련 없음 ❌

**C. 양쪽 환경 EC2 인스턴스 크기 축소**
- **프로덕션 영향**: 프로덕션 환경 성능 저하로 부적절 ❌
- **고트래픽 대응 불가**: 프로덕션의 피크 트래픽 처리 불가능 ❌

**D. 개발 환경 Auto Scaling 최대 인스턴스 수 감소** ⭐
- **개발 환경만 영향**: 프로덕션 성능 유지 ✅
- **비용 효율**: 불필요한 확장 방지로 직접적 비용 절감 ✅
- **유연성 유지**: 필요시 여전히 확장 가능 ✅
- **운영 안전성**: 최소 인스턴스는 유지하여 가용성 보장 ✅

### Auto Scaling 최적화

```yaml
프로덕션 환경:
Min: 2, Desired: 2, Max: 10 (피크 트래픽 대응)

개발 환경 (기존):
Min: 2, Desired: 2, Max: 10 (불필요한 확장 가능)

개발 환경 (최적화):
Min: 2, Desired: 2, Max: 3-4 (제한된 확장)

효과:
- 개발 중 불필요한 스케일링 방지
- 비용 예측 가능성 증가
- 여전히 기본 가용성 유지
```
