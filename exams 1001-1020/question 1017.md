## Question #1017
A company has an application that runs on an Amazon Elastic Kubernetes Service (Amazon EKS) cluster on Amazon EC2 instances. 
The application has a UI that uses Amazon DynamoDB and data services that use Amazon S3 as part of the application deployment.

The company must ensure that the EKS Pods for the UI can access only Amazon DynamoDB and that the EKS Pods for the data services can access only Amazon S3. The company uses AWS Identity and Access Management (IAM).

Which solution meals these requirements?

A. Create separate IAM policies for Amazon S3 and DynamoDB access with the required permissions. Attach both IAM policies to the EC2 instance profile. Use role-based access control (RBAC) to control access to Amazon S3 or DynamoDB for the respective EKS Pods.

B. Create separate IAM policies for Amazon S3 and DynamoDB access with the required permissions. Attach the Amazon S3 IAM policy directly to the EKS Pods for the data services and the DynamoDB policy to the EKS Pods for the UI.

C. Create separate Kubernetes service accounts for the UI and data services to assume an IAM role. Attach the AmazonS3FullAccess policy to the data services account and the AmazonDynamoDBFullAccess policy to the UI service account.

D. Create separate Kubernetes service accounts for the UI and data services to assume an IAM role. Use IAM Role for Service Accounts (IRSA) to provide access to the EKS Pods for the UI to Amazon S3 and the EKS Pods for the data services to DynamoDB.

## AWS 권한 부여 메커니즘 개요

### 기본 권한 부여 방식

**1. IAM User/Role + Policy**
- 직접적인 권한 부여
- 장기 자격증명 (User) vs 임시 자격증명 (Role)

**2. 서비스별 특화 방식**
- **EC2**: Instance Profile (IAM Role 연결)
- **Lambda**: Execution Role
- **EKS**: IRSA (IAM Role for Service Accounts)
- **ECS**: Task Role

**3. 리소스 기반 정책**
- S3 Bucket Policy, API Gateway Resource Policy 등

---

## Question #1017 - EKS Pod 권한 분리

### 선택지 분석

**A. EC2 Instance Profile + RBAC**
```yaml
문제점:
- EC2에 모든 권한 → 과도한 권한
- 모든 Pod가 동일한 권한 상속
- RBAC는 Kubernetes 리소스용, AWS 서비스 제어 불가
결과: 권한 분리 불가 ❌
```

**B. Pod에 IAM 정책 직접 연결**
```yaml
문제점:
- Pod는 Kubernetes 객체, IAM 정책 직접 연결 불가
- 기술적으로 불가능한 방법
결과: 구현 불가 ❌
```

**C. Service Account + 잘못된 권한**
```yaml
구조는 맞지만:
- UI → S3 (잘못됨, DynamoDB 필요)
- Data Service → DynamoDB (잘못됨, S3 필요)
결과: 요구사항 불일치 ❌
```

**D. IRSA + 올바른 권한** ⭐
```yaml
올바른 구현:
- UI Service Account → DynamoDB Role
- Data Service Account → S3 Role
- IRSA로 연결
결과: 정확한 권한 분리 ✅
```

### EKS 권한 모델

**IRSA (IAM Role for Service Accounts)**
- Kubernetes Service Account ↔ IAM Role 매핑
- Pod별 독립적인 AWS 권한
- 최소 권한 원칙 구현 가능
