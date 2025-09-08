## Question #99
A company is implementing a shared storage solution for a gaming application that is hosted in an on-premises data center. 
The company needs the ability to use Lustre clients to access data. 
The solution must be fully managed.

Which solution meets these requirements?

A. Create an AWS Storage Gateway file gateway. Create a file share that uses the required client protocol. Connect the application server to the file share.

B. Create an Amazon EC2 Windows instance. Install and configure a Windows file share role on the instance. Connect the application server to the file share.

C. Create an Amazon Elastic File System (Amazon EFS) file system, and configure it to support Lustre. Attach the file system to the origin server. Connect the application server to the file system.

D. Create an Amazon FSx for Lustre file system. Attach the file system to the origin server. Connect the application server to the file system.

## Question #99 분석

### 요구사항
- **게임 애플리케이션** 공유 스토리지
- **Lustre 클라이언트** 접근 필요
- **완전 관리형** 솔루션

### 선택지 분석

**A. AWS Storage Gateway File Gateway**
- **Lustre 미지원**: NFS/SMB 프로토콜만 지원 ❌
- **용도 불일치**: 하이브리드 파일 동기화용 ❌

**B. EC2 Windows + 파일 공유**
- **완전 관리형 아님**: EC2 인스턴스 직접 관리 필요 ❌
- **Lustre 미지원**: Windows 파일 공유는 SMB 기반 ❌

**C. Amazon EFS + Lustre 구성**
- **기술적 불가능**: EFS는 NFS 기반, Lustre 지원 안함 ❌
- **프로토콜 불일치**: EFS와 Lustre는 다른 파일 시스템 ❌

**D. Amazon FSx for Lustre** ⭐
- **Lustre 전용**: 네이티브 Lustre 파일 시스템 ✅
- **완전 관리형**: AWS 완전 관리 서비스 ✅
- **고성능**: HPC/게임 워크로드 최적화 ✅
- **클라이언트 호환**: 표준 Lustre 클라이언트 지원 ✅

### Lustre 파일 시스템

```yaml
Lustre 특징:
- 고성능 병렬 파일 시스템
- HPC/게임 애플리케이션 최적화
- 높은 처리량과 낮은 지연시간
- 대용량 데이터 처리

FSx for Lustre:
✅ 완전 관리형 Lustre
✅ S3 통합 지원
✅ 자동 스케일링
✅ 백업 및 복구
```
