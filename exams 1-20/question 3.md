## Question #3
A company uses AWS Organizations to manage multiple AWS accounts for different departments. 
The management account has an Amazon S3 bucket that contains project reports. 
The company wants to limit access to this S3 bucket to only users of accounts within the organization in AWS Organizations.
Which solution meets these requirements with the LEAST amount of operational overhead?

A. Add the aws PrincipalOrgID global condition key with a reference to the organization ID to the S3 bucket policy. <br>
B. Create an organizational unit (OU) for each department. Add the aws:PrincipalOrgPaths global condition key to the S3 bucket policy. <br>
C. Use AWS CloudTrail to monitor the CreateAccount, InviteAccountToOrganization, LeaveOrganization, and RemoveAccountFromOrganization events. Update the S3 bucket policy accordingly. <br>
D. Tag each user that needs access to the S3 bucket. Add the aws:PrincipalTag global condition key to the S3 bucket policy.<br>

### ✅ 요구사항:
- AWS Organizations로 관리되는 여러 AWS 계정이 있음
- 관리 계정(Management Account)의 S3 버킷에 프로젝트 리포트 저장
- 조직 내 계정의 사용자만 S3 버킷 접근 허용
- 최소한의 운영 오버헤드로 구현

### ✅ 선택지 분석
**A. aws:PrincipalOrgID 조건 키 사용** ⭐
- 조직 ID 기반 단일 조건으로 모든 조직 내 계정 포함
- 계정 추가/제거 시 자동 적용 - 운영 오버헤드 최소
- 간단하고 직관적인 구현
- AWS Organizations의 네이티브 기능 활용

B. aws:PrincipalOrgPaths 조건 키 사용

- OU별 세분화된 제어이지만 문제에서 요구하지 않음
- 각 부서별 OU 생성 및 관리 필요 (운영 오버헤드 증가)
- OU 구조 변경 시 정책 업데이트 필요
- 과도한 복잡성

C. CloudTrail 이벤트 모니터링 사용

- 수동적인 접근법 - 이벤트 발생 후 대응
- 실시간 정책 업데이트를 위한 추가 자동화 필요
- 높은 운영 복잡성 및 지연 가능성
- 개발 및 유지보수 오버헤드 큼

D. 사용자 태깅 기반 접근

- 개별 사용자별 태깅 필요 (관리 복잡성 극대화)
- 사용자 추가/제거 시마다 수동 태깅 작업
- 확장성 부족 및 실수 발생 가능성 높음
- 조직 레벨 제어가 아닌 개별 관리

### ✅ 개념 정리
### 1. AWS Organizations 조건 키 
#### 1.1 aws:PrincipalOrgID
- 핵심 특징
    - 조직 전체를 대상으로 하는 글로벌 조건 키
    - 조직의 고유 ID (o-xxxxxxxxxx) 를 기준으로 접근 제어
    - 조직 내 모든 계정의 프린시패ল(사용자, 역할)에 자동 적용
- 작동 방식
  ```json
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Principal": "*",
        "Action": "s3:GetObject",
        "Resource": "arn:aws:s3:::bucket/*",
        "Condition": {
          "StringEquals": {
            "aws:PrincipalOrgID": "o-1234567890"
          }
        }
      }
    ]
  }
  ```
- 장점
    - 단일 조건으로 모든 조직 구성원 포함
    - 계정 추가/제거 시 자동 반영
    - 관리 오버헤드 최소화

#### 1.2 aws:PrincipalOrgPaths
- 핵심 특징
    - OU(Organizational Unit) 경로 기반 세분화된 접근 제어
    - 특정 OU나 OU 계층에 속한 계정만 접근 허용
    - OU 구조 설계 및 관리 필요, OU 변경 시 정책 업데이트 필요
    - 복잡한 조직 구조에서 부서별 권한 분리에 유용
- 경로 형식
  ```
  o-1234567890/r-abcd/ou-abcd-efghijkl/ou-abcd-mnopqrst/
  ```
- 사용 예시
  ```
  {
    "Condition": {
      "ForAllValues:StringEquals": {
        "aws:PrincipalOrgPaths": [
          "o-1234567890/r-abcd/ou-abcd-finance/",
          "o-1234567890/r-abcd/ou-abcd-hr/"
        ]
      }
    }
  }
#### 1.3 aws:PrincipalTag - 사용자 태깅 
- 특징
    - IAM 사용자나 역할에 태그 기반 접근 제어
    - 세분화된 권한 관리 가능
    - ABAC(Attribute-Based Access Control) 모델
- 한계
    - 수동 태깅 작업 필요
    - 사용자 수 증가 시 관리 복잡성 기하급수적 증가
    - 태그 누락이나 잘못된 태그로 인한 보안 위험
    - 확장성 제약

### 2. AWS CloudTrail 조직 이벤트
- 관련 이벤트
    - CreateAccount: 새 계정 생성
    - InviteAccountToOrganization: 기존 계정 초대
    - LeaveOrganization: 계정이 조직 탈퇴
    - RemoveAccountFromOrganization: 관리자가 계정 제거
- 이벤트 기반 자동화의 문제점
    - 비동기 처리: 이벤트 발생과 정책 업데이트 사이 지연
    - Lambda/Step Functions 등 추가 서비스 필요
    - 복잡한 오류 처리 및 재시도 로직 구현
    - 일관성 보장 어려움
 


