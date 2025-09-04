## Question #12
A global company hosts its web application on Amazon EC2 instances behind an Application Load Balancer (ALB). 
The web application has static data and dynamic data. 
The company stores its static data in an Amazon S3 bucket. 
The company wants to improve performance and reduce latency for the static data and dynamic data. 
The company is using its own domain name registered with Amazon Route 53.

What should a solutions architect do to meet these requirements?

A. Create an Amazon CloudFront distribution that has the S3 bucket and the ALB as origins. Configure Route 53 to route traffic to the CloudFront distribution.

B. Create an Amazon CloudFront distribution that has the ALB as an origin. Create an AWS Global Accelerator standard accelerator that has the S3 bucket as an endpoint Configure Route 53 to route traffic to the CloudFront distribution.

C. Create an Amazon CloudFront distribution that has the S3 bucket as an origin. Create an AWS Global Accelerator standard accelerator that has the ALB and the CloudFront distribution as endpoints. Create a custom domain name that points to the accelerator DNS name. Use the custom domain name as an endpoint for the web application.

D. Create an Amazon CloudFront distribution that has the ALB as an origin. Create an AWS Global Accelerator standard accelerator that has the S3 bucket as an endpoint. Create two domain names. Point one domain name to the CloudFront DNS name for dynamic content. Point the other domain name to the accelerator DNS name for static content. Use the domain names as endpoints for the web application.

### ✅ 요구사항
- 글로벌 회사의 웹 애플리케이션이 ALB 뒤 EC2에서 실행
- 정적 데이터는 S3 버킷에 저장
- 동적 데이터는 EC2/ALB에서 처리
- 정적/동적 데이터 모두 성능 및 지연시간 개선 필요
- Route 53에 등록된 자체 도메인 사용

### ✅ 선택지 분석
**A. CloudFront distribution (S3 + ALB origins) + Route 53** ⭐
- CloudFront가 정적 데이터(S3)와 동적 데이터(ALB) 모두 캐싱
- 단일 distribution으로 통합 관리
- Route 53이 트래픽을 CloudFront로 라우팅
- 글로벌 엣지 로케이션 활용으로 지연시간 감소
- 비용 효율적이고 단순한 아키텍처

**B. CloudFront (ALB) + Global Accelerator (S3)**
- 정적/동적 콘텐츠가 다른 서비스로 분산
- Global Accelerator는 S3 정적 콘텐츠에 부적합
- 아키텍처 복잡성 증가
- 불필요한 서비스 조합

**C. CloudFront (S3) + Global Accelerator (ALB + CloudFront)**
- Global Accelerator에 CloudFront를 엔드포인트로 사용하는 이상한 구조
- 정적 콘텐츠만 CloudFront 혜택
- 동적 콘텐츠는 Global Accelerator만 사용
- 과도하게 복잡한 설계

**D. 분리된 도메인 + 각각 다른 서비스**
- 두 개의 별도 도메인 필요
- 사용자 경험 복잡화
- 관리 오버헤드 증가
- 요구사항의 "단일 도메인" 위배

### ✅ 개념 정리
### 1. CloudFront Multi-Origin 구성
#### 1.1 Origin 설정
```yaml
Origins:
  - S3 Origin: static-content.s3.amazonaws.com
    Path Pattern: /static/*
    Cache Behavior: Long TTL (1년)
    
  - ALB Origin: app-alb.us-east-1.elb.amazonaws.com  
    Path Pattern: Default (*)
    Cache Behavior: Short TTL (1시간) 또는 Dynamic
```

#### 1.2 Path-based Routing
```
/static/images/* → S3 Origin (정적 콘텐츠)
/api/*          → ALB Origin (동적 콘텐츠)  
/               → ALB Origin (기본 경로)
```

#### 1.3 캐싱 전략
- **정적 콘텐츠 (S3)**
  - 긴 TTL (24시간 ~ 1년)
  - 압축 활성화
  - 브라우저 캐싱 헤더 설정
- **동적 콘텐츠 (ALB)**
  - 짧은 TTL 또는 캐시 없음
  - Query string/Headers 기반 캐싱
  - 세션 기반 라우팅 고려

### 2. 서비스별 적합성 분석
#### 2.1 CloudFront 장점
- **글로벌 CDN 네트워크**
  - 400+ 엣지 로케이션
  - 지연시간 최소화
  - 대역폭 절약
- **다중 Origin 지원**
  - S3와 ALB 동시 처리 가능
  - Path-based routing
  - Origin failover 지원

#### 2.2 Global Accelerator vs CloudFront
```
Global Accelerator:
- TCP/UDP 레벨 최적화
- 정적 IP 주소 제공
- 지역 기반 트래픽 라우팅
- 캐싱 기능 없음

CloudFront:
- HTTP/HTTPS 최적화  
- 콘텐츠 캐싱
- 압축 및 최적화
- 정적/동적 모두 처리 가능
```

### 3. 아키텍처 비교
#### 3.1 선택지 A (권장)
```
사용자 → Route 53 → CloudFront Distribution
                   ├── S3 Origin (정적)
                   └── ALB Origin (동적)
```
- 단일 엔드포인트
- 통합 SSL 인증서
- 간단한 DNS 관리

#### 3.2 선택지 B/C/D (비권장)
```
사용자 → 복잡한 라우팅 → 여러 서비스
```
- 관리 복잡성 증가
- 비용 증가
- 잠재적 성능 저하

**핵심:** CloudFront의 다중 Origin 기능을 활용하면 정적/동적 콘텐츠를 모두 효율적으로 처리할 수 있습니다.