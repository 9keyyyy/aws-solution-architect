## Question #37
A company recently launched a variety of new workloads on Amazon EC2 instances in its AWS account. 
The company needs to create a strategy to access and administer the instances remotely and securely. 
The company needs to implement a repeatable process that works with native AWS services and follows the AWS Well-Architected Framework.

Which solution will meet these requirements with the LEAST operational overhead?

A. Use the EC2 serial console to directly access the terminal interface of each instance for administration.

B. Attach the appropriate IAM role to each existing instance and new instance. Use AWS Systems Manager Session Manager to establish a remote SSH session.

C. Create an administrative SSH key pair. Load the public key into each EC2 instance. Deploy a bastion host in a public subnet to provide a tunnel for administration of each instance.

D. Establish an AWS Site-to-Site VPN connection. Instruct administrators to use their local on-premises machines to connect directly to the instances by using SSH keys across the VPN tunnel.

## Question #37 분析

### ✅ 요구사항
- **다양한 EC2 인스턴스** 원격 접근 및 관리
- **안전하고 반복 가능한** 프로세스
- **AWS 네이티브 서비스** 사용
- **AWS Well-Architected Framework** 준수
- **최소 운영 오버헤드**

### ✅ 선택지 분석

**A. EC2 Serial Console**
- **제한적 접근**: 콘솔만 사용 가능 
- **확장성 부족**: 다수 인스턴스 관리 어려움 
- **보안 한계**: 세밀한 접근 제어 불가
- Well-Architected 원칙 위배

**B. IAM 역할 + Systems Manager Session Manager** ⭐
- **AWS 네이티브**: SSM은 완전 관리형 서비스 ✅
- **IAM 통합**: 세밀한 권한 제어 ✅
- **SSH 키 불필요**: 키 관리 오버헤드 제거 ✅
- **반복 가능**: 표준화된 접근 방식 ✅
- **최소 운영**: 추가 인프라 불필요 ✅

**C. Bastion Host + SSH 키**
- **추가 인프라**: Bastion 서버 관리 필요 
- **SSH 키 관리**: 복잡한 키 배포 및 순환 
- **운영 오버헤드**: 서버 패치, 보안 관리 
- **단일 장애점**: Bastion 서버 의존성

**D. Site-to-Site VPN + SSH**
- **복잡한 네트워킹**: VPN 설정 및 관리 
- **SSH 키 관리**: 키 배포 복잡성 
- **온프레미스 의존**: 로컬 머신 요구사항 
- **높은 운영 부담**: 네트워크 관리 복잡

#### 📋 Systems Manager Session Manager 장점

#### **AWS 네이티브 통합**
```yaml
완전 관리형 서비스:
  - 서버 관리 불필요
  - 자동 패치 및 업데이트
  - AWS 보안 모델 통합
  - CloudTrail 로깅 자동 연동

IAM 권한 기반:
  - 사용자별 세밀한 접근 제어
  - 역할 기반 권한 관리
  - 임시 자격 증명 사용
  - SSH 키 관리 불필요 ✅
```

#### **Session Manager 작동 원리**
```yaml
접근 과정:
  1. 관리자가 AWS Console/CLI에서 세션 시작
  2. IAM 권한 확인
  3. SSM Agent가 설치된 인스턴스 연결
  4. 브라우저 또는 터미널에서 셸 접근
  5. 모든 활동이 CloudTrail에 로깅

필요 구성요소:
  - EC2에 IAM 역할 연결
  - SSM Agent 설치 (Amazon Linux 기본 포함)
  - 인터넷 또는 VPC 엔드포인트 연결
```

### 🔄 각 솔루션의 운영 복잡도 비교

#### **Session Manager (B번) - 최소 오버헤드**
```yaml
초기 설정:
  1. IAM 역할 생성 (AmazonSSMManagedInstanceCore 정책)
  2. EC2 인스턴스에 역할 연결
  3. 완료 (5분 설정)

지속 운영:
  - 키 관리: 불필요 ✅
  - 서버 관리: 불필요 ✅
  - 보안 패치: AWS 자동 처리 ✅
  - 액세스 로깅: 자동 ✅

확장성:
  - 새 인스턴스: 동일한 IAM 역할만 연결
  - 사용자 추가: IAM 정책만 수정
  - 글로벌 적용: 모든 리전에서 동일
```

#### **Bastion Host (C번) - 높은 오버헤드**
```yaml
초기 설정:
  1. 퍼블릭 서브넷에 Bastion 호스트 생성
  2. 보안 그룹 구성 (복잡)
  3. SSH 키 쌍 생성 및 배포
  4. 각 인스턴스에 공개 키 설치

지속 운영:
  - Bastion 서버 패치 및 관리 ❌
  - SSH 키 순환 (보안 모범 사례) ❌
  - 보안 그룹 관리 ❌
  - 장애 모니터링 및 복구 ❌

확장성:
  - 새 인스턴스: SSH 키 개별 설치
  - 사용자 추가: 키 재배포 필요
  - 고가용성: 다중 Bastion 필요
```

### 📊 Well-Architected Framework 준수도

#### **보안 원칙 (Security Pillar)**
```yaml
Session Manager:
  ✅ 최소 권한 원칙 (IAM 기반)
  ✅ 심층 방어 (다단계 인증 지원)
  ✅ 전송 중 암호화 (TLS)
  ✅ 감사 로깅 (CloudTrail)
  ✅ 자격 증명 중앙 관리

Bastion Host:
  ❌ SSH 키 관리 복잡성
  ❌ 단일 장애점
  ❌ 추가 공격 표면
  ❌ 수동 보안 관리
```

#### **운영 우수성 (Operational Excellence)**
```yaml
Session Manager:
  ✅ 자동화된 배포 (IaC 지원)
  ✅ 표준화된 접근 방식
  ✅ 변경 추적 자동화
  ✅ 운영 간소화

Traditional SSH:
  ❌ 수동 키 관리
  ❌ 복잡한 네트워크 설정
  ❌ 분산된 구성 관리
  ❌ 수동 모니터링 필요
```

#### **비용 최적화 (Cost Optimization)**
```yaml
Session Manager:
  ✅ 추가 인프라 비용 없음
  ✅ 관리 인력 비용 절약
  ✅ 자동화로 운영 효율성

Bastion Host:
  ❌ EC2 인스턴스 비용
  ❌ EIP 비용
  ❌ 운영 관리 인력 비용
  ❌ 고가용성 구성 시 추가 비용
```

### 🔍 실제 구현 시나리오

#### **Session Manager 설정 (B번)**
```yaml
1. IAM 역할 생성:
   역할명: EC2-SSM-Role
   정책: AmazonSSMManagedInstanceCore

2. EC2 인스턴스 설정:
   - 기존 인스턴스: 역할 연결 (재부팅 없이)
   - 신규 인스턴스: 런치 템플릿에 역할 포함

3. 사용자 권한 설정:
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": "ssm:StartSession",
         "Resource": "arn:aws:ec2:*:*:instance/i-*",
         "Condition": {
           "StringEquals": {
             "aws:RequestedRegion": "us-east-1"
           }
         }
       }
     ]
   }

4. 접근 방법:
   - AWS Console: EC2 → Connect → Session Manager
   - AWS CLI: aws ssm start-session --target i-1234567890abcdef0
```

#### **세션 로깅 및 모니터링**
```yaml
자동 로깅:
  - CloudTrail: 세션 시작/종료 API 호출
  - CloudWatch: 세션 기간 및 활동 메트릭
  - S3: 세션 출력 로그 저장 (선택사항)

규정 준수:
  - 모든 관리 활동 추적
  - 사용자별 접근 기록
  - 명령어 실행 히스토리
  - 감사 보고서 자동 생성
```
