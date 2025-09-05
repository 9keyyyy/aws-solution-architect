## Question #54
A company runs multiple Windows workloads on AWS. 
The company's employees use Windows file shares that are hosted on two Amazon EC2 instances. 
The file shares synchronize data between themselves and maintain duplicate copies. 
The company wants a highly available and durable storage solution that preserves how users currently access the files.

What should a solutions architect do to meet these requirements?

A. Migrate all the data to Amazon S3. Set up IAM authentication for users to access files.

B. Set up an Amazon S3 File Gateway. Mount the S3 File Gateway on the existing EC2 instances.

C. Extend the file share environment to Amazon FSx for Windows File Server with a Multi-AZ configuration. Migrate all the data to FSx for Windows File Server.

D. Extend the file share environment to Amazon Elastic File System (Amazon EFS) with a Multi-AZ configuration. Migrate all the data to Amazon EFS.

## Question #54 분석

### ✅ 요구사항
- **Windows 워크로드** 환경
- **Windows 파일 공유** 사용 중
- **현재 접근 방식 유지** (기존 사용자 경험)
- **고가용성 및 내구성**
- EC2 인스턴스 간 **데이터 동기화 및 중복 복사** 현재 운영

### ✅ 선택지 분석

**A. 모든 데이터를 S3로 마이그레이션 + IAM 인증**
- **접근 방식 변경**: 기존 Windows 파일 공유 → S3 API ❌
- **사용자 경험 파괴**: 파일 탐색기 직접 접근 불가 ❌
- **IAM 인증**: Windows 도메인 인증과 호환성 부족 ❌

**B. S3 File Gateway 설정 + 기존 EC2 마운트**
- **NFS/SMB 지원**: Windows 파일 공유와 호환 가능 ✅
- **하이브리드 접근**: 로컬 캐시 + S3 백엔드 ✅
- **기존 접근 방식 유지**: 파일 탐색기 사용 가능 ✅
- **고가용성**: Multi-AZ S3 백엔드 ✅
- **운영 복잡성**: 여전히 EC2 인스턴스 관리 필요 ⚠️
- **캐시 관리**: 로컬 캐시 용량 및 성능 고려 필요 ⚠️

**C. Amazon FSx for Windows File Server (Multi-AZ)** ⭐
- **네이티브 Windows 지원**: SMB 프로토콜, AD 통합 ✅
- **기존 접근 방식 완벽 유지**: Windows 파일 공유 동일 ✅
- **Multi-AZ**: 고가용성 보장 ✅
- **관리형 서비스**: 운영 오버헤드 최소화 ✅
- **완전한 마이그레이션**: EC2 인스턴스 관리 불필요 ✅

**D. Amazon EFS (Multi-AZ) + 데이터 마이그레이션**
- **Linux/Unix 최적화**: Windows 워크로드에 부적합 ❌
- **NFS 프로토콜**: Windows 네이티브 SMB와 다름 ❌
- **Windows 통합 부족**: AD 인증, NTFS 권한 미지원 ❌

### 📋 B번 vs C번 상세 비교

### **S3 File Gateway (B번)**
```yaml
장점:
  ✅ 하이브리드 스토리지 (온프레미스 + 클라우드)
  ✅ 무제한 S3 백엔드 스토리지
  ✅ 기존 EC2 인스턴스 활용 가능
  ✅ 로컬 캐시로 성능 향상

단점:
  ⚠️ EC2 인스턴스 지속 관리 필요
  ⚠️ 캐시 용량 제한
  ⚠️ 네트워크 지연 시 성능 저하
  ⚠️ 복잡한 하이브리드 아키텍처
```

### **FSx for Windows (C번)**
```yaml
장점:
  ✅ 완전관리형 서비스 (No EC2 관리)
  ✅ 네이티브 Windows 성능
  ✅ 자동 백업 및 패치
  ✅ 일관된 성능 보장
  ✅ 간단한 클라우드 네이티브 아키텍처

단점:
  💰 비용이 상대적으로 높을 수 있음
```

### 📋 핵심 개념 정리

### **Windows 파일 공유 요구사항**
```yaml
프로토콜: SMB (Server Message Block)
인증: Windows Active Directory
권한: NTFS 파일 시스템 권한
접근: Windows 파일 탐색기 직접 마운트
```

### **AWS 스토리지 솔루션 비교**
```yaml
FSx for Windows:
  ✅ 완전관리형 Windows 파일 시스템
  ✅ SMB 프로토콜 네이티브 지원
  ✅ Active Directory 통합
  ✅ NTFS 권한 지원
  ✅ Multi-AZ 고가용성

S3 File Gateway:
  ✅ 하이브리드 클라우드 스토리지
  ✅ SMB/NFS 지원
  ✅ 로컬 캐시 + S3 백엔드
  ⚠️ 하이브리드 아키텍처 복잡성

EFS:
  ❌ Linux/Unix 환경 최적화
  ❌ NFS만 지원 (SMB 미지원)
  ❌ Windows AD 통합 부족
```

### 📋 솔루션 아키텍처 (C번)

### **FSx for Windows File Server**
```yaml
완전관리형:
  ✅ 백업, 패치, 모니터링 자동화
  ✅ 기존 EC2 동기화 복잡성 완전 제거

Multi-AZ 구성:
  ✅ 자동 장애 조치
  ✅ 99.99% 가용성 SLA
  ✅ 데이터 중복성 자동 관리

Windows 네이티브:
  ✅ 기존 사용자 경험 100% 유지
  ✅ 파일 탐색기 직접 접근
  ✅ Windows 보안 모델 지원
```
