## Question #266
A company has a popular gaming platform running on AWS. 
The application is sensitive to latency because latency can impact the user experience and introduce unfair advantages to some players. 
The application is deployed in every AWS Region. 
It runs on Amazon EC2 instances that are part of Auto Scaling groups configured behind Application Load Balancers (ALBs). 
A solutions architect needs to implement a mechanism to monitor the health of the application and redirect traffic to healthy endpoints.

Which solution meets these requirements?

A. Configure an accelerator in AWS Global Accelerator. Add a listener for the port that the application listens on, and attach it to a Regional endpoint in each Region. Add the ALB as the endpoint.

B. Create an Amazon CloudFront distribution and specify the ALB as the origin server. Configure the cache behavior to use origin cache headers. Use AWS Lambda functions to optimize the traffic.

C. Create an Amazon CloudFront distribution and specify Amazon S3 as the origin server. Configure the cache behavior to use origin cache headers. Use AWS Lambda functions to optimize the traffic.

D. Configure an Amazon DynamoDB database to serve as the data store for the application. Create a DynamoDB Accelerator (DAX) cluster to act as the in-memory cache for DynamoDB hosting the application data.

## Question #266 분석

### 요구사항
- **게임 플랫폼** (지연시간에 민감)
- **모든 AWS 리전** 배포
- **헬스 모니터링** 및 **정상 엔드포인트로 트래픽 리디렉션**
- **공정한 게임 환경** 유지

### 선택지 분석

**A. AWS Global Accelerator + 리전별 ALB 엔드포인트** ⭐
- **지연시간 최적화**: 가장 가까운 AWS 엣지에서 최적 경로 라우팅 ✅
- **헬스 모니터링**: 엔드포인트 헬스체크로 비정상 리전 자동 제외 ✅
- **게임 최적화**: TCP/UDP 최적화로 실시간 게임에 적합 ✅
- **트래픽 리디렉션**: 리전 장애 시 자동으로 정상 리전으로 전환 ✅

**B. CloudFront + ALB 오리진**
- **캐싱 부적합**: 게임은 실시간 상호작용으로 캐싱 효과 제한적 ❌
- **지연시간**: HTTP 기반으로 TCP 최적화 대비 성능 열위 ❌
- **헬스체크 제한**: 오리진 레벨 헬스체크만 가능 ❌

**C. CloudFront + S3 오리진**
- **정적 콘텐츠 전용**: 게임 애플리케이션 로직 처리 불가 ❌
- **요구사항 불일치**: 동적 게임 플랫폼에 부적절 ❌

**D. DynamoDB + DAX**
- **데이터베이스 솔루션**: 트래픽 라우팅과 무관 ❌
- **헬스 모니터링 무관**: 애플리케이션 엔드포인트 헬스체크 해결 안됨 ❌

### Global Accelerator 게임 최적화

```yaml
게임 플랫폼 특화 기능:
- AWS 글로벌 네트워크 활용
- TCP/UDP 연결 최적화
- 지연시간 25-60% 감소
- 패킷 손실 방지

헬스 모니터링:
- 리전별 ALB 엔드포인트 헬스체크
- 장애 감지 시 30초 내 트래픽 전환
- 자동 장애조치
```
