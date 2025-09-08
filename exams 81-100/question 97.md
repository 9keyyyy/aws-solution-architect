## Question #97
A company has a large Microsoft SharePoint deployment running on-premises that requires Microsoft Windows shared file storage. 
The company wants to migrate this workload to the AWS Cloud and is considering various storage options. 
The storage solution must be highly available and integrated with Active Directory for access control.

Which solution will satisfy these requirements?

A. Configure Amazon EFS storage and set the Active Directory domain for authentication.

B. Create an SMB file share on an AWS Storage Gateway file gateway in two Availability Zones.

C. Create an Amazon S3 bucket and configure Microsoft Windows Server to mount it as a volume.

D. Create an Amazon FSx for Windows File Server file system on AWS and set the Active Directory domain for authentication.

## Question #97 분석

### 요구사항
- **Microsoft SharePoint** 온프레미스 마이그레이션
- **Windows 공유 파일 저장소** 필요
- **고가용성** 요구
- **Active Directory 통합** 인증 필요

### 선택지 분석

**A. Amazon EFS + Active Directory**
- **Linux 중심**: EFS는 주로 Linux/POSIX 환경용 ❌
- **Windows 호환성**: SMB 프로토콜 네이티브 지원 부족 ❌
- **AD 통합**: 제한적 Windows 인증 지원 ❌

**B. Storage Gateway File Gateway (SMB)**
- **하이브리드 솔루션**: 온프레미스-클라우드 연결 용도 ❌
- **SharePoint 마이그레이션**: 완전 클라우드 마이그레이션에 부적합 ❌
- **복잡성**: 불필요한 하이브리드 아키텍처 ❌

**C. S3 + Windows 볼륨 마운트**
- **객체 스토리지**: 파일 시스템이 아닌 객체 저장소 ❌
- **SharePoint 호환성**: SharePoint는 파일 시스템 필요 ❌
- **AD 통합**: S3는 Windows AD 인증 미지원 ❌

**D. Amazon FSx for Windows File Server + AD** ⭐
- **Windows 네이티브**: 완전 관리형 Windows 파일 시스템 ✅
- **SMB 프로토콜**: SharePoint 요구사항 완벽 지원 ✅
- **AD 통합**: 네이티브 Active Directory 인증 ✅
- **고가용성**: Multi-AZ 배포 지원 ✅
- **SharePoint 최적화**: Microsoft 워크로드 전용 설계 ✅

### FSx for Windows 특징

```yaml
SharePoint 지원:
✅ SMB 2.0/3.0 프로토콜
✅ Windows NTFS 파일 시스템
✅ DFS 네임스페이스 지원
✅ 완전 관리형 백업

Active Directory:
✅ 도메인 조인 지원
✅ 사용자/그룹 기반 권한
✅ 기존 AD 정책 적용
✅ Kerberos 인증

고가용성:
✅ Multi-AZ 자동 장애조치
✅ 자동 백업 및 복원
✅ 99.9% 가용성 SLA
```
