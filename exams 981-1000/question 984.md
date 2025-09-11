## Question #984
A company hosts its main public web application in one AWS Region across multiple Availability Zones. The application uses an Amazon EC2 Auto Scaling group and an Application Load Balancer (ALB).

A web development team needs a cost-optimized compute solution to improve the company’s ability to serve dynamic content globally to millions of customers.

Which solution will meet these requirements?

A. Create an Amazon CloudFront distribution. Configure the existing ALB as the origin.

B. Use Amazon Route 53 to serve traffic to the ALB and EC2 instances based on the geographic location of each customer.

C. Create an Amazon S3 bucket with public read access enabled. Migrate the web application to the S3 bucket. Configure the S3 bucket for website hosting.

D. Use AWS Direct Connect to directly serve content from the web application to the location of each customer.

## Question #984 분석

### 문제 상황
- **단일 리전** 웹 애플리케이션 (Multi-AZ)
- **EC2 Auto Scaling + ALB** 구성
- **전 세계 수백만 고객**에게 동적 콘텐츠 제공
- **비용 최적화된 컴퓨팅** 솔루션 필요

### 핵심 요구사항
- **글로벌 성능 향상**
- **동적 콘텐츠 제공**
- **비용 최적화**

### 선택지 분석

**A. CloudFront + 기존 ALB를 Origin으로 설정** ⭐
- 글로벌 엣지 로케이션 활용 ✅
- 동적 콘텐츠 지원 ✅
- 기존 인프라 재사용 ✅
- 비용 효율적 (캐싱으로 백엔드 부하 감소) ✅

**B. Route 53 지리적 라우팅**
- DNS 라우팅만으로는 성능 개선 제한적 ❌
- 여전히 단일 리전에서 처리 ❌
- 비용 최적화 효과 부족 ❌

**C. S3 정적 웹사이트 호스팅**
- 동적 콘텐츠 지원 불가 ❌
- 현재 "동적 콘텐츠" 요구사항과 맞지 않음 ❌

**D. Direct Connect**
- 온프레미스-AWS 연결용 ❌
- 고객 위치별 직접 연결 불가능 ❌
- 매우 비효율적이고 비현실적 ❌

### CloudFront 동적 콘텐츠 처리

```yaml
CloudFront 기능:
- 동적 콘텐츠 가속 (캐싱 불가능한 요청도 네트워크 최적화)
- 엣지 로케이션에서 Origin까지 최적 경로
- 전 세계 200+ 엣지 로케이션

비용 최적화:
- 캐싱 가능한 콘텐츠는 엣지에서 제공
- Origin 서버 부하 감소
- 데이터 전송 비용 절감
```

### 글로벌 성능 개선

**현재**: 고객 → 인터넷 → 단일 리전 ALB
**개선**: 고객 → 가까운 엣지 → 최적 경로 → ALB
