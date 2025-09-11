## Question #996
A company runs an on-premises application on a Kubernetes cluster. The company recently added millions of new customers. The company's existing on-premises infrastructure is unable to handle the large number of new customers. The company needs to migrate the on-premises application to the AWS Cloud.

The company will migrate to an Amazon Elastic Kubernetes Service (Amazon EKS) cluster. The company does not want to manage the underlying compute infrastructure for the new architecture on AWS.

Which solution will meet these requirements with the LEAST operational overhead?

A. Use a self-managed node to supply compute capacity. Deploy the application to the new EKS cluster.

B. Use managed node groups to supply compute capacity. Deploy the application to the new EKS cluster.

C. Use AWS Fargate to supply compute capacity. Create a Fargate profile. Use the Fargate profile to deploy the application.

D. Use managed node groups with Karpenter to supply compute capacity. Deploy the application to the new EKS cluster.

## Question #996 분석

### 문제 상황
- **온프레미스 Kubernetes** 클러스터 실행 중
- **수백만 신규 고객** 추가로 인한 용량 부족
- **EKS로 마이그레이션** 계획
- **컴퓨팅 인프라 관리 원하지 않음**

### 핵심 요구사항
- **EKS 마이그레이션**
- **인프라 관리 최소화**
- **최소 운영 오버헤드**

### EKS 컴퓨팅 옵션 비교

| 옵션 | 관리 수준 | 운영 오버헤드 | 인프라 관리 |
|------|-----------|---------------|-------------|
| **Self-managed** | 직접 관리 | 높음 | 필요 |
| **Managed Node Groups** | 일부 관리 | 중간 | 일부 필요 |
| **Fargate** | 완전 관리 | 낮음 | 불필요 |

### 선택지 분석

**A. Self-managed Node**
- EC2 인스턴스 직접 관리 ❌
- 패치, 업데이트, 스케일링 수동 ❌
- 높은 운영 부담 ❌

**B. Managed Node Groups**
- AWS가 일부 관리하지만 EC2 인스턴스는 여전히 존재 ❌
- 노드 관리 부담 있음 ❌

**C. AWS Fargate** ⭐
- 완전 서버리스 ✅
- 인프라 관리 불필요 ✅
- 최소 운영 오버헤드 ✅
- Pod 수준에서만 관리 ✅

**D. Managed Node Groups + Karpenter**
- 여전히 EC2 노드 관리 필요 ❌
- Karpenter 추가 설정/관리 ❌

### Fargate 장점

```yaml
완전 서버리스:
- EC2 인스턴스 없음
- 노드 관리 불필요
- 자동 스케일링

운영 간소화:
- 패치/업데이트 AWS 담당
- 보안 관리 AWS 담당
- 용량 계획 불필요
```
