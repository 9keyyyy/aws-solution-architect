## Question #249
A company is implementing a shared storage solution for a media application that is hosted in the AWS Cloud. 
The company needs the ability to use SMB clients to access data. 
The solution must be fully managed.

Which AWS solution meets these requirements?

A. Create an AWS Storage Gateway volume gateway. Create a file share that uses the required client protocol. Connect the application server to the file share.

B. Create an AWS Storage Gateway tape gateway. Configure tapes to use Amazon S3. Connect the application server to the tape gateway.

C. Create an Amazon EC2 Windows instance. Install and configure a Windows file share role on the instance. Connect the application server to the file share.

D. Create an Amazon FSx for Windows File Server file system. Attach the file system to the origin server. Connect the application server to the file system.

## Question #249 분석

### 요구사항
- **미디어 애플리케이션** 공유 스토리지
- **SMB 클라이언트** 접근 필요
- **완전 관리형** 솔루션

### 선택지 분석

**A. Storage Gateway Volume Gateway + 파일 공유**
- **용도 불일치**: Volume Gateway는 블록 스토리지용(iSCSI), SMB 파일 공유 아님 ❌
- **프로토콜 미스매치**: SMB 프로토콜 지원하지 않음 ❌

**B. Storage Gateway Tape Gateway + S3 테이프**
- **백업 전용**: Tape Gateway는 백업/아카이브용, 활성 파일 공유 아님 ❌
- **SMB 지원 없음**: 테이프 기반으로 SMB 접근 불가 ❌

**C. EC2 Windows + 파일 공유 역할**
- **관리형 아님**: EC2 인스턴스 직접 관리 필요 ❌
- **운영 부담**: Windows 서버, 패치, 백업 등 수동 관리 ❌
- **요구사항 위반**: "완전 관리형" 조건 불충족 ❌

**D. Amazon FSx for Windows File Server** ⭐
- **SMB 네이티브 지원**: Windows 파일 시스템으로 SMB 완벽 지원 ✅
- **완전 관리형**: AWS가 패치, 백업, 고가용성 자동 관리 ✅
- **미디어 애플리케이션 최적**: 높은 처리량과 IOPS 제공 ✅
- **AD 통합**: Active Directory 인증 지원 ✅

### Amazon FSx for Windows File Server

```yaml
주요 특징:
- SMB 2.1, 3.0, 3.1.1 프로토콜 지원
- Windows 기반 애플리케이션과 완벽 호환
- 완전 관리형 서비스
- 고성능 (수 GB/s 처리량)

미디어 워크플로우 최적화:
- 대용량 파일 처리
- 낮은 지연시간
- 높은 IOPS
- 동시 접근 지원
```
