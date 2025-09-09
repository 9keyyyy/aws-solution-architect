## Question #254
A company is reviewing a recent migration of a three-tier application to a VPC. 
The security team discovers that the principle of least privilege is not being applied to Amazon EC2 security group ingress and egress rules between the application tiers.

What should a solutions architect do to correct this issue?

A. Create security group rules using the instance ID as the source or destination.

B. Create security group rules using the security group ID as the source or destination.

C. Create security group rules using the VPC CIDR blocks as the source or destination.

D. Create security group rules using the subnet CIDR blocks as the source or destination.

## Question #254 분석

### 요구사항
- **3계층 애플리케이션** VPC 마이그레이션 완료
- **최소 권한 원칙** 미적용 (보안 그룹 규칙)
- **계층 간 통신** 보안 강화 필요

### 선택지 분석

**A. 인스턴스 ID를 소스/대상으로 사용**
- **기술적 불가능**: 보안 그룹 규칙에서 인스턴스 ID 직접 참조 불가 ❌
- **확장성 부족**: 인스턴스 추가/제거 시 규칙 수동 업데이트 필요 ❌

**B. 보안 그룹 ID를 소스/대상으로 사용** ⭐
- **최소 권한**: 특정 계층의 보안 그룹만 접근 허용 ✅
- **동적 관리**: 인스턴스 추가/제거 시 자동 적용 ✅
- **계층별 격리**: 웹-앱-DB 계층 간 정확한 분리 ✅
- **확장성**: Auto Scaling 시에도 자동 적용 ✅

**C. VPC CIDR 블록 사용**
- **과도한 권한**: VPC 내 모든 리소스에 접근 허용 ❌
- **최소 권한 위반**: 불필요한 광범위한 접근 ❌

**D. 서브넷 CIDR 블록 사용**
- **부분적 개선**: 서브넷 단위 제한은 있지만 여전히 광범위 ❌
- **계층 혼재**: 같은 서브넷에 다른 용도 리소스 존재 시 문제 ❌

### 보안 그룹 참조의 장점

```yaml
3계층 애플리케이션 예시:
Web Tier Security Group (web-sg):
- 인바운드: 0.0.0.0/0:443 (HTTPS)
- 아웃바운드: app-sg:8080

App Tier Security Group (app-sg):
- 인바운드: web-sg:8080
- 아웃바운드: db-sg:3306

DB Tier Security Group (db-sg):
- 인바운드: app-sg:3306
- 아웃바운드: 없음
```

### 최소 권한 원칙 구현

```yaml
보안 그룹 참조 효과:
- 웹 계층: 앱 계층에만 접근 가능
- 앱 계층: DB 계층에만 접근 가능
- DB 계층: 외부 접근 완전 차단
- 동일 계층 내 불필요한 통신 차단
```
