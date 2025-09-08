## Question #74
A solutions architect is designing a two-tier web application. 
The application consists of a public-facing web tier hosted on Amazon EC2 in public subnets. 
The database tier consists of Microsoft SQL Server running on Amazon EC2 in a private subnet. 
Security is a high priority for the company.

How should security groups be configured in this situation? (Choose two.)

A. Configure the security group for the web tier to allow inbound traffic on port 443 from 0.0.0.0/0.

B. Configure the security group for the web tier to allow outbound traffic on port 443 from 0.0.0.0/0.

C. Configure the security group for the database tier to allow inbound traffic on port 1433 from the security group for the web tier.

D. Configure the security group for the database tier to allow outbound traffic on ports 443 and 1433 to the security group for the web tier.

E. Configure the security group for the database tier to allow inbound traffic on ports 443 and 1433 from the security group for the web tier.

## Question #74 분석

### ✅ 요구사항
- **2계층 웹 애플리케이션** 설계
- **웹 계층**: 퍼블릭 서브넷의 EC2 (공개 대면)
- **데이터베이스 계층**: 프라이빗 서브넷의 Microsoft SQL Server
- **보안 최우선**: 높은 보안 수준 요구

### ✅ 애플리케이션 아키텍처
```yaml
인터넷 사용자 → 웹 계층 → 데이터베이스 계층
(HTTPS/443)    (Public)   (Private/SQL Server)
                           (Port 1433)
```

### ✅ 선택지 분석

**A. 웹 계층: 포트 443 인바운드 0.0.0.0/0에서 허용** ⭐
- **HTTPS 웹 서비스**: 표준 HTTPS 포트 443 ✅
- **인터넷 접근**: 전 세계 사용자의 웹 접근 허용 ✅
- **퍼블릭 웹 서비스**: 공개 웹사이트의 표준 설정 ✅
- **필수 설정**: 웹 계층의 기본 요구사항 ✅

**B. 웹 계층: 포트 443 아웃바운드 0.0.0.0/0으로 허용**
- **아웃바운드 기본값**: AWS 보안 그룹은 기본적으로 모든 아웃바운드 허용 ⚠️
- **불필요한 설정**: 명시적 설정 불필요 ⚠️
- **보안상 문제**: 과도한 아웃바운드 권한 부여 ❌
- **요구사항 무관**: 웹 → 인터넷 HTTPS는 일반적이지 않음 ❌

**C. DB 계층: 포트 1433 인바운드 웹 계층 보안 그룹에서 허용** ⭐
- **SQL Server 포트**: 표준 SQL Server 포트 1433 ✅
- **정확한 소스**: 웹 계층 보안 그룹만 허용 ✅
- **최소 권한 원칙**: 필요한 소스만 제한적 허용 ✅
- **계층간 통신**: 웹 → DB 연결의 핵심 설정 ✅

**D. DB 계층: 포트 443, 1433 아웃바운드 웹 계층으로 허용**
- **잘못된 방향**: DB에서 웹으로의 아웃바운드는 비정상 ❌
- **아키텍처 오해**: 일반적으로 웹 → DB 방향 통신 ❌
- **불필요한 포트**: 443은 DB 계층에서 불필요 ❌
- **보안 위험**: 불필요한 아웃바운드 권한 ❌

**E. DB 계층: 포트 443, 1433 인바운드 웹 계층에서 허용**
- **SQL Server 포트**: 1433은 올바름 ✅
- **불필요한 포트**: 443은 DB 계층에서 불필요 ❌
- **과도한 권한**: 필요 이상의 포트 오픈 ❌
- **보안 원칙 위배**: 최소 권한 원칙 위반 ❌

### 📋 핵심 개념 정리

#### **2계층 웹 애플리케이션 통신 흐름**
```yaml
정상적인 트래픽 흐름:
1. 사용자 → 웹 계층 (HTTPS/443)
2. 웹 계층 → DB 계층 (SQL/1433)
3. DB 계층 → 웹 계층 (응답)
4. 웹 계층 → 사용자 (HTTPS 응답)

포트별 역할:
- 443: HTTPS 웹 트래픽
- 1433: Microsoft SQL Server 기본 포트
```

#### **보안 그룹 설정 원칙**
```yaml
웹 계층 보안 그룹:
Inbound:
  ✅ Port 443 from 0.0.0.0/0 (전 세계 HTTPS 접근)
  ✅ Port 80 from 0.0.0.0/0 (HTTP, 선택적)
  ✅ Port 22 from 관리 IP (SSH 관리)

Outbound:
  ✅ Port 1433 to DB 보안 그룹 (DB 연결)
  ✅ Port 443 to 0.0.0.0/0 (API 호출 등, 필요시)

DB 계층 보안 그룹:
Inbound:
  ✅ Port 1433 from 웹 계층 보안 그룹
  ✅ Port 22 from Bastion (관리, 선택적)

Outbound:
  ✅ 기본 설정 (모든 아웃바운드 허용)
  또는 제한적 설정 (보안 강화)
```

#### **Microsoft SQL Server 포트**
```yaml
SQL Server 기본 포트:
- TCP 1433: 기본 인스턴스
- TCP 1434: SQL Server Browser (UDP도 사용)
- 동적 포트: Named 인스턴스 (권장하지 않음)

보안 고려사항:
✅ 기본 포트 1433 사용
✅ 웹 계층에서만 접근 허용
✅ 인터넷에서 직접 접근 차단
✅ 프라이빗 서브넷 배치
```
