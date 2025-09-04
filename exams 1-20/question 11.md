## Question #11
A company has an application that runs on Amazon EC2 instances and uses an Amazon Aurora database. 
The EC2 instances connect to the database by using user names and passwords that are stored locally in a file. 
The company wants to minimize the operational overhead of credential management.

What should a solutions architect do to accomplish this goal?

A. Use AWS Secrets Manager. Turn on automatic rotation.

B. Use AWS Systems Manager Parameter Store. Turn on automatic rotation.

C. Create an Amazon S3 bucket to store objects that are encrypted with an AWS Key Management Service (AWS KMS) encryption key. Migrate the credential file to the S3 bucket. Point the application to the S3 bucket.

D. Create an encrypted Amazon Elastic Block Store (Amazon EBS) volume for each EC2 instance. Attach the new EBS volume to each EC2 instance. Migrate the credential file to the new EBS volume. Point the application to the new EBS volume.

### ✅ 요구사항
- EC2 인스턴스가 Aurora 데이터베이스에 연결
- 사용자명과 비밀번호가 로컬 파일에 저장됨
- credential 관리의 운영 오버헤드 최소화 필요

### ✅ 선택지 분석
**A. AWS Secrets Manager + 자동 로테이션** ⭐
- 중앙화된 credential 관리
- 자동 로테이션으로 보안 강화
- Aurora와 네이티브 통합 지원
- 운영 오버헤드 최소화
- EC2에서 API 호출로 credential 검색 가능

**B. AWS Systems Manager Parameter Store + 자동 로테이션**
- Parameter Store는 자동 로테이션 기능이 없음
- Parameter Store는 주로 설정값 저장용
- credential 관리에는 부적합

**C. S3 + KMS 암호화**
- 여전히 수동 credential 관리 필요
- 로테이션 자동화 불가
- 운영 오버헤드 감소 효과 제한적

**D. 암호화된 EBS 볼륨**
- 여전히 로컬 파일 기반 저장
- 수동 credential 관리
- 근본적인 문제 해결 안됨

### ✅ 개념 정리
### 1. AWS Secrets Manager
#### 1.1 핵심 기능
- **자동 로테이션**
  - 정기적인 비밀번호 변경
  - Aurora, RDS와 완전 통합
  - 다운타임 없는 로테이션
- **중앙 집중식 관리**
  - 모든 credential을 한 곳에서 관리
  - 세밀한 접근 제어 (IAM 정책)
  - 감사 로그 및 모니터링

#### 1.2 Aurora와의 통합
```
1. Secrets Manager가 새 비밀번호 생성
2. Aurora에 새 credential로 테스트 연결
3. 성공하면 이전 credential 무효화
4. 애플리케이션은 항상 최신 credential 사용
```


### 2. Parameter Store vs Secrets Manager
#### 2.1 Parameter Store 한계
- **자동 로테이션 없음**
  - 수동으로 값을 업데이트해야 함
  - 애플리케이션 재시작 필요할 수 있음
- **용도**
  - 설정 매개변수 저장
  - 환경 변수 관리
  - 비밀 정보보다는 구성 정보에 적합

#### 2.2 Secrets Manager 장점
- **완전 자동화**
  - 로테이션 일정 설정 가능
  - 애플리케이션 코드 변경 불필요
  - 다운타임 없는 업데이트
- **보안**
  - 전송 및 저장 시 암호화
  - 세분화된 접근 제어
  - 감사 추적 기능

### 3. 운영 오버헤드 비교
#### 3.1 현재 방식 (로컬 파일)
```
- 각 인스턴스마다 파일 관리
- 수동 비밀번호 업데이트
- 보안 위험 (파일 노출)
- 확장성 부족
```

#### 3.2 Secrets Manager 방식
```
- 중앙 집중식 관리
- 자동 로테이션
- API 기반 접근
- 확장성 및 보안성 확보
```