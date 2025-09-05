## Question #58
A company wants to run its critical applications in containers to meet requirements for scalability and availability. 
The company prefers to focus on maintenance of the critical applications. 
The company does not want to be responsible for provisioning and managing the underlying infrastructure that runs the containerized workload.

What should a solutions architect do to meet these requirements?

A. Use Amazon EC2 instances, and install Docker on the instances.

B. Use Amazon Elastic Container Service (Amazon ECS) on Amazon EC2 worker nodes.

C. Use Amazon Elastic Container Service (Amazon ECS) on AWS Fargate.

D. Use Amazon EC2 instances from an Amazon Elastic Container Service (Amazon ECS)-optimized Amazon Machine Image (AMI).

## Question #58 분석

### ✅ 요구사항
- **컨테이너**에서 중요 애플리케이션 실행
- **확장성과 가용성** 충족
- **핵심 애플리케이션 유지보수**에 집중 원함
- **인프라 프로비저닝/관리 책임 회피** (핵심 요구사항)

### ✅ 선택지 분석

**A. EC2 인스턴스 + Docker 설치**
- **인프라 관리**: EC2 인스턴스 직접 관리 필요 ❌
- **Docker 관리**: 컨테이너 런타임 수동 설정 ❌
- **확장성**: 수동 스케일링 필요 ❌
- **최대 운영 부담**: 요구사항과 정반대 ❌

**B. Amazon ECS on EC2 워커 노드**
- **ECS 관리**: 컨테이너 오케스트레이션 자동화 ✅
- **EC2 관리**: 여전히 인스턴스 관리 필요 ❌
- **인프라 책임**: 서버 패치, 스케일링, 모니터링 ❌
- **부분적 관리형**: 완전한 서버리스 아님 ❌

**C. Amazon ECS on AWS Fargate** ⭐
- **완전 서버리스**: 인프라 관리 불필요 ✅
- **자동 확장**: 컨테이너 기반 자동 스케일링 ✅
- **관리 부담 최소**: 애플리케이션에만 집중 ✅
- **고가용성**: AWS가 인프라 가용성 보장 ✅
- **프로비저닝 불필요**: 컨테이너 정의만 필요 ✅

**D. ECS 최적화 AMI 사용**
- **ECS 최적화**: 컨테이너 실행에 최적화된 AMI ✅
- **여전히 EC2**: 인스턴스 관리 책임 존재 ❌
- **인프라 관리**: 서버 운영 부담 지속 ❌

### 📋 핵심 개념 정리

#### **컨테이너 실행 옵션 비교**
```yaml
EC2 + Docker:
  ❌ 인스턴스 관리 (패치, 모니터링)
  ❌ 컨테이너 런타임 관리
  ❌ 수동 스케일링
  ✅ 완전한 제어권

ECS on EC2:
  ✅ 컨테이너 오케스트레이션 자동화
  ❌ EC2 인스턴스 관리 필요
  ✅ 클러스터 스케일링
  ⚠️ 하이브리드 관리 모델

ECS on Fargate:
  ✅ 완전 서버리스
  ✅ 인프라 관리 불필요
  ✅ 자동 스케일링
  ✅ 사용량 기반 과금
```

#### **AWS Fargate 특징**
```yaml
서버리스 컨테이너:
  ✅ 서버 프로비저닝 불필요
  ✅ 용량 계획 불필요
  ✅ 패치 관리 자동화
  ✅ 보안 관리 자동화

운영 모델:
  ✅ 컨테이너 정의만 제공
  ✅ AWS가 모든 인프라 관리
  ✅ 자동 확장/축소
  ✅ 가용성 자동 보장
```

#### **정답 시나리오 (C번)**
```yaml
1. ECS 클러스터 생성 (Fargate 모드)
   - 서버 프로비저닝 불필요
   
2. 태스크 정의 생성
   - 컨테이너 이미지
   - CPU/메모리 요구사항
   - 네트워킹 설정
   
3. ECS 서비스 배포
   - 원하는 태스크 수 지정
   - 자동 스케일링 정책
   - 로드 밸런서 연결

4. 운영 결과
   ✅ 애플리케이션 코드에만 집중
   ✅ AWS가 인프라 완전 관리
   ✅ 자동 확장성
   ✅ 고가용성 보장
   ✅ 패치/보안 자동 관리
```
