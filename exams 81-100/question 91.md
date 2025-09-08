## Question #91
A company has applications that run on Amazon EC2 instances in a VPC. 
One of the applications needs to call the Amazon S3 API to store and read objects. 
According to the company's security regulations, no traffic from the applications is allowed to travel across the internet.

Which solution will meet these requirements?

A. Configure an S3 gateway endpoint.

B. Create an S3 bucket in a private subnet.

C. Create an S3 bucket in the same AWS Region as the EC2 instances.

D. Configure a NAT gateway in the same subnet as the EC2 instances.

## Question #91 분석

### ✅ 요구사항
- **VPC 내 EC2 애플리케이션**이 S3 API 호출 필요
- **보안 규정**: 인터넷을 통한 트래픽 **완전 금지**
- **S3 읽기/쓰기** 기능 필요

### ✅ 핵심 문제
```yaml
현재 상황:
❌ S3 API 호출 시 인터넷 경유 필요
❌ 보안 규정 위반 위험

해결 방향:
✅ VPC 내부에서 S3 직접 접근
✅ 인터넷 우회 경로 구성
✅ AWS 백본 네트워크 활용
```

### ✅ 선택지 분석

**A. S3 Gateway Endpoint 구성** ⭐
- **VPC 내부 경로**: S3 트래픽이 인터넷 우회 ✅
- **AWS 백본 네트워크**: 완전히 AWS 내부 통신 ✅
- **보안 요구사항 충족**: 인터넷 트래픽 완전 차단 ✅
- **비용 무료**: Gateway Endpoint 사용료 없음 ✅

**B. S3 버킷을 프라이빗 서브넷에 생성**
- **기술적 불가능**: S3는 AWS 관리형 서비스, 특정 서브넷 배치 불가 ❌
- **개념 오해**: S3는 VPC 외부 글로벌 서비스 ❌

**C. 동일 리전에 S3 버킷 생성**
- **인터넷 경유 지속**: 여전히 인터넷을 통한 S3 접근 ❌
- **보안 요구사항 미충족**: 트래픽이 인터넷 경유 ❌
- **리전 위치 무관**: 네트워크 경로와 별개 ❌

**D. NAT Gateway 구성**
- **인터넷 경유**: NAT Gateway는 인터넷 접근용 ❌
- **보안 규정 위반**: 명시적으로 금지된 인터넷 트래픽 ❌
- **목적 불일치**: 아웃바운드 인터넷 접근 솔루션 ❌

### 📋 S3 Gateway Endpoint 작동 방식

**네트워크 경로 변경**
```yaml
Before (인터넷 경유):
EC2 → Internet Gateway → Internet → S3

After (Gateway Endpoint):
EC2 → S3 Gateway Endpoint → AWS Backbone → S3

결과:
✅ 인터넷 완전 우회
✅ AWS 내부 네트워크만 사용
✅ 보안 요구사항 충족
```

**구현 과정**
```yaml
1. VPC Gateway Endpoint 생성
   - Service: com.amazonaws.region.s3
   - VPC 선택
   - Route Table 연결

2. 라우팅 자동 업데이트
   - S3 트래픽이 자동으로 Endpoint 경유
   - 기존 애플리케이션 코드 변경 불필요

3. 즉시 적용
   - 인터넷 트래픽 완전 차단
   - S3 기능 정상 동작
```
