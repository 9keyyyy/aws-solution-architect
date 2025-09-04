## Question #27
A company is launching a new application and will display application metrics on an Amazon CloudWatch dashboard. 
The company's product manager needs to access this dashboard periodically. 
The product manager does not have an AWS account. 
A solutions architect must provide access to the product manager by following the principle of least privilege.

Which solution will meet these requirements?

A. Share the dashboard from the CloudWatch console. Enter the product manager's email address, and complete the sharing steps. Provide a shareable link for the dashboard to the product manager.

B. Create an IAM user specifically for the product manager. Attach the CloudWatchReadOnlyAccess AWS managed policy to the user. Share the new login credentials with the product manager. Share the browser URL of the correct dashboard with the product manager.

C. Create an IAM user for the company's employees. Attach the ViewOnlyAccess AWS managed policy to the IAM user. Share the new login credentials with the product manager. Ask the product manager to navigate to the CloudWatch console and locate the dashboard by name in the Dashboards section.

D. Deploy a bastion server in a public subnet. When the product manager requires access to the dashboard, start the server and share the RDP credentials. On the bastion server, ensure that the browser is configured to open the dashboard URL with cached AWS credentials that have appropriate permissions to view the dashboard.

## Question #27 분석

### ✅ 요구사항
- **외부 사용자** (AWS 계정 없음)에게 CloudWatch 대시보드 액세스
- **최소 권한 원칙** 준수
- 제품 매니저의 **주기적 접근** 필요

### ✅ 선택지 분석

**A. CloudWatch 대시보드 공유 기능** ⭐
- **외부 공유 전용 기능**: AWS 계정 없이도 접근 ✅
- **최소 권한**: 해당 대시보드만 접근 ✅
- **간단한 설정**: 이메일 + 링크로 완료 ✅
- **AWS 네이티브 기능**: 보안 최적화 ✅

**B. IAM 사용자 + CloudWatchReadOnlyAccess**
- **과도한 권한**: 모든 CloudWatch 리소스 접근 
- **최소 권한 위배**: 필요 이상의 권한 부여
- AWS 계정 생성 필요

**C. IAM 사용자 + ViewOnlyAccess**
- **매우 과도한 권한**: 모든 AWS 서비스 읽기 권한 
- **최소 권한 심각 위배**: 불필요한 광범위 접근
- 보안 위험 증가

**D. Bastion 서버 + RDP**
- **과도하게 복잡**: 불필요한 인프라 
- **높은 운영 오버헤드**: 서버 관리 필요 
- **보안 위험**: RDP 노출 위험

### 📋 CloudWatch 대시보드 공유 기능

### **외부 공유 메커니즘**
```yaml
공유 과정:
  1. CloudWatch Console → 대시보드 선택
  2. "Share dashboard" 클릭
  3. "Share with specific people" 선택
  4. 이메일 주소 입력
  5. 권한 레벨 설정 (View-only)
  6. 공유 링크 생성

결과:
  - 제품 매니저가 이메일로 링크 수신
  - AWS 로그인 없이 대시보드 접근
  - 해당 대시보드만 볼 수 있음
```

### **보안 특징**
```yaml
최소 권한 구현:
  ✅ 특정 대시보드만 접근
  ✅ 읽기 전용 권한
  ✅ 다른 AWS 리소스 접근 불가
  ✅ 시간 제한 설정 가능

외부 접근 최적화:
  - AWS 계정 생성 불필요
  - 자격 증명 관리 불필요
  - 안전한 토큰 기반 인증
```

### 🔄 다른 옵션들의 문제점

### **IAM 사용자 방식 (B, C번)**
```yaml
CloudWatchReadOnlyAccess 문제점:
  ❌ 모든 CloudWatch 메트릭 접근
  ❌ 다른 대시보드들도 접근 가능
  ❌ 로그 그룹, 알람 등 불필요한 권한

ViewOnlyAccess 문제점:
  ❌ 모든 AWS 서비스 읽기 권한
  ❌ EC2, S3, RDS 등 민감 정보 접근
  ❌ 최소 권한 원칙 심각 위배
  ❌ 컴플라이언스 위험
```

### **Bastion 서버 방식 (D번)**
```yaml
복잡성 문제:
  ❌ EC2 인스턴스 관리 필요
  ❌ RDP 보안 설정 필요
  ❌ VPC, 보안 그룹 구성
  ❌ 정기적인 패치 및 유지보수

보안 위험:
  ❌ RDP 포트 노출
  ❌ 캐시된 자격 증명 위험
  ❌ 서버 침해 시 AWS 계정 노출
  ❌ 불필요한 공격 표면 확장
```

### 💡 최소 권한 원칙 비교

### **권한 범위 비교**
```yaml
대시보드 공유 (A번):
  접근 범위: 특정 대시보드 1개만
  권한 레벨: 읽기 전용
  AWS 리소스: 해당 메트릭만

CloudWatchReadOnlyAccess (B번):
  접근 범위: 모든 CloudWatch 리소스
  권한 레벨: 읽기 전용
  AWS 리소스: CloudWatch 전체

ViewOnlyAccess (C번):
  접근 범위: 모든 AWS 서비스
  권한 레벨: 읽기 전용  
  AWS 리소스: 계정 내 모든 리소스
```
