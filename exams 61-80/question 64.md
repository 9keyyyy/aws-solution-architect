## Question #64
A company has more than 5 TB of file data on Windows file servers that run on premises. 
Users and applications interact with the data each day.
The company is moving its Windows workloads to AWS. 
As the company continues this process, the company requires access to AWS and on-premises file storage with minimum latency. 
The company needs a solution that minimizes operational overhead and requires no significant changes to the existing file access patterns. 
The company uses an AWS Site-to-Site VPN connection for connectivity to AWS.

What should a solutions architect do to meet these requirements?

A. Deploy and configure Amazon FSx for Windows File Server on AWS. Move the on-premises file data to FSx for Windows File Server. Reconfigure the workloads to use FSx for Windows File Server on AWS.

B. Deploy and configure an Amazon S3 File Gateway on premises. Move the on-premises file data to the S3 File Gateway. Reconfigure the on-premises workloads and the cloud workloads to use the S3 File Gateway.

C. Deploy and configure an Amazon S3 File Gateway on premises. Move the on-premises file data to Amazon S3. Reconfigure the workloads to use either Amazon S3 directly or the S3 File Gateway. depending on each workload's location.

D. Deploy and configure Amazon FSx for Windows File Server on AWS. Deploy and configure an Amazon FSx File Gateway on premises. Move the on-premises file data to the FSx File Gateway. Configure the cloud workloads to use FSx for Windows File Server on AWS. Configure the on-premises workloads to use the FSx File Gateway.

## Question #64 분석

### ✅ 요구사항
- **5TB 이상** Windows 파일 서버 데이터 (온프레미스)
- **일일 사용자/애플리케이션 상호작용**
- **Windows 워크로드 AWS 마이그레이션** 진행 중
- **최소 지연 시간**으로 AWS와 온프레미스 파일 접근
- **운영 오버헤드 최소화**
- **기존 파일 접근 패턴 변경 최소화**
- **Site-to-Site VPN** 연결 사용

### ✅ 선택지 분석

**A. FSx for Windows File Server로 완전 마이그레이션**
- **완전 클라우드 이전**: 모든 데이터를 AWS로 이동 ⚠️
- **온프레미스 접근**: VPN을 통한 원격 접근으로 지연 시간 증가 ❌
- **최소 지연 시간**: 온프레미스에서 AWS 접근 시 지연 발생 ❌
- **단일 위치**: 하이브리드 요구사항 미충족 ❌

**B. S3 File Gateway 온프레미스 + 모든 워크로드 재구성**
- **S3 File Gateway**: NFS/SMB 인터페이스 제공 ✅
- **하이브리드**: 온프레미스 캐시 + S3 백엔드 ✅
- **워크로드 재구성**: 클라우드 워크로드도 File Gateway 사용 ❌
- **비효율적**: AWS 워크로드가 온프레미스 경유 ❌
```
아키텍처:
  온프레미스: [사용자] → [S3 File Gateway] → [S3]
  AWS 워크로드: [EC2] → [VPN] → [온프레미스 S3 File Gateway] → [S3]

문제점:
  ❌ AWS 워크로드가 비효율적 경로
  ❌ AWS→온프레미스→S3로 돌아가는 구조
  ❌ VPN 대역폭 과부하
  ❌ 불필요한 네트워크 홉
```

**C. S3 File Gateway + 위치별 다른 접근 방식**
- **하이브리드 접근**: 위치에 따라 최적화된 접근 ✅
- **온프레미스**: File Gateway로 로컬 캐시 + 최소 지연 ✅
- **AWS 워크로드**: S3 직접 접근으로 최적 성능 ✅
- **Windows 호환**: SMB 프로토콜 지원 ✅
- **운영 최소화**: 관리형 서비스 활용 ✅
```
아키텍처:
  온프레미스: [사용자] → [S3 File Gateway] → [S3]
  AWS 워크로드: [EC2] → [S3] (직접 접근)

장점:
  ✅ 각 위치에서 최적 경로
  ✅ 온프레미스: 로컬 캐시 + SMB
  ✅ AWS: S3 API 직접 접근
  ✅ 네트워크 효율성
```

**D. FSx + FSx File Gateway 조합** ⭐
- **FSx for Windows**: AWS에서 네이티브 Windows 파일 서비스 ✅
- **FSx File Gateway**: 온프레미스 로컬 캐시 제공 ✅
- **최소 지연 시간**: 각 위치에서 로컬 접근 ✅
- **Windows 네이티브**: 완전한 Windows 파일 시스템 호환 ✅
- **기존 패턴 유지**: SMB 프로토콜 그대로 사용 ✅
```
아키텍처:
  온프레미스: [사용자] → [FSx File Gateway] → [FSx for Windows]
  AWS 워크로드: [EC2] → [FSx for Windows] (직접 접근)

장점:
  ✅ 완전한 Windows 호환성
  ✅ 네이티브 파일 시스템
  ✅ 기존 패턴 100% 유지
```

### 📋 핵심 개념 정리

#### **하이브리드 파일 스토리지 옵션**
```yaml
S3 File Gateway:
  ✅ NFS/SMB 인터페이스
  ✅ S3 백엔드 스토리지
  ✅ 로컬 캐시
  ⚠️ 객체 스토리지 기반

FSx File Gateway:
  ✅ SMB 인터페이스
  ✅ FSx for Windows 백엔드
  ✅ 로컬 캐시
  ✅ 완전한 파일 시스템 기능
```

#### **Windows 워크로드 최적화**
```yaml
FSx for Windows File Server:
  ✅ 네이티브 Windows 기능
  ✅ Active Directory 통합
  ✅ NTFS 권한 지원
  ✅ VSS 스냅샷 지원
  ✅ DFS 지원

S3:
  ✅ 무제한 확장성
  ✅ 비용 효율적
  ❌ 파일 시스템 기능 제한
  ❌ Windows 네이티브 기능 부족
```

#### **정답 시나리오 (D번)**
```yaml
1. AWS에 FSx for Windows File Server 배포
   - Multi-AZ 구성으로 고가용성
   - 5TB+ 스토리지 용량 설정
   
2. 온프레미스에 FSx File Gateway 배포
   - VMware 또는 Hyper-V에 설치
   - 로컬 캐시 스토리지 구성
   
3. 데이터 마이그레이션
   - FSx File Gateway를 통해 점진적 마이그레이션
   - 기존 파일 서버에서 FSx로 동기화
   
4. 워크로드 구성
   온프레미스:
   ✅ FSx File Gateway 접근 (로컬 성능)
   ✅ 자주 사용하는 파일 로컬 캐시
   ✅ 기존 SMB 공유 그대로 사용
   
   AWS 클라우드:
   ✅ FSx for Windows 직접 접근
   ✅ 네이티브 성능
   ✅ VPC 내 최적화된 연결

5. 이점
   최소 지연 시간:
   ✅ 온프레미스: 로컬 캐시 접근
   ✅ AWS: 직접 FSx 접근
   
   운영 최소화:
   ✅ 완전 관리형 서비스
   ✅ 자동 동기화
   ✅ 백업 자동화
   
   기존 패턴 유지:
   ✅ SMB 프로토콜 동일
   ✅ Windows 권한 모델 동일
   ✅ 파일 시스템 기능 완전 지원
```

#### **C번 vs D번 비교**
```yaml
C번 (S3 File Gateway):
  ✅ 비용 효율적
  ✅ 무제한 확장성
  ❌ 파일 시스템 기능 제한
  ❌ Windows 네이티브 기능 부족

D번 (FSx File Gateway):
  ✅ 완전한 Windows 호환성
  ✅ 네이티브 파일 시스템
  ✅ 기존 패턴 100% 유지
  💰 상대적으로 높은 비용
  ✅ Windows 워크로드에 최적화
```

