## Question #261
A company recently announced the deployment of its retail website to a global audience. 
The website runs on multiple Amazon EC2 instances behind an Elastic Load Balancer. 
The instances run in an Auto Scaling group across multiple Availability Zones.

The company wants to provide its customers with different versions of content based on the devices that the customers use to access the website.

Which combination of actions should a solutions architect take to meet these requirements? (Choose two.)

A. Configure Amazon CloudFront to cache multiple versions of the content.

B. Configure a host header in a Network Load Balancer to forward traffic to different instances.

C. Configure a Lambda@Edge function to send specific objects to users based on the User-Agent header.

D. Configure AWS Global Accelerator. Forward requests to a Network Load Balancer (NLB). Configure the NLB to set up host-based routing to different EC2 instances.

E. Configure AWS Global Accelerator. Forward requests to a Network Load Balancer (NLB). Configure the NLB to set up path-based routing to different EC2 instances.

## Question #261 분석 (Choose Two)

### 요구사항
- **글로벌 소매 웹사이트** 배포
- **ELB + Auto Scaling + 멀티 AZ** 아키텍처
- **디바이스별 다른 콘텐츠 버전** 제공

### 선택지 분석

**A. CloudFront에서 다중 버전 콘텐츠 캐시** ⭐
- **글로벌 배포**: 전 세계 엣지 로케이션에서 빠른 콘텐츠 제공 ✅
- **다중 버전 캐시**: 디바이스별 콘텐츠를 엣지에서 캐시 가능 ✅
- **성능 향상**: 오리진 서버 부하 감소 및 응답 시간 단축 ✅

**B. NLB의 호스트 헤더 기반 라우팅**
- **Layer 4 제한**: NLB는 Layer 4에서 작동, HTTP 헤더 처리 불가 ❌
- **디바이스 감지 불가**: User-Agent 헤더 분석 불가능 ❌

**C. Lambda@Edge에서 User-Agent 기반 객체 전송** ⭐
- **디바이스 감지**: User-Agent 헤더로 디바이스 타입 식별 ✅
- **엣지 처리**: 사용자에 가까운 위치에서 즉시 처리 ✅
- **동적 콘텐츠 제공**: 디바이스에 맞는 특정 객체 전송 ✅
- **CloudFront 통합**: A안과 완벽한 조합 ✅

**D. Global Accelerator + NLB + 호스트 기반 라우팅**
- **NLB 한계**: 동일하게 Layer 4에서 HTTP 헤더 처리 불가 ❌
- **복잡성**: 불필요한 아키텍처 복잡화 ❌

**E. Global Accelerator + NLB + 경로 기반 라우팅**
- **동일한 NLB 문제**: Layer 4 제한으로 디바이스 감지 불가 ❌
- **경로 기반 부적합**: 디바이스별 콘텐츠와 URL 경로는 별개 ❌

### 솔루션 조합 (A + C)

```yaml
아키텍처:
1. 사용자 요청 → CloudFront 엣지 로케이션
2. Lambda@Edge가 User-Agent 헤더 분석
3. 디바이스 타입에 따른 콘텐츠 결정
4. CloudFront에서 적절한 버전 캐시/제공
5. 필요시 오리진 서버에서 새 콘텐츠 가져오기

디바이스별 처리:
- 모바일: 경량화된 이미지, 터치 최적화 UI
- 태블릿: 중간 해상도, 터치 지원
- 데스크톱: 고해상도, 마우스 최적화
```
