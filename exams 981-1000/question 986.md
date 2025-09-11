## Question #986
A company is testing an application that runs on an Amazon EC2 Linux instance. A single 500 GB Amazon Elastic Block Store (Amazon EBS) General Purpose SSO (gp2) volume is attached to the EC2 instance.

The company will deploy the application on multiple EC2 instances in an Auto Scaling group. All instances require access to the data that is stored in the EBS volume. The company needs a highly available and resilient solution that does not introduce significant changes to the application's code.

Which solution will meet these requirements?

A. Provision an EC2 instance that uses NFS server software. Attach a single 500 GB gp2 EBS volume to the instance.

B. Provision an Amazon FSx for Windows File Server file system. Configure the file system as an SMB file store within a single Availability Zone.

C. Provision an EC2 instance with two 250 GB Provisioned IOPS SSD EBS volumes.

D. Provision an Amazon Elastic File System (Amazon EFS) file system. Configure the file system to use General Purpose performance mode.

## Question #986 분석

### 문제 상황
- **단일 EC2** + **500GB gp2 EBS** 볼륨
- **Auto Scaling Group**으로 확장 예정
- **모든 인스턴스가 동일 데이터 접근** 필요
- **고가용성** + **애플리케이션 코드 변경 최소화**

### 핵심 요구사항
- **다중 인스턴스 공유 스토리지**
- **고가용성**
- **코드 변경 최소화**

### EBS 제약사항
**EBS는 단일 인스턴스에만 연결 가능** → Auto Scaling에 부적합

### 선택지 분석

**A. NFS 서버 EC2 + EBS**
- 단일 장애점 (EC2 인스턴스) ❌
- 고가용성 부족 ❌

**B. FSx for Windows File Server**
- Windows 전용 (문제는 Linux) ❌
- SMB 프로토콜 → Linux에 부적합 ❌

**C. 2개 250GB Provisioned IOPS**
- 여전히 EBS → 단일 인스턴스만 연결 ❌
- 공유 스토리지 해결 안됨 ❌

**D. Amazon EFS** ✅
- Linux 네이티브 NFS 지원 ✅
- 다중 인스턴스 동시 마운트 ✅
- Multi-AZ 고가용성 ✅
- 코드 변경 최소화 (파일 경로만 변경) ✅

### EFS vs EBS 비교

| 구분 | EBS | EFS |
|------|-----|-----|
| **연결** | 단일 인스턴스 | 다중 인스턴스 |
| **프로토콜** | 블록 스토리지 | NFS |
| **고가용성** | 단일 AZ | Multi-AZ |
| **Auto Scaling** | 부적합 | 적합 |

