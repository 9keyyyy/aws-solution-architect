## Question #1004
A company hosts its enterprise resource planning (ERP) system in the us-east-1 Region. The system runs on Amazon EC2 instances. Customers use a public API that is hosted on the EC2 instances to exchange information with the ERP system. International customers report slow API response times from their data centers.

Which solution will improve response times for the international customers MOST cost-effectively?

A. Create an AWS Direct Connect connection that has a public virtual interface (VIF) to provide connectivity from each customer's data center to us-east-1. Route customer API requests by using a Direct Connect gateway to the ERP system API.

B. Set up an Amazon CloudFront distribution in front of the API. Configure the CachingOptimized managed cache policy to provide improved cache efficiency.

C. Set up AWS Global Accelerator. Configure listeners for the necessary ports. Configure endpoint groups for the appropriate Regions to distribute traffic. Create an endpoint in the group for the API.

D. Use AWS Site-to-Site VPN to establish dedicated VPN tunnels between Regions and customer networks. Route traffic to the API over the VPN connections.

## Question #1004 분석

### 문제 상황
- **ERP 시스템** (us-east-1)
- **국제 고객**들이 API 사용
- **느린 응답 시간** 문제
- **비용 효율적** 해결책 필요

### 핵심 요구사항
- **국제 고객 응답 시간 개선**
- **비용 효율성**
- **API 성능 향상**

### 선택지 분석

**A. Direct Connect + Public VIF**
- 각 고객마다 Direct Connect 필요 → 매우 비쌈 ❌
- 비용 효율적이지 않음 ❌

**B. CloudFront + CachingOptimized**
- API는 동적 데이터 → 캐싱 효과 제한적 ❌
- ERP API는 실시간 데이터 교환이므로 캐싱 부적합 ❌

**C. AWS Global Accelerator** ⭐
- AWS 글로벌 네트워크 활용 ✅
- 동적 콘텐츠(API) 성능 향상 ✅
- 비용 효율적 ✅
- 지연 시간 대폭 감소 ✅

**D. Site-to-Site VPN**
- 각 고객마다 VPN 설정 → 복잡하고 비쌈 ❌
- 인터넷 기반이라 성능 향상 제한적 ❌

### CloudFront vs Global Accelerator

| 구분 | CloudFront | Global Accelerator |
|------|------------|-------------------|
| **용도** | 정적 콘텐츠 캐싱 | 동적 트래픽 가속 |
| **API** | 캐싱 제한적 | 최적화됨 |
| **비용** | 낮음 | 중간 |

### Global Accelerator 장점

```yaml
네트워크 최적화:
- AWS 백본 네트워크 사용
- 인터넷 경로 우회
- 지연 시간 60% 감소

비용 효율성:
- 단일 설정으로 모든 고객 혜택
- 고객별 개별 연결 불필요
```

