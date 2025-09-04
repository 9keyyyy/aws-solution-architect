## Question #38
A company is hosting a static website on Amazon S3 and is using Amazon Route 53 for DNS. 
The website is experiencing increased demand from around the world. 
The company must decrease latency for users who access the website.

Which solution meets these requirements MOST cost-effectively?

A. Replicate the S3 bucket that contains the website to all AWS Regions. Add Route 53 geolocation routing entries.

B. Provision accelerators in AWS Global Accelerator. Associate the supplied IP addresses with the S3 bucket. Edit the Route 53 entries to point to the IP addresses of the accelerators.

C. Add an Amazon CloudFront distribution in front of the S3 bucket. Edit the Route 53 entries to point to the CloudFront distribution.

D. Enable S3 Transfer Acceleration on the bucket. Edit the Route 53 entries to point to the new endpoint.

## Question #38 분석

### ✅ 요구사항
- **정적 웹사이트** (S3 호스팅)
- **전 세계적 수요 증가**
- **지연 시간 감소** 필요
- **가장 비용 효율적인** 솔루션

### ✅ 선택지 분석

**A. 모든 리전에 S3 복제 + Route 53 지리 라우팅**
- **높은 복제 비용**: 모든 리전에 스토리지 비용 
- **복잡한 관리**: 여러 버킷 동기화 
- **과도한 리소스**: 정적 사이트에 오버킬 

**B. AWS Global Accelerator + S3**
- **S3 직접 연결 불가**: Global Accelerator는 S3 정적 웹사이트 지원 안함 
- **기술적 불가능**: 아키텍처 미스매치 

**C. CloudFront + S3** ⭐
- **CDN 전용 설계**: 정적 콘텐츠 배포 최적화 ✅
- **글로벌 엣지 위치**: 전 세계 캐싱 ✅
- **비용 효율적**: 트래픽 기반 과금, 캐시 효율 ✅
- **간단한 설정**: S3와 네이티브 통합 ✅

**D. S3 Transfer Acceleration**
- **업로드 전용**: 파일 업로드 가속화만 지원 
- **다운로드 개선 없음**: 웹사이트 접근 속도 향상 없음 
- **목적 불일치**: 웹사이트 방문자에게 도움 안됨


### **CloudFront CDN의 작동 원리**
```yaml
글로벌 캐싱 네트워크:
  - 전 세계 400+ 엣지 로케이션
  - 사용자와 가장 가까운 엣지에서 콘텐츠 제공
  - 첫 요청 시 S3에서 가져와 엣지에 캐시
  - 이후 요청은 엣지에서 즉시 응답

지연 시간 개선:
  - 서울 사용자 → 서울 엣지 (10-50ms)
  - 직접 S3 → 버지니아 (150-300ms)
  - 70-90% 지연 시간 감소 ✅
```



### 🔍 각 솔루션의 문제점

#### **S3 Transfer Acceleration (D번)**
```yaml
서비스 목적:
  ✅ 클라이언트 → S3 업로드 가속화
  ✅ 파일 업로드 시 CloudFront 엣지 활용
  ✅ PUT, POST 요청 최적화

웹사이트 방문에는 부적합:
  ❌ S3 → 사용자 다운로드 개선 없음
  ❌ GET 요청 가속화 안됨
  ❌ 정적 웹사이트 성능 향상 없음

비유: 
  "짐을 창고에 넣는 속도는 빨라지지만,
   창고에서 짐을 꺼내는 속도는 그대로"
```

#### **Global Accelerator 한계 (B번)**
```yaml
지원 대상:
  ✅ Application Load Balancer
  ✅ Network Load Balancer  
  ✅ Elastic IP (EC2)
  ❌ S3 정적 웹사이트 엔드포인트

S3 정적 웹사이트는:
  - HTTP 엔드포인트 (ALB 아님)
  - AWS 리소스 ARN 없음
  - Global Accelerator 타겟 불가
```

#### **CloudFront 설정 (C번)**
```yaml
1단계: CloudFront Distribution 생성
   Origin: S3 버킷 (example-bucket.s3.amazonaws.com)
   Cache Behavior: 정적 파일 최적화
   Geographic Restrictions: 필요 시 설정

2단계: Route 53 레코드 수정
   기존: example.com → s3-bucket-endpoint
   변경: example.com → d123456abcdef0.cloudfront.net

3단계: 캐시 최적화
   TTL 설정: HTML (1시간), CSS/JS (24시간), 이미지 (30일)
   Compression: Gzip 압축 활성화
   
설정 시간: 30분, 전파 시간: 15-30분
```
