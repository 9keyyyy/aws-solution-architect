## Question #73
A company recently launched Linux-based application instances on Amazon EC2 in a private subnet and launched a Linux-based bastion host on an Amazon EC2 instance in a public subnet of a VPC. 
A solutions architect needs to connect from the on-premises network, through the company's internet connection, to the bastion host, and to the application servers. 
The solutions architect must make sure that the security groups of all the EC2 instances will allow that access.

Which combination of steps should the solutions architect take to meet these requirements? (Choose two.)

A. Replace the current security group of the bastion host with one that only allows inbound access from the application instances.

B. Replace the current security group of the bastion host with one that only allows inbound access from the internal IP range for the company.

C. Replace the current security group of the bastion host with one that only allows inbound access from the external IP range for the company.

D. Replace the current security group of the application instances with one that allows inbound SSH access from only the private IP address of the bastion host.

E. Replace the current security group of the application instances with one that allows inbound SSH access from only the public IP address of the bastion host.

## Question #73 분석

### ✅ 요구사항
- **온프레미스 네트워크**에서 접근 시작
- **인터넷 연결**을 통해 접근
- **퍼블릭 서브넷의 Bastion Host**를 경유
- **프라이빗 서브넷의 애플리케이션 서버**에 최종 접근
- **모든 EC2 인스턴스의 보안 그룹** 적절히 설정

### ✅ 접근 경로 분석
```yaml
접근 흐름:
On-premises → Internet → Bastion Host → Application Servers
(Internal IP)   (External)  (Public Subnet)  (Private Subnet)
```

### ✅ 선택지 분석

**A. Bastion 보안 그룹: 애플리케이션 인스턴스만 허용**
- **접근 방향 오류**: 애플리케이션 → Bastion (역방향) ❌
- **요구사항 위배**: 온프레미스에서 Bastion 접근 불가 ❌
- **논리적 오류**: Bastion이 게이트웨이 역할 못함 ❌

**B. Bastion 보안 그룹: 회사 내부 IP 대역만 허용**
- **접근 불가**: 내부 IP는 인터넷을 통해 도달 불가 ❌
- **NAT 이해 부족**: 온프레미스 내부 IP는 인터넷에서 라우팅 안됨 ❌
- **실제 불가능**: 내부 IP 대역은 외부에서 접근 불가 ❌

**C. Bastion 보안 그룹: 회사 외부 IP 대역만 허용** ⭐
- **올바른 접근**: 온프레미스의 퍼블릭 IP 대역 허용 ✅
- **인터넷 경유**: 실제 인터넷을 통한 접근 경로 ✅
- **보안 강화**: 회사 IP 대역만 제한적 허용 ✅
- **NAT 고려**: 온프레미스 NAT 이후의 퍼블릭 IP ✅

**D. 애플리케이션 보안 그룹: Bastion 프라이빗 IP만 허용** ⭐
- **정확한 소스**: Bastion의 VPC 내부 프라이빗 IP ✅
- **최소 권한**: 오직 Bastion에서만 접근 허용 ✅
- **VPC 내부 통신**: 프라이빗 IP로 정확한 내부 통신 ✅
- **보안 원칙**: 최소 필요 권한만 부여 ✅

**E. 애플리케이션 보안 그룹: Bastion 퍼블릭 IP만 허용**
- **잘못된 소스**: VPC 내부에서는 프라이빗 IP 사용 ❌
- **접근 불가**: 퍼블릭 IP는 VPC 내부 통신에서 사용 안됨 ❌
- **네트워킹 오해**: AWS VPC 내부 통신 방식 오해 ❌

### 📋 핵심 개념 정리

#### **Bastion Host 아키텍처**
```yaml
온프레미스 → 인터넷 → Bastion Host → 애플리케이션
(내부IP)     (외부IP)   (퍼블릭서브넷)   (프라이빗서브넷)

네트워크 경로:
1. 온프레미스: 내부 IP (예: 192.168.1.100)
2. NAT/방화벽: 외부 IP로 변환 (예: 203.0.113.50)
3. 인터넷 경유: 외부 IP로 AWS 도달
4. Bastion: 퍼블릭 IP 수신, 프라이빗 IP로 내부 통신
5. 애플리케이션: 프라이빗 IP만 사용
```

#### **보안 그룹 설정 원칙**
```yaml
Bastion Host 보안 그룹:
- 소스: 회사 외부 IP 대역 (예: 203.0.113.0/24)
- 포트: SSH (22)
- 프로토콜: TCP
- 방향: Inbound

애플리케이션 보안 그룹:
- 소스: Bastion Host 프라이빗 IP (예: 10.0.1.10/32)
- 포트: SSH (22) 또는 애플리케이션 포트
- 프로토콜: TCP
- 방향: Inbound
```

#### **IP 주소 이해**
```yaml
온프레미스 네트워크:
- 내부 IP: 192.168.x.x, 10.x.x.x, 172.16-31.x.x
- 외부 IP: ISP가 할당한 퍼블릭 IP
- NAT: 내부 IP → 외부 IP 변환

AWS VPC:
- Bastion 퍼블릭 IP: 인터넷 통신용
- Bastion 프라이빗 IP: VPC 내부 통신용
- 애플리케이션 프라이빗 IP: VPC 내부만
```

#### **정답 시나리오 (C + D)**
```yaml
1. 회사 네트워크 현황 파악
   - 온프레미스 외부 IP 대역: 203.0.113.0/24
   - 방화벽/NAT 설정 확인
   - 실제 아웃바운드 IP 식별

2. Bastion Host 보안 그룹 (C번)
   Inbound Rules:
   - Type: SSH
   - Protocol: TCP
   - Port: 22
   - Source: 203.0.113.0/24 (회사 외부 IP 대역)

3. 애플리케이션 보안 그룹 (D번)
   Inbound Rules:
   - Type: SSH
   - Protocol: TCP  
   - Port: 22
   - Source: 10.0.1.10/32 (Bastion 프라이빗 IP)

4. 접근 테스트
   온프레미스 → Bastion (외부IP 203.0.113.50 → Bastion 퍼블릭IP)
   Bastion → App (Bastion 프라이빗IP 10.0.1.10 → App 프라이빗IP)
```

#### **실제 접속 시나리오**
```yaml
Step 1: 온프레미스에서 Bastion 접속
$ ssh -i key.pem ec2-user@bastion-public-ip
- 소스: 회사 외부 IP (203.0.113.50)
- 대상: Bastion 퍼블릭 IP
- 보안그룹: 회사 외부 IP 대역 허용

Step 2: Bastion에서 애플리케이션 접속  
$ ssh -i key.pem ec2-user@app-private-ip
- 소스: Bastion 프라이빗 IP (10.0.1.10)
- 대상: 애플리케이션 프라이빗 IP
- 보안그룹: Bastion 프라이빗 IP만 허용

결과:
✅ 보안 최적화: 최소 필요 권한만
✅ 접근 가능: 올바른 네트워크 경로
✅ 최소 노출: 불필요한 포트 차단
```

#### **보안 고려사항**
```yaml
Bastion Host 강화:
✅ 키 기반 인증만 허용
✅ 회사 IP 대역만 접근 허용
✅ SSH 포트 변경 고려
✅ 접속 로그 모니터링

애플리케이션 보안:
✅ Bastion에서만 접근 허용
✅ 프라이빗 서브넷 격리
✅ 애플리케이션별 포트 최소화
✅ 세션 타임아웃 설정
```
