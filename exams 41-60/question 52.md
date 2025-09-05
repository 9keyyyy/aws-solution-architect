## Question #52
A company wants to migrate its on-premises application to AWS. 
The application produces output files that vary in size from tens of gigabytes to hundreds of terabytes. 
The application data must be stored in a standard file system structure. 
The company wants a solution that scales automatically. is highly available, and requires minimum operational overhead.

Which solution will meet these requirements?

A. Migrate the application to run as containers on Amazon Elastic Container Service (Amazon ECS). Use Amazon S3 for storage.

B. Migrate the application to run as containers on Amazon Elastic Kubernetes Service (Amazon EKS). Use Amazon Elastic Block Store (Amazon EBS) for storage.

C. Migrate the application to Amazon EC2 instances in a Multi-AZ Auto Scaling group. Use Amazon Elastic File System (Amazon EFS) for storage.

D. Migrate the application to Amazon EC2 instances in a Multi-AZ Auto Scaling group. Use Amazon Elastic Block Store (Amazon EBS) for storage.

## Question #52 분석

### ✅ 요구사항
- **대용량 파일**: 수십 GB ~ 수백 TB 규모
- **표준 파일 시스템 구조** 필요
- **자동 확장** (Auto Scaling)
- **고가용성** (High Availability)
- **최소 운영 오버헤드**

### ✅ 선택지 분석

**A. ECS + S3**
- **S3**: 객체 스토리지로 **표준 파일 시스템 구조 미지원** ❌
- 파일 시스템 계층 구조 불가능

**B. EKS + EBS**
- **EBS**: 단일 인스턴스 연결, **공유 스토리지 불가** ❌
- 컨테이너 간 데이터 공유 제한
- **과도한 Kubernetes 복잡성**

**C. EC2 Multi-AZ Auto Scaling + EFS** ✅
- **EFS**: 완전관리형 **NFS 파일 시스템** ✅
- **표준 POSIX 파일 시스템**: 계층적 디렉터리 구조 ✅
- **자동 확장**: 페타바이트 규모까지 자동 스케일링 ✅
- **Multi-AZ**: 여러 가용 영역 동시 마운트 가능 ✅
- **최소 운영 오버헤드**: 완전관리형 서비스 ✅

**D. EC2 Multi-AZ Auto Scaling + EBS**
- **EBS**: 인스턴스당 개별 볼륨, **공유 불가** ❌
- 데이터 일관성 문제
- 스케일링 시 데이터 이전 복잡성

### 📋 핵심 차별화 요소

#### **스토리지 비교**
```yaml
파일 시스템 요구사항:
  EFS: ✅ POSIX 호환 파일 시스템
  EBS: ❌ 블록 스토리지 (공유 불가)
  S3:  ❌ 객체 스토리지 (파일 시스템 X)

확장성:
  EFS: ✅ 자동 확장 (페타바이트급)
  EBS: ❌ 수동 확장 필요
  S3:  ✅ 자동 확장 (하지만 파일 시스템 X)
```

#### **솔루션 아키텍처 (C번)**
```yaml
Multi-AZ Auto Scaling Group:
  ✅ 인스턴스 자동 확장/축소
  ✅ 가용 영역별 고가용성
  ✅ 로드 분산

Amazon EFS:
  ✅ 모든 인스턴스가 동일 파일 시스템 공유
  ✅ 수백 TB까지 자동 확장
  ✅ 표준 파일 시스템 인터페이스
  ✅ 완전관리형 (운영 오버헤드 최소)
```

