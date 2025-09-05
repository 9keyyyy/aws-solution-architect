## Question #56
A company has registered its domain name with Amazon Route 53. 
The company uses Amazon API Gateway in the ca-central-1 Region as a public interface for its backend microservice APIs. 
Third-party services consume the APIs securely. 
The company wants to design its API Gateway URL with the company's domain name and corresponding certificate so that the third-party services can use HTTPS.

Which solution will meet these requirements?

A. Create stage variables in API Gateway with Name="Endpoint-URL" and Value="Company Domain Name" to overwrite the default URL. Import the public certificate associated with the company's domain name into AWS Certificate Manager (ACM).

B. Create Route 53 DNS records with the company's domain name. Point the alias record to the Regional API Gateway stage endpoint. Import the public certificate associated with the company's domain name into AWS Certificate Manager (ACM) in the us-east-1 Region.

C. Create a Regional API Gateway endpoint. Associate the API Gateway endpoint with the company's domain name. Import the public certificate associated with the company's domain name into AWS Certificate Manager (ACM) in the same Region. Attach the certificate to the API Gateway endpoint. Configure Route 53 to route traffic to the API Gateway endpoint.

D. Create a Regional API Gateway endpoint. Associate the API Gateway endpoint with the company's domain name. Import the public certificate associated with the company's domain name into AWS Certificate Manager (ACM) in the us-east-1 Region. Attach the certificate to the API Gateway APIs. Create Route 53 DNS records with the company's domain name. Point an A record to the company's domain name.

## Question #56 분석

### ✅ 요구사항
- **Route 53으로 도메인 등록됨**
- **API Gateway** (ca-central-1 리전)
- **커스텀 도메인명** 사용 원함
- **HTTPS 인증서** 필요
- **제3자 서비스**가 안전하게 API 소비

### ✅ 선택지 분석

**A. Stage Variables + ACM 인증서**
- **Stage Variables**: URL 오버라이드 불가능 ❌
- **Stage Variables 역할**: 환경별 설정값 저장용 ❌
- **커스텀 도메인 구성 없음**: DNS 연결 누락 ❌

**B. Route 53 DNS + us-east-1 ACM**
- **Route 53 DNS**: 올바른 접근 ✅
- **Regional API Gateway**: 별도 언급 없음 ⚠️
- **us-east-1 ACM**: Regional API Gateway는 **동일 리전**에 ACM 필요 ❌
- **ca-central-1 요구**: API Gateway가 ca-central-1에 위치 ❌

**C. Regional API Gateway + 동일 리전 ACM + Route 53** ⭐
- **Regional API Gateway**: 명시적 생성 ✅
- **커스텀 도메인 연결**: API Gateway와 도메인 연동 ✅
- **동일 리전 ACM**: ca-central-1에서 ACM 인증서 ✅
- **Route 53 라우팅**: DNS → API Gateway 연결 ✅
- **완전한 워크플로우**: 모든 단계 포함 ✅

**D. Regional API Gateway + us-east-1 ACM + A 레코드**
- **us-east-1 ACM**: Regional API Gateway는 동일 리전 필요 ❌
- **A 레코드**: API Gateway는 ALIAS 레코드 사용 권장 ⚠️
- **리전 불일치**: ca-central-1 ≠ us-east-1 ❌

### 📋 핵심 개념 정리

#### **Regional API Gateway vs Edge-Optimized**
```yaml
Regional API Gateway:
  ✅ 특정 리전에만 배포
  ✅ 낮은 지연 시간 (해당 리전 내)
  ✅ ACM 인증서: 동일 리전 필요
  ✅ 비용 효율적
  📍 예: ca-central-1에만 존재

Edge-Optimized API Gateway:
  ✅ CloudFront로 전 세계 배포
  ✅ 글로벌 최적화
  ✅ ACM 인증서: us-east-1 필수
  💰 CloudFront 비용 추가
  📍 예: 전 세계 엣지 로케이션 배포
```

#### **Route 53 DNS 서비스**
```yaml
Amazon Route 53:
  ✅ AWS 관리형 DNS 서비스
  ✅ 도메인 등록 및 관리
  ✅ DNS 레코드 관리 (A, ALIAS, CNAME 등)
  ✅ 헬스 체크 및 장애 조치
  
DNS 레코드 타입:
  A 레코드: 도메인 → IP 주소
  ALIAS 레코드: 도메인 → AWS 리소스 (권장)
  CNAME 레코드: 도메인 → 다른 도메인
```

#### **ACM 인증서 리전 규칙**
```yaml
Regional API Gateway:
  ✅ API Gateway와 동일 리전의 ACM 인증서
  
Edge-Optimized API Gateway:
  ✅ us-east-1 리전의 ACM 인증서 (CloudFront용)

ALB/NLB:
  ✅ 로드 밸런서와 동일 리전의 ACM 인증서
```

#### **정답 시나리오 (C번)**
```yaml
1. Regional API Gateway 생성 (ca-central-1)
   - 캐나다 중부 리전에만 배포
   - 로컬 트래픽 최적화
   
2. ACM에서 인증서 준비 (ca-central-1)
   - 회사 도메인명으로 인증서 가져오기
   - Regional API Gateway와 동일 리전
   
3. API Gateway 커스텀 도메인 설정
   - 도메인명과 API Gateway 연결
   - ACM 인증서 연결
   
4. Route 53 DNS 설정
   - ALIAS 레코드: api.company.com → API Gateway
   - DNS 쿼리를 API Gateway로 라우팅
   
5. 결과
   ✅ https://api.company.com → API Gateway
   ✅ HTTPS 보안 연결
   ✅ 제3자 서비스 안전한 API 접근
```
