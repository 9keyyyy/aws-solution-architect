## Question #251
An Amazon EC2 instance is located in a private subnet in a new VPC. 
This subnet does not have outbound internet access, but the EC2 instance needs the ability to download monthly security updates from an outside vendor.

What should a solutions architect do to meet these requirements?

A. Create an internet gateway, and attach it to the VPC. Configure the private subnet route table to use the internet gateway as the default route.

B. Create a NAT gateway, and place it in a public subnet. Configure the private subnet route table to use the NAT gateway as the default route.

C. Create a NAT instance, and place it in the same subnet where the EC2 instance is located. Configure the private subnet route table to use the NAT instance as the default route.

D. Create an internet gateway, and attach it to the VPC. Create a NAT instance, and place it in the same subnet where the EC2 instance is located. Configure the private subnet route table to use the internet gateway as the default route.

## Question #251 분석

### 요구사항
- **EC2 인스턴스**: 새 VPC의 프라이빗 서브넷
- **아웃바운드 인터넷 접근 없음** (현재 상황)
- **월간 보안 업데이트 다운로드** 필요
- **외부 벤더**에서 다운로드

### 선택지 분석

**A. IGW + 프라이빗 서브넷 라우팅**
- **프라이빗 서브넷 목적 위반**: IGW 직접 연결로 퍼블릭 서브넷화 ❌
- **보안 위험**: EC2가 인터넷에 직접 노출됨 ❌
- **인바운드 접근 가능**: 외부에서 EC2로 직접 접근 위험 ❌

**B. NAT Gateway (퍼블릭 서브넷) + 프라이빗 라우팅** ⭐
- **아웃바운드 전용**: 프라이빗 EC2가 인터넷으로만 나갈 수 있음 ✅
- **인바운드 차단**: 외부에서 EC2로 직접 접근 불가 ✅
- **올바른 배치**: NAT Gateway를 퍼블릭 서브넷에 배치 ✅
- **보안 유지**: 프라이빗 서브넷 특성 유지 ✅

**C. NAT Instance (같은 프라이빗 서브넷)**
- **인터넷 접근 불가**: NAT Instance도 프라이빗 서브넷에 있어 인터넷 접근 불가 ❌
- **배치 오류**: NAT Instance는 퍼블릭 서브넷에 있어야 함 ❌

**D. IGW + NAT Instance (같은 프라이빗 서브넷)**
- **동일한 문제**: NAT Instance가 프라이빗 서브넷에 있어 작동 불가 ❌
- **라우팅 오류**: IGW로 직접 라우팅하면 NAT Instance 우회 ❌

### NAT Gateway 아키텍처

```yaml
올바른 구성 (B안):
1. 퍼블릭 서브넷 생성
2. 퍼블릭 서브넷에 NAT Gateway 배치
3. 퍼블릭 서브넷 라우팅: 0.0.0.0/0 → IGW
4. 프라이빗 서브넷 라우팅: 0.0.0.0/0 → NAT Gateway

트래픽 흐름:
프라이빗 EC2 → NAT Gateway → IGW → 인터넷
(인바운드는 불가능)
```
