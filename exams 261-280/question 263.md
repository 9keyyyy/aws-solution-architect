## Question #263
A company is building an application that consists of several microservices. 
The company has decided to use container technologies to deploy its software on AWS. 
The company needs a solution that minimizes the amount of ongoing effort for maintenance and scaling. 
The company cannot manage additional infrastructure.

Which combination of actions should a solutions architect take to meet these requirements? (Choose two.)

A. Deploy an Amazon Elastic Container Service (Amazon ECS) cluster.

B. Deploy the Kubernetes control plane on Amazon EC2 instances that span multiple Availability Zones.

C. Deploy an Amazon Elastic Container Service (Amazon ECS) service with an Amazon EC2 launch type. Specify a desired task number level of greater than or equal to 2.

D. Deploy an Amazon Elastic Container Service (Amazon ECS) service with a Fargate launch type. Specify a desired task number level of greater than or equal to 2.

E. Deploy Kubernetes worker nodes on Amazon EC2 instances that span multiple Availability Zones. Create a deployment that specifies two or more replicas for each microservice.

## Question #263 분석 (Choose Two)

### 요구사항
- **마이크로서비스** 컨테이너 배포
- **유지보수 및 확장 노력 최소화**
- **추가 인프라 관리 불가**

### 선택지 분석

**A. ECS 클러스터 배포** ⭐
- **관리형 서비스**: AWS가 클러스터 관리 담당 ✅
- **인프라 추상화**: 컨테이너 오케스트레이션 자동화 ✅
- **확장성**: 자동 스케일링 지원 ✅

**B. EC2에 Kubernetes 컨트롤 플레인 배포**
- **인프라 관리 필요**: 컨트롤 플레인 직접 관리 ❌
- **요구사항 위반**: 추가 인프라 관리 불가 조건 불충족 ❌

**C. ECS + EC2 런치 타입**
- **EC2 관리 부담**: 인스턴스 패치, 스케일링 직접 관리 ❌
- **인프라 관리**: 기본 EC2 인프라 운영 필요 ❌

**D. ECS + Fargate 런치 타입** ⭐
- **서버리스 컨테이너**: 인프라 관리 완전 불필요 ✅
- **자동 확장**: 태스크 수 기반 자동 스케일링 ✅
- **고가용성**: 2개 이상 태스크로 가용성 보장 ✅
- **유지보수 최소**: AWS가 모든 인프라 관리 ✅

**E. EC2에 Kubernetes 워커 노드**
- **동일한 관리 부담**: EC2 인스턴스 직접 관리 필요 ❌
- **운영 복잡성**: Kubernetes 클러스터 운영 노하우 필요 ❌

### Fargate의 장점

```yaml
완전 서버리스:
- 서버 프로비저닝 불필요
- 패치 관리 자동
- 용량 계획 불필요

운영 효율성:
- 컨테이너 정의만 관리
- 자동 스케일링
- 내장 로드 밸런싱
- 모니터링 통합
```
