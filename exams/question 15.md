## Question #15
A company recently migrated to AWS and wants to implement a solution to protect the traffic that flows in and out of the production VPC. 
The company had an inspection server in its on-premises data center. 
The inspection server performed specific operations such as traffic flow inspection and traffic filtering. 
The company wants to have the same functionalities in the AWS Cloud.

Which solution will meet these requirements?

A. Use Amazon GuardDuty for traffic inspection and traffic filtering in the production VPC.

B. Use Traffic Mirroring to mirror traffic from the production VPC for traffic inspection and filtering.

C. Use AWS Network Firewall to create the required rules for traffic inspection and traffic filtering for the production VPC.

D. Use AWS Firewall Manager to create the required rules for traffic inspection and traffic filtering for the production VPC.

## Question #15 분석

### ✅ 요구사항
- 프로덕션 VPC **in/out 트래픽 보호**
- 온프레미스 검사 서버 기능 복제
- **트래픽 흐름 검사** (Traffic Flow Inspection)
- **트래픽 필터링** (Traffic Filtering)

### ✅ 선택지 분석

**A. Amazon GuardDuty**
- **위협 탐지 서비스** (보안 분석)
- 트래픽 차단/필터링 기능 없음 ❌
- 로그 분석 기반, 실시간 인라인 처리 불가
- 알림만 제공, 실제 차단 불가

**B. Traffic Mirroring**
- 트래픽 **복사본 생성** 서비스
- 실제 트래픽 차단/허용 불가 ❌
- 분석용 미러링만 제공
- 인라인 필터링 기능 없음

**C. AWS Network Firewall** ⭐
- **관리형 네트워크 방화벽** 서비스
- VPC 인/아웃바운드 트래픽 **인라인 검사**
- **실시간 필터링 및 차단**
- 상태 기반 방화벽 + IPS/IDS 기능

**D. AWS Firewall Manager**
- **방화벽 정책 관리** 서비스
- 실제 트래픽 검사/필터링 불가 ❌
- 다른 방화벽 서비스들의 중앙 관리 도구
- Security Group, WAF 등의 정책 관리

### 📋 서비스별 기능 비교

### **AWS Network Firewall**
```yaml
트래픽 검사 기능:
  - 패킷 레벨 딥 인스펙션
  - 애플리케이션 레이어 검사 (L7)
  - 상태 기반 연결 추적
  - 침입 탐지/방지 (IDS/IPS)

필터링 기능:
  - 도메인 기반 차단
  - IP/포트 기반 차단
  - 애플리케이션 프로토콜 차단
  - 커스텀 규칙 생성

배포 위치:
  - VPC 경계에서 인라인 처리
  - 인바운드/아웃바운드 모두 지원
  - 서브넷 간 트래픽도 검사 가능
```

### **Amazon GuardDuty vs Network Firewall**
```yaml
GuardDuty:
  - 위협 "탐지" 전용 ❌
  - 로그 분석 기반
  - 사후 알림 제공
  - 실시간 차단 불가

Network Firewall:
  - 위협 "차단" 가능 ✅
  - 실시간 인라인 처리
  - 즉시 트래픽 드롭
  - 프로액티브 보호
```

### **Traffic Mirroring vs Network Firewall**
```yaml
Traffic Mirroring:
  - 트래픽 "복사" 전용 ❌
  - 원본 트래픽 변경 불가
  - 분석 목적으로만 사용
  - 차단 기능 없음

Network Firewall:
  - 트래픽 "제어" 가능 ✅
  - 허용/차단 결정
  - 인라인 처리
  - 실시간 보호
```

### **Firewall Manager vs Network Firewall**
```yaml
Firewall Manager:
  - 정책 "관리" 도구 ❌
  - 여러 계정 중앙 관리
  - 실제 방화벽 기능 없음
  - Security Group 정책 관리

Network Firewall:
  - 실제 "방화벽" 기능 ✅
  - 트래픽 검사/차단
  - VPC 레벨 보호
  - 인라인 처리
```

### 🔄 AWS Network Firewall 동작 원리

### **VPC 트래픽 흐름**
```
인터넷 ↕
    ↓
Internet Gateway
    ↓
Network Firewall ← 여기서 검사/필터링
    ↓
Route Table
    ↓
EC2 Instances (프로덕션)
```
