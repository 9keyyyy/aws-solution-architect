## Question #223
A company has deployed a Java Spring Boot application as a pod that runs on Amazon Elastic Kubernetes Service (Amazon EKS) in private subnets. 
The application needs to write data to an Amazon DynamoDB table. 
A solutions architect must ensure that the application can interact with the DynamoDB table without exposing traffic to the internet.

Which combination of steps should the solutions architect take to accomplish this goal? (Choose two.)

A. Attach an IAM role that has sufficient privileges to the EKS pod.

B. Attach an IAM user that has sufficient privileges to the EKS pod.

C. Allow outbound connectivity to the DynamoDB table through the private subnets’ network ACLs.

D. Create a VPC endpoint for DynamoDB.

E. Embed the access keys in the Java Spring Boot code.

## Question #223 분석 (Choose Two)

### 요구사항
- **EKS Pod** (프라이빗 서브넷)에서 DynamoDB 접근
- **인터넷 노출 없이** DynamoDB 통신
- **Java Spring Boot** 애플리케이션

### 선택지 분석

**A. EKS Pod에 IAM Role 연결** ⭐
- **서비스 계정 기반**: EKS에서 Pod에 IAM Role 안전하게 부여 ✅
- **임시 자격증명**: 자동 토큰 로테이션으로 보안 강화 ✅
- **AWS 권장 방식**: IRSA(IAM Roles for Service Accounts) ✅

**B. EKS Pod에 IAM User 연결**
- **장기 자격증명**: 보안 위험이 높은 방식 ❌
- **키 관리 부담**: 액세스 키 수동 관리 필요 ❌

**C. Network ACL 아웃바운드 허용**
- **인터넷 경유**: 여전히 퍼블릭 인터넷을 통한 접근 ❌
- **요구사항 미충족**: "인터넷 노출 없이" 조건 위반 ❌

**D. DynamoDB VPC Endpoint 생성** ⭐
- **프라이빗 연결**: VPC 내부에서 DynamoDB 직접 접근 ✅
- **인터넷 우회**: AWS 백본 네트워크만 사용 ✅
- **요구사항 충족**: 인터넷 노출 없는 완전한 해결책 ✅

**E. 코드에 액세스 키 하드코딩**
- **보안 위험**: 소스코드 노출 시 자격증명 유출 ❌
- **관리 복잡성**: 키 로테이션 어려움 ❌
- **안티 패턴**: AWS 보안 모범 사례 위반 ❌

### IRSA (IAM Roles for Service Accounts)

```yaml
설정 과정:
1. EKS 클러스터에서 OIDC Identity Provider 활성화
2. Kubernetes Service Account 생성
3. IAM Role 생성 및 Trust Policy 설정
4. Pod에 Service Account 연결

장점:
- 자동 토큰 갱신
- 세밀한 권한 제어
- CloudTrail 감사 지원
```

### VPC Endpoint for DynamoDB

```yaml
Gateway Endpoint:
- DynamoDB 전용 VPC Endpoint
- 라우팅 테이블 기반 동작
- 추가 비용 없음
- 인터넷 완전 우회
```
