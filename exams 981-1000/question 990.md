## Question #990
A company is planning to migrate a legacy application to AWS. The application currently uses NFS to communicate to an on-premises storage solution to store application data. The application cannot be modified to use any other communication protocols other than NFS for this purpose.

Which storage solution should a solutions architect recommend for use after the migration?

A. AWS DataSync

B. Amazon Elastic Block Store (Amazon EBS)

C. Amazon Elastic File System (Amazon EFS)

D. Amazon EMR File System (Amazon EMRFS)

## Question #990 분석

### 문제 상황
- **레거시 애플리케이션** AWS 마이그레이션
- 현재 **NFS 프로토콜** 사용
- **애플리케이션 수정 불가** (NFS만 사용 가능)

### 핵심 요구사항
- **NFS 프로토콜 지원**
- **기존 애플리케이션 호환성**

### AWS 스토리지 프로토콜 지원

| 스토리지 | 프로토콜 | 타입 |
|----------|----------|------|
| **EFS** | NFS | 파일 시스템 |
| **EBS** | iSCSI/블록 | 블록 스토리지 |
| **S3** | REST API | 객체 스토리지 |
| **FSx** | SMB/NFS | 파일 시스템 |

### 선택지 분석

**A. AWS DataSync**
- 데이터 마이그레이션 도구 ❌
- 스토리지 솔루션 아님 ❌

**B. Amazon EBS**
- 블록 스토리지 (iSCSI) ❌
- NFS 프로토콜 지원 안함 ❌

**C. Amazon EFS** ⭐
- NFS 프로토콜 네이티브 지원 ✅
- 다중 EC2 동시 마운트 ✅
- 완전 관리형 파일 시스템 ✅

**D. Amazon EMRFS**
- EMR 전용 S3 인터페이스 ❌
- 일반 애플리케이션용 아님 ❌

### EFS 특징

```yaml
프로토콜: NFS v4.1
확장성: 페타바이트급 자동 확장
가용성: Multi-AZ 기본 지원
성능: 범용/최대 I/O 모드
암호화: 전송 중/저장 시 암호화
```

### 마이그레이션 시나리오

```yaml
Before:
애플리케이션 → NFS → 온프레미스 스토리지

After:
애플리케이션 → NFS → Amazon EFS
```
