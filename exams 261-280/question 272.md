## Question #272
A company serves a dynamic website from a fleet of Amazon EC2 instances behind an Application Load Balancer (ALB). 
The website needs to support multiple languages to serve customers around the world. 
The website’s architecture is running in the us-west-1 Region and is exhibiting high request latency for users that are located in other parts of the world.

The website needs to serve requests quickly and efficiently regardless of a user’s location. 
However, the company does not want to recreate the existing architecture across multiple Regions.

What should a solutions architect do to meet these requirements?

A. Replace the existing architecture with a website that is served from an Amazon S3 bucket. Configure an Amazon CloudFront distribution with the S3 bucket as the origin. Set the cache behavior settings to cache based on the Accept-Language request header.

B. Configure an Amazon CloudFront distribution with the ALB as the origin. Set the cache behavior settings to cache based on the Accept-Language request header.

C. Create an Amazon API Gateway API that is integrated with the ALB. Configure the API to use the HTTP integration type. Set up an API Gateway stage to enable the API cache based on the Accept-Language request header.

D. Launch an EC2 instance in each additional Region and configure NGINX to act as a cache server for that Region. Put all the EC2 instances and the ALB behind an Amazon Route 53 record set with a geolocation routing policy.

## Question #272 분석

### 요구사항
- **동적 웹사이트** (ALB + EC2) us-west-1
- **다국어 지원** 필요
- **글로벌 사용자** 높은 지연시간 문제
- **기존 아키텍처 재생성 금지**

### 선택지 분석

**A. S3 정적 웹사이트 + CloudFront**
- **동적 웹사이트 불가**: S3는 정적 콘텐츠만 호스팅 ❌
- **기존 아키텍처 폐기**: 요구사항 위반 ❌

**B. CloudFront + ALB 오리진 + Accept-Language 캐싱** ⭐
- **기존 아키텍처 유지**: ALB와 EC2 그대로 보존 ✅
- **글로벌 성능**: CloudFront 엣지로 지연시간 해결 ✅
- **다국어 캐싱**: Accept-Language 헤더 기반 언어별 캐싱 ✅
- **동적 콘텐츠**: ALB 뒤 EC2에서 동적 처리 ✅

**C. API Gateway + ALB 통합**
- **불필요한 복잡성**: API Gateway 레이어 추가로 복잡화 ❌
- **성능 저하**: 추가 홉으로 지연시간 증가 ❌
- **글로벌 배포 없음**: 여전히 us-west-1에서만 처리 ❌

**D. 각 리전 NGINX + Route 53**
- **아키텍처 재생성**: 각 리전에 인프라 복제 필요 ❌
- **요구사항 위반**: 명시적으로 금지된 방식 ❌

### CloudFront Accept-Language 캐싱

```yaml
캐시 동작 설정:
- Origin: ALB 엔드포인트
- Cache Behavior: Accept-Language 헤더 포함
- TTL 설정: 적절한 캐시 만료 시간

효과:
- 언어별 콘텐츠 엣지 캐싱
- 동일 언어 요청 시 캐시 히트
- 글로벌 사용자 지연시간 대폭 감소
```
