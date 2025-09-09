## Question #253
 solutions architect has created two IAM policies: Policy1 and Policy2. Both policies are attached to an IAM group.





A cloud engineer is added as an IAM user to the IAM group. Which action will the cloud engineer be able to perform?

A. Deleting IAM users

B. Deleting directories

C. Deleting Amazon EC2 instances

D. Deleting logs from Amazon CloudWatch Logs

## Policy 1 분석

```yaml
Effect: Allow
Actions:
- iam:Get*
- iam:List*
- kms:List*
- ec2:*
- ds:*
- logs:Get*
- logs:Describe*
Resource: "*"
```

**허용되는 작업:**
- IAM 읽기 전용 (Get, List)
- KMS 목록 조회
- **EC2 모든 작업** (생성, 삭제, 수정 등)
- **Directory Service 모든 작업**
- CloudWatch Logs 읽기 전용

## Policy 2 분석

```yaml
Effect: Deny
Action: ds:Delete*
Resource: "*"
```

**명시적 거부:**
- Directory Service의 모든 삭제 작업

## 최종 권한 (Policy 1 + Policy 2)

```yaml
Deny가 Allow보다 우선:
- EC2: 모든 작업 가능 (삭제 포함) ✅
- Directory Service: 읽기/생성은 가능, 삭제는 거부 ❌
- IAM: 읽기 전용만 가능 (삭제 불가) ❌
- CloudWatch Logs: 읽기 전용만 가능 (삭제 불가) ❌
```

## 선택지 검토

**A. IAM 사용자 삭제**
- Policy 1에서 `iam:Get*`, `iam:List*`만 허용
- 삭제 권한 없음 ❌

**B. 디렉토리 삭제**
- Policy 1에서 `ds:*` 허용
- Policy 2에서 `ds:Delete*` 명시적 거부
- Deny가 우선하여 불가능 ❌

**C. EC2 인스턴스 삭제** ⭐
- Policy 1에서 `ec2:*` 허용
- Policy 2에서 EC2 관련 제한 없음
- 삭제 가능 ✅

**D. CloudWatch Logs 삭제**
- Policy 1에서 `logs:Get*`, `logs:Describe*`만 허용
- 삭제 권한 없음 ❌
