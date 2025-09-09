## Question #260
A company’s compliance team needs to move its file shares to AWS. 
The shares run on a Windows Server SMB file share. 
A self-managed on-premises Active Directory controls access to the files and folders.

The company wants to use Amazon FSx for Windows File Server as part of the solution. 
The company must ensure that the on-premises Active Directory groups restrict access to the FSx for Windows File Server SMB compliance shares, folders, and files after the move to AWS. 
The company has created an FSx for Windows File Server file system.

Which solution will meet these requirements?

A. Create an Active Directory Connector to connect to the Active Directory. Map the Active Directory groups to IAM groups to restrict access.

B. Assign a tag with a Restrict tag key and a Compliance tag value. Map the Active Directory groups to IAM groups to restrict access.

C. Create an IAM service-linked role that is linked directly to FSx for Windows File Server to restrict access.

D. Join the file system to the Active Directory to restrict access.

## Question #260 분석

### 요구사항
- **Windows Server SMB 파일 공유** → AWS 이전
- **온프레미스 Active Directory** 접근 제어 유지
- **FSx for Windows File Server** 사용
- **AD 그룹**이 FSx SMB 공유, 폴더, 파일 접근 제한

### 선택지 분석

**A. Active Directory Connector + IAM 그룹 매핑**
- **불필요한 복잡성**: AD Connector는 AWS 서비스 인증용, FSx 파일 권한과 별개 ❌
- **IAM 매핑 부적절**: 파일 레벨 권한은 NTFS ACL로 관리, IAM과 분리 ❌

**B. 태그 + IAM 그룹 매핑**
- **태그 무관**: 태그는 AWS 리소스 관리용, 파일 접근 제어와 무관 ❌
- **동일한 IAM 문제**: 파일 시스템 권한은 IAM이 아닌 AD로 관리 ❌

**C. IAM 서비스 링크 역할**
- **용도 불일치**: 서비스 링크 역할은 AWS 서비스 간 권한용 ❌
- **파일 권한 무관**: 사용자/그룹 파일 접근과 별개 ❌

**D. 파일 시스템을 Active Directory에 조인** ⭐
- **네이티브 통합**: FSx가 온프레미스 AD에 직접 조인 ✅
- **기존 권한 유지**: AD 사용자/그룹 권한이 그대로 적용 ✅
- **NTFS ACL**: Windows 파일 권한 모델 완전 지원 ✅
- **SSO**: 도메인 사용자가 자동 인증으로 접근 ✅

### FSx Active Directory 조인

```yaml
설정 과정:
1. FSx 파일 시스템을 온프레미스 AD 도메인에 조인
2. 기존 AD 사용자/그룹 권한이 자동 적용
3. NTFS ACL을 통한 세밀한 파일/폴더 권한 제어
4. Kerberos 인증을 통한 SSO

결과:
- 기존 권한 구조 그대로 유지
- 추가 매핑 작업 불필요
- Windows 네이티브 보안 모델 활용
```
