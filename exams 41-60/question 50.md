## Question #50
A company has a production workload that runs on 1,000 Amazon EC2 Linux instances. 
The workload is powered by third-party software. 
The company needs to patch the third-party software on all EC2 instances as quickly as possible to remediate a critical security vulnerability.

What should a solutions architect do to meet these requirements?

A. Create an AWS Lambda function to apply the patch to all EC2 instances.

B. Configure AWS Systems Manager Patch Manager to apply the patch to all EC2 instances.

C. Schedule an AWS Systems Manager maintenance window to apply the patch to all EC2 instances.

D. Use AWS Systems Manager Run Command to run a custom command that applies the patch to all EC2 instances.

## Question #50 분석

### ✅ 요구사항
- **1,000대 EC2 Linux 인스턴스**
- **서드파티 소프트웨어** 패치 필요
- **중요한 보안 취약점** 해결
- **가능한 빠르게** 패치 적용

### ✅ 선택지 분석

**A. Lambda 함수로 패치 적용**
- **15분 실행 제한**: 대규모 패치 작업에 부적합 
- **네트워크 연결 복잡성**: EC2에 직접 접근 어려움 
- **확장성 한계**: 1,000대 동시 처리 제약 

**B. Systems Manager Patch Manager**
- **OS 패치 전용**: 서드파티 소프트웨어 지원 제한적 
- **사전 정의된 패치**: 커스텀 서드파티 패치 처리 어려움 
- **Windows/Amazon Linux 중심**: 범용 서드파티 지원 부족

**C. Systems Manager 유지보수 창**
- **스케줄링 도구**: 즉시 실행보다는 계획된 작업 
- **빠른 대응 부적합**: 중요 보안 패치에 시간 지연 
- **추가 설정 시간**: 긴급 상황에 비효율적

**D. Systems Manager Run Command** ⭐
- **즉시 실행**: 명령어 즉시 대규모 배포 ✅
- **커스텀 스크립트**: 서드파티 소프트웨어 패치 지원 ✅
- **대규모 확장**: 수천 대 인스턴스 동시 처리 ✅
- **빠른 배포**: 중요 보안 패치에 최적 ✅

### 📋 Systems Manager Run Command

#### **대규모 명령 실행 최적화**
```yaml
동시 실행 능력:
  - 최대 50개 인스턴스 동시 실행 (기본값)
  - 동시성 설정 조정으로 확장 가능
  - 배치 단위로 순차 실행
  - 1,000대도 효율적 처리

실행 시간:
  - 즉시 명령 전송
  - 각 인스턴스에서 병렬 실행
  - 전체 완료: 수분-수십분 내
```

#### **서드파티 소프트웨어 패치**
```bash
# Run Command 스크립트 예시
#!/bin/bash
# 서드파티 소프트웨어 패치 스크립트
wget https://vendor.com/security-patch.rpm
sudo rpm -Uvh security-patch.rpm
sudo systemctl restart third-party-service
echo "Patch applied successfully"
```

### 🔄 다른 옵션들의 한계

#### **Patch Manager 제약 (B번)**
```yaml
지원 범위:
  - OS 레벨 패치 (yum, apt 등)
  - AWS에서 관리하는 패치 베이스라인
  - 표준 리포지토리 패키지

서드파티 소프트웨어:
  ❌ 벤더별 커스텀 패치 절차
  ❌ 특수한 설치/재시작 요구사항
  ❌ 비표준 패키지 관리자
```

#### **Lambda 확장성 한계 (A번)**
```yaml
기술적 제약:
  - 실행 시간: 최대 15분
  - 동시 실행: 1,000개 (기본 할당량)
  - 네트워크: VPC 설정 복잡
  - 패치 작업: 15분 초과 가능성

1,000대 처리:
  ❌ 순차 처리 시 16시간+ 소요
  ❌ 병렬 처리 시 할당량 초과
  ❌ 네트워크 타임아웃 위험
```

#### **유지보수 창 지연 (C번)**
```yaml
스케줄링 오버헤드:
  - 유지보수 창 생성 및 설정
  - 실행 시간 계획 및 승인
  - 추가 설정 단계

긴급 상황 부적합:
  ❌ 중요 보안 패치는 즉시 적용 필요
  ❌ 공격 창구 최소화 중요
  ❌ 계획된 다운타임 불필요
```
