## Question #218
A company has a web server running on an Amazon EC2 instance in a public subnet with an Elastic IP address. 
The default security group is assigned to the EC2 instance. 
The default network ACL has been modified to block all traffic. 
A solutions architect needs to make the web server accessible from everywhere on port 443.

Which combination of steps will accomplish this task? (Choose two.)

A. Create a security group with a rule to allow TCP port 443 from source 0.0.0.0/0.

B. Create a security group with a rule to allow TCP port 443 to destination 0.0.0.0/0.

C. Update the network ACL to allow TCP port 443 from source 0.0.0.0/0.

D. Update the network ACL to allow inbound/outbound TCP port 443 from source 0.0.0.0/0 and to destination 0.0.0.0/0.

E. Update the network ACL to allow inbound TCP port 443 from source 0.0.0.0/0 and outbound TCP port 32768-65535 to destination 0.0.0.0/0.

## Question #218 분석 (Choose Two)

### 요구사항
- **웹 서버** EC2 (퍼블릭 서브넷, Elastic IP)
- **기본 보안 그룹** 할당됨
- **기본 네트워크 ACL** 모든 트래픽 차단으로 수정됨
- **포트 443** 전 세계 접근 허용

### 현재 상황
```yaml
문제점:
- 기본 보안 그룹: 아웃바운드만 허용, 인바운드 차단
- 수정된 네트워크 ACL: 모든 트래픽 차단
- 결과: 외부에서 웹 서버 접근 불가

해결 필요:
- 보안 그룹: 인바운드 HTTPS(443) 허용
- 네트워크 ACL: 인바운드/아웃바운드 허용
```

### 선택지 분석

**A. 보안 그룹 인바운드 TCP 443 허용** ⭐
- **인바운드 규칙**: 외부에서 웹 서버로 HTTPS 접근 ✅
- **소스 0.0.0.0/0**: 전 세계 접근 허용 ✅
- **필수 설정**: 웹 서버 접근을 위해 반드시 필요 ✅

**B. 보안 그룹 아웃바운드 TCP 443**
- **방향 오류**: 아웃바운드는 서버에서 외부로 나가는 트래픽 ❌
- **웹 서버 접근과 무관**: 클라이언트 접근에 불필요 ❌

**C. 네트워크 ACL 인바운드 TCP 443만**
- **응답 트래픽 문제**: 아웃바운드 설정 없어 응답 불가 ❌
- **불완전한 해결**: 절반만 해결 ❌

**D. 네트워크 ACL 인바운드/아웃바운드 TCP 443**
- **포트 문제**: 응답 트래픽은 임의 포트 사용 ❌
- **연결 실패**: 클라이언트 응답 받을 수 없음 ❌

**E. 네트워크 ACL 인바운드 443 + 아웃바운드 32768-65535** ⭐
- **완전한 통신**: 요청 수신 + 응답 전송 모두 허용 ✅
- **Ephemeral 포트**: 응답 트래픽의 임시 포트 범위 ✅
- **올바른 네트워크 ACL 설정**: 상태 비저장 특성 고려 ✅

### TCP 통신 과정

```yaml
클라이언트 → 서버 (포트 443):
1. 클라이언트 (임의포트) → 서버 (443)
2. 서버 (443) → 클라이언트 (임의포트)

네트워크 ACL 필요 규칙:
- 인바운드: 포트 443 허용
- 아웃바운드: 포트 32768-65535 허용 (응답용)
```

### 보안 그룹 vs 네트워크 ACL

```yaml
보안 그룹 (상태 저장):
- 인바운드만 설정하면 응답 자동 허용
- 연결 상태 추적

네트워크 ACL (상태 비저장):
- 인바운드와 아웃바운드 별도 설정 필요
- 응답 트래픽용 Ephemeral 포트 고려
```

**정답: A + E**

A안으로 보안 그룹에서 인바운드 HTTPS 접근을 허용하고, E안으로 네트워크 ACL에서 요청 수신과 응답 전송을 모두 허용해야 완전한 웹 서버 접근이 가능합니다.