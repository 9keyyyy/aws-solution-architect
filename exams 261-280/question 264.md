## Question #264
A company has a web application hosted over 10 Amazon EC2 instances with traffic directed by Amazon Route 53. 
The company occasionally experiences a timeout error when attempting to browse the application. 
The networking team finds that some DNS queries return IP addresses of unhealthy instances, resulting in the timeout error.

What should a solutions architect implement to overcome these timeout errors?

A. Create a Route 53 simple routing policy record for each EC2 instance. Associate a health check with each record.

B. Create a Route 53 failover routing policy record for each EC2 instance. Associate a health check with each record.

C. Create an Amazon CloudFront distribution with EC2 instances as its origin. Associate a health check with the EC2 instances.

D. Create an Application Load Balancer (ALB) with a health check in front of the EC2 instances. Route to the ALB from Route 53.

## Question #264 분석

### 문제 상황
- **10개 EC2 인스턴스** 웹 애플리케이션
- **Route 53**으로 트래픽 라우팅
- **타임아웃 에러** 발생
- **비정상 인스턴스 IP**가 DNS 응답에 포함됨

### 선택지 분석

**A. Simple 라우팅 정책 + 헬스체크**
- **Simple 라우팅 제한**: 헬스체크를 지원하지만 모든 IP를 반환 ❌
- **비정상 인스턴스**: 여전히 비정상 인스턴스 IP가 응답에 포함될 수 있음 ❌

**B. Failover 라우팅 정책 + 헬스체크**
- **용도 불일치**: Failover는 Primary/Secondary 구조용 ❌
- **10개 인스턴스**: 동등한 10개 인스턴스에는 부적절 ❌

**C. CloudFront + EC2 오리진 + 헬스체크**
- **오리진 헬스체크**: CloudFront는 비정상 오리진 자동 제외 
- **캐싱 레이어**: 추가 성능 향상 
- **글로벌 배포**: 지연시간 개선 

**D. ALB + 헬스체크 + Route 53** ⭐
- **ALB 헬스체크**: 비정상 인스턴스를 자동으로 트래픽에서 제외 ✅
- **단일 엔드포인트**: Route 53이 ALB 하나의 IP만 반환 ✅
- **자동 복구**: 인스턴스 복구 시 자동으로 트래픽 재개 ✅
- **근본 해결**: DNS 레벨 문제를 로드 밸런서 레벨에서 해결 ✅

### 문제 해결 방식

```yaml
현재 문제:
Route 53 → 비정상 인스턴스 IP 포함 → 타임아웃

D안 솔루션:
Route 53 → ALB (단일 엔드포인트) → 정상 인스턴스만
- ALB가 헬스체크로 비정상 인스턴스 자동 제외
- 클라이언트는 항상 정상 작동하는 엔드포인트만 받음
```
