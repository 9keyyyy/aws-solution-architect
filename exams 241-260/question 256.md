## Question #256
A solutions architect is implementing a document review application using an Amazon S3 bucket for storage. 
The solution must prevent accidental deletion of the documents and ensure that all versions of the documents are available. 
Users must be able to download, modify, and upload documents.

Which combination of actions should be taken to meet these requirements? (Choose two.)

A. Enable a read-only bucket ACL.

B. Enable versioning on the bucket.

C. Attach an IAM policy to the bucket.

D. Enable MFA Delete on the bucket.

E. Encrypt the bucket using AWS KMS.

## Question #256 분석 (Choose Two)

### 요구사항
- **문서 검토 애플리케이션** S3 스토리지
- **실수로 인한 삭제 방지**
- **모든 문서 버전 사용 가능**
- **다운로드, 수정, 업로드** 기능 필요

### 선택지 분석

**A. 읽기 전용 버킷 ACL**
- **기능 제한**: 수정 및 업로드 불가능하게 됨 ❌
- **요구사항 위반**: 사용자가 문서 수정/업로드 불가 ❌

**B. 버킷에서 버전 관리 활성화** ⭐
- **모든 버전 보존**: 문서의 모든 버전 자동 보관 ✅
- **삭제 방지**: 삭제 시 삭제 마커 추가, 실제 데이터는 보존 ✅
- **버전 복구**: 이전 버전으로 언제든 복원 가능 ✅

**C. 버킷에 IAM 정책 연결**
- **권한 관리**: 접근 제어는 가능하지만 삭제 방지 효과 제한적 ❌
- **버전 관리 없음**: 문서 버전 보존 기능 없음 ❌

**D. 버킷에서 MFA Delete 활성화** ⭐
- **삭제 보호 강화**: 버전 삭제 시 MFA 인증 필수 ✅
- **실수 방지**: 의도하지 않은 삭제 방지 ✅
- **Versioning 필요**: 버전 관리와 함께 사용 시 효과적 ✅

**E. AWS KMS로 버킷 암호화**
- **보안 강화**: 데이터 암호화로 보안 향상 ⚡
- **삭제 방지 무관**: 실수 삭제 방지와 직접 관련 없음 ❌

### 조합 효과 (B + D)

```yaml
S3 Versioning + MFA Delete:
- 문서 업로드 시: 새 버전 자동 생성
- 문서 "삭제" 시: 삭제 마커만 추가, 데이터 보존
- 버전 영구 삭제: MFA 인증 필요로 보호
- 모든 버전 접근: 언제든 이전 버전 다운로드 가능

사용자 기능:
✅ 다운로드: 현재 및 이전 버전 모두 가능
✅ 수정: 새 버전으로 업로드
✅ 업로드: 정상 작동
✅ 삭제 방지: 실수로 영구 삭제 불가
```
