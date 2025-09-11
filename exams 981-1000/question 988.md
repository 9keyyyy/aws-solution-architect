## Question #988
A company is designing the architecture for a new mobile app that uses the AWS Cloud. The company uses organizational units (OUs) in AWS Organizations to manage its accounts. The company wants to tag Amazon EC2 instances with data sensitivity by using values of sensitive and nonsensitive. IAM identities must not be able to delete a tag or create instances without a tag.

Which combination of steps will meet these requirements? (Choose two.)

A. In Organizations, create a new tag policy that specifies the data sensitivity tag key and the required values. Enforce the tag values for the EC2 instances. Attach the tag policy to the appropriate OU.

B. In Organizations, create a new service control policy (SCP) that specifies the data sensitivity tag key and the required tag values. Enforce the tag values for the EC2 instances. Attach the SCP to the appropriate OU.

C. Create a tag policy to deny running instances when a tag key is not specified. Create another tag policy that prevents identities from deleting tags. Attach the tag policies to the appropriate OU.

D. Create a service control policy (SCP) to deny creating instances when a tag key is not specified. Create another SCP that prevents identities from deleting tags. Attach the SCPs to the appropriate OU.

E. Create an AWS Config rule to check if EC2 instances use the data sensitivity tag and the specified values. Configure an AWS Lambda function to delete the resource if a noncompliant resource is found.

## Question #988 분석

### 문제 상황
- **모바일 앱 아키텍처** 설계
- **Organizations OU** 사용
- EC2 인스턴스에 **data sensitivity** 태그 필요 (sensitive/nonsensitive)
- **태그 삭제 금지** + **태그 없는 인스턴스 생성 금지**

### 핵심 요구사항
- **특정 태그 값 강제**
- **태그 삭제 방지**
- **태그 없는 생성 방지**

### Organizations 정책 타입

**Tag Policy**: 태그 표준화 및 강제
**SCP**: 권한 제한 및 거부

### 선택지 분석

**A. Tag Policy로 태그 값 강제**
- Tag Policy: 태그 키/값 표준화 전용 ✅
- 특정 값 강제 가능 ✅
- OU 수준 적용 ✅

**B. SCP로 태그 값 강제**
- SCP: 권한 거부 전용 ❌
- 태그 값 강제는 Tag Policy 역할 ❌

**C. Tag Policy로 삭제/생성 방지**
- Tag Policy: 표준화 전용, 권한 제어 불가 ❌

**D. SCP로 삭제/생성 방지** ✅
- SCP: 권한 거부에 적합 ✅
- 태그 삭제 액션 차단 가능 ✅
- 태그 없는 생성 차단 가능 ✅

**E. Config + Lambda**
- 사후 대응 방식 ❌
- 예방적 제어 아님 ❌

### 올바른 조합

**Tag Policy (A) + SCP (D)**
```yaml
Tag Policy:
- 태그 키: data-sensitivity
- 허용 값: sensitive, nonsensitive
- 표준화 강제

SCP:
- ec2:CreateTags 거부 (삭제 방지)
- 태그 없는 ec2:RunInstances 거부
```