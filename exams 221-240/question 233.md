## Question #233
A solutions architect has created a new AWS account and must secure AWS account root user access.

Which combination of actions will accomplish this? (Choose two.)

A. Ensure the root user uses a strong password.

B. Enable multi-factor authentication to the root user.

C. Store root user access keys in an encrypted Amazon S3 bucket.

D. Add the root user to a group containing administrative permissions.

E. Apply the required permissions to the root user with an inline policy document.

## Question #233 분석 (Choose Two)

### 요구사항
- **새 AWS 계정** 생성 완료
- **Root 사용자 접근** 보안 강화

### 선택지 분석

**A. Root 사용자 강력한 패스워드 사용** ⭐
- **기본 보안**: 복잡한 패스워드로 무차별 대입 공격 방지 ✅
- **필수 조치**: Root 계정 보안의 첫 번째 단계 ✅
- **AWS 권장사항**: 모든 계정에 적용되는 기본 보안 조치 ✅

**B. Root 사용자 MFA 활성화** ⭐
- **이중 인증**: 패스워드 외 추가 보안 계층 ✅
- **계정 탈취 방지**: 패스워드 유출 시에도 접근 차단 ✅
- **AWS 강력 권장**: Root 계정 보안의 핵심 조치 ✅

**C. 암호화된 S3에 액세스 키 저장**
- **보안 위험**: Root 액세스 키 생성 자체가 위험 ❌
- **모범 사례 위반**: Root 사용자는 액세스 키 사용 금지 ❌
- **불필요한 노출**: Root 자격증명을 파일로 저장 부적절 ❌

**D. 관리 권한 그룹에 Root 사용자 추가**
- **기술적 불가능**: Root 사용자는 IAM 그룹에 추가 불가 ❌
- **설계 오해**: Root는 이미 모든 권한 보유 ❌

**E. 인라인 정책으로 Root 권한 적용**
- **불필요한 조치**: Root는 이미 모든 권한 보유 ❌
- **설계 원칙 위반**: Root에 추가 정책 적용 불필요 ❌

### Root 사용자 보안 모범 사례

```yaml
필수 조치:
✅ 강력한 패스워드 설정
✅ MFA 활성화 (하드웨어/소프트웨어 토큰)
✅ Root 액세스 키 생성 금지
✅ 일상 업무용 사용 금지

권장 추가 조치:
- IAM 관리자 사용자 생성
- Root 사용자 사용 최소화
- CloudTrail로 Root 사용 모니터링
- 정기적 보안 검토
```
