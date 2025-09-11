## Question #981
A company is building a cloud-based application on AWS that will handle sensitive customer data. The application uses Amazon RDS for the database, Amazon S3 for object storage, and S3 Event Notifications that invoke AWS Lambda for serverless processing.

The company uses AWS IAM Identity Center to manage user credentials. The development, testing, and operations teams need secure access to Amazon RDS and Amazon S3 while ensuring the confidentiality of sensitive customer data. The solution must comply with the principle of least privilege.

Which solution meets these requirements with the LEAST operational overhead?

A. Use IAM roles with least privilege to grant all the teams access. Assign IAM roles to each team with customized IAM policies defining specific permission for Amazon RDS and S3 object access based on team responsibilities.

B. Enable IAM Identity Center with an Identity Center directory. Create and configure permission sets with granular access to Amazon RDS and Amazon S3. Assign all the teams to groups that have specific access with the permission sets.

C. Create individual IAM users for each member in all the teams with role-based permissions. Assign the IAM roles with predefined policies for RDS and S3 access to each user based on user needs. Implement IAM Access Analyzer for periodic credential evaluation.

D. Use AWS Organizations to create separate accounts for each team. Implement cross-account IAM roles with least privilege. Grant specific permission for RDS and S3 access based on team roles and responsibilities.

## Question #981 분석

### 문제 상황
- **민감한 고객 데이터** 처리 애플리케이션
- **RDS + S3 + Lambda** 아키텍처
- **IAM Identity Center** 사용 중
- **개발/테스트/운영팀** 접근 필요
- **최소 권한 원칙** + **최소 운영 오버헤드**

### 핵심 요구사항
- **팀별 세분화된 접근 제어**
- **최소 권한 원칙**
- **최소 운영 부담**

### 선택지 분석

**A. IAM Role + 개별 정책**
- 팀별 개별 정책 생성 → 관리 복잡성 증가 ❌
- 운영 오버헤드 높음 ❌

**B. IAM Identity Center + Permission Sets** ⭐
- 중앙화된 사용자 관리 ✅
- Permission Sets으로 표준화된 권한 ✅
- 그룹 기반 관리 → 운영 효율성 ✅
- 기존 Identity Center 활용 ✅

**C. 개별 IAM 사용자**
- 사용자별 개별 관리 → 운영 부담 최대 ❌
- 확장성 부족 ❌

**D. 별도 계정 분리**
- 과도한 격리 → 협업 어려움 ❌
- 크로스 계정 역할 관리 복잡성 ❌

### IAM Identity Center vs 기타 방식

**Identity Center 장점**
```yaml
중앙 관리:
- 단일 로그인 포털
- 표준화된 Permission Sets
- 그룹 기반 권한 할당

운영 효율성:
- 신규 사용자 추가 간단
- 권한 변경 중앙에서 관리
- 감사 추적 용이
```

### Permission Sets 활용

```yaml
예시 구조:
- Dev-RDS-ReadWrite: 개발팀 RDS 권한
- Test-S3-ReadOnly: 테스트팀 S3 읽기 권한
- Ops-Full-Access: 운영팀 전체 권한

그룹 할당:
- Development-Team → Dev Permission Sets
- Testing-Team → Test Permission Sets
- Operations-Team → Ops Permission Sets
```

