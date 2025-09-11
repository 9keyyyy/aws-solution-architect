## Question #983

A company hosts its core network services, including directory services and DNS, in its on-premises data center. The data center is connected to the AWS Cloud using AWS Direct Connect (DX). Additional AWS accounts are planned that will require quick, cost-effective, and consistent access to these network services.

What should a solutions architect implement to meet these requirements with the LEAST amount of operational overhead?

A. Create a DX connection in each new account. Route the network traffic to the on-premises servers.

B. Configure VPC endpoints in the DX VPC for all required services. Route the network traffic to the on-premises servers.

C. Create a VPN connection between each new account and the DX VPRoute the network traffic to the on-premises servers.

D. Configure AWS Transit Gateway between the accounts. Assign DX to the transit gateway and route network traffic to the on-premises servers.

## Question #983 분석

### 문제 상황
- **온프레미스 핵심 네트워크 서비스** (디렉터리, DNS)
- **Direct Connect**로 AWS 연결
- **추가 AWS 계정들**이 이 서비스에 접근 필요
- **빠르고, 비용 효율적, 일관된 접근** 필요

### 핵심 요구사항
- **다중 계정 네트워크 서비스 접근**
- **최소 운영 오버헤드**
- **비용 효율성**

### 선택지 분석

**A. 각 계정마다 DX 연결**
- 각 계정별 DX 비용 → 매우 비쌈 ❌
- 운영 복잡성 증가 ❌

**B. DX VPC에 VPC Endpoints**
- VPC Endpoints는 AWS 서비스용 ❌
- 온프레미스 서비스에는 부적합 ❌

**C. 각 계정과 DX VPC 간 VPN**
- 계정별 VPN 관리 → 운영 부담 ❌
- 확장성 제한 ❌

**D. Transit Gateway + DX 연결** ⭐
- 중앙화된 연결 허브 ✅
- 모든 계정이 단일 DX 공유 ✅
- 확장성 우수 ✅
- 최소 운영 오버헤드 ✅

### Transit Gateway 장점

```yaml
Hub-and-Spoke 모델:
- 중앙: Transit Gateway + DX
- Spoke: 각 AWS 계정 VPC

효과:
- 단일 DX로 모든 계정 연결
- 새 계정 추가시 TGW 연결만 하면 됨
- 라우팅 간소화
```

### 비용 및 운영 비교

**Option D (Transit Gateway)**
```yaml
비용: DX 1개 + TGW 요금
운영: 중앙 관리
확장: 간단한 VPC 연결
```

**Option A (각 계정 DX)**
```yaml
비용: DX × 계정 수 (매우 비쌈)
운영: 개별 관리
확장: 각각 새 DX 필요
```
