## Question #227
A company needs to retain its AWS CloudTrail logs for 3 years. The company is enforcing CloudTrail across a set of AWS accounts by using AWS Organizations from the parent account. The CloudTrail target S3 bucket is configured with S3 Versioning enabled. An S3 Lifecycle policy is in place to delete current objects after 3 years.

After the fourth year of use of the S3 bucket, the S3 bucket metrics show that the number of objects has continued to rise. However, the number of new CloudTrail logs that are delivered to the S3 bucket has remained consistent.

Which solution will delete objects that are older than 3 years in the MOST cost-effective manner?

A. Configure the organization’s centralized CloudTrail trail to expire objects after 3 years.

B. Configure the S3 Lifecycle policy to delete previous versions as well as current versions.

C. Create an AWS Lambda function to enumerate and delete objects from Amazon S3 that are older than 3 years.

D. Configure the parent account as the owner of all objects that are delivered to the S3 bucket.

## Question #227 분석

### 요구사항
- **CloudTrail 로그 3년 보존**
- **S3 Versioning 활성화**
- **Lifecycle 정책**: 현재 객체만 3년 후 삭제
- **문제**: 객체 수 지속 증가 (이전 버전 누적)

### 문제 원인
```yaml
S3 Versioning + Lifecycle 정책:
- 현재 버전만 삭제됨
- 이전 버전들은 계속 누적
- 4년차에 1년치 이전 버전 잔존
```

### 선택지 분석

**A. CloudTrail에서 객체 만료 설정**
- **기능 없음**: CloudTrail은 S3 객체 수명주기 제어 기능 없음 ❌
- **역할 혼동**: CloudTrail은 로그 전송만 담당 ❌

**B. Lifecycle 정책에 이전 버전 삭제 추가** ⭐
- **근본 해결**: 현재 버전과 이전 버전 모두 관리 ✅
- **비용 효율적**: 자동화된 정책으로 관리 비용 없음 ✅
- **정확한 보존**: 3년 정책 정확히 적용 ✅

**C. Lambda 함수로 객체 열거 및 삭제**
- **운영 부담**: 함수 개발 및 유지보수 필요 ❌
- **비용 증가**: Lambda 실행 비용 지속 발생 ❌
- **복잡성**: 불필요한 커스텀 솔루션 ❌

**D. 부모 계정을 객체 소유자로 설정**
- **문제 해결 무관**: 객체 소유권은 삭제와 별개 ❌
- **기존 객체**: 이미 생성된 객체 소유권 변경 복잡 ❌

