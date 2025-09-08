## Question #83
A company's dynamic website is hosted using on-premises servers in the United States. 
The company is launching its product in Europe, and it wants to optimize site loading times for new European users. 
The site's backend must remain in the United States. 
The product is being launched in a few days, and an immediate solution is needed.

What should the solutions architect recommend?

A. Launch an Amazon EC2 instance in us-east-1 and migrate the site to it.

B. Move the website to Amazon S3. Use Cross-Region Replication between Regions.

C. Use Amazon CloudFront with a custom origin pointing to the on-premises servers.

D. Use an Amazon Route 53 geoproximity routing policy pointing to on-premises servers.

## Question #83 분석

### ✅ 요구사항
- **동적 웹사이트** 온프레미스 서버 운영 (미국)
- **유럽 진출**: 유럽 사용자 로딩 시간 최적화
- **백엔드 제약**: 미국에 그대로 유지 필요
- **시간 압박**: 며칠 내 출시, 즉시 솔루션 필요

### ✅ 핵심 고려사항
```yaml
성능 요구사항:
✅ 유럽 사용자 로딩 시간 최적화
✅ 동적 콘텐츠 지원
✅ 백엔드 위치 고정 (미국)

구현 제약사항:
✅ 즉시 구현 가능
✅ 기존 인프라 변경 최소화
✅ 안정적 서비스 제공

성능 최적화 전략:
- 지리적 거리 단축
- 네트워크 지연 감소
- 글로벌 엣지 네트워크 활용
```

### ✅ 선택지 분석

**A. EC2 us-east-1 마이그레이션**
- **지리적 개선 없음**: 여전히 미국 동부 위치 ❌
- **유럽 최적화 실패**: 거리상 개선 효과 미미 ❌
- **마이그레이션 시간**: 며칠 내 완료 어려움 ❌
- **복잡성**: 전체 애플리케이션 이전 필요 ❌

**B. S3 + Cross-Region Replication**
- **정적 콘텐츠만**: 동적 웹사이트 지원 불가 ❌
- **백엔드 처리 없음**: S3는 서버 사이드 로직 미지원 ❌
- **요구사항 불충족**: 동적 기능 손실 ❌
- **아키텍처 불일치**: 웹사이트 특성과 맞지 않음 ❌

**C. CloudFront + 커스텀 오리진** ⭐
- **글로벌 CDN**: 전 세계 엣지 로케이션 활용 ✅
- **동적 콘텐츠 지원**: 커스텀 오리진으로 동적 처리 ✅
- **즉시 구현**: 설정 후 즉시 적용 가능 ✅
- **백엔드 유지**: 온프레미스 서버 그대로 유지 ✅
- **성능 최적화**: 유럽 엣지에서 캐싱 및 가속 ✅

**D. Route 53 지오근접 라우팅**
- **DNS 라우팅만**: 실제 콘텐츠 가속화 없음 ❌
- **백엔드 위치 고정**: 여전히 미국 서버로 모든 요청 ❌
- **성능 개선 제한**: 네트워크 지연 근본 해결 안됨 ❌
- **단순 라우팅**: CDN 없이는 성능 향상 미미 ❌

### 📋 핵심 개념 정리

#### **CloudFront 동적 콘텐츠 가속화**
```yaml
CloudFront 작동 방식:
1. 유럽 사용자 요청
2. 가장 가까운 유럽 엣지 로케이션 접속
3. 캐시 미스 시 오리진 서버 요청
4. AWS 백본 네트워크로 최적화된 경로
5. 응답을 엣지에서 캐시 (설정에 따라)
6. 사용자에게 응답 전달

동적 콘텐츠 최적화:
✅ TCP 커넥션 최적화
✅ Keep-Alive 연결 재사용
✅ AWS 백본 네트워크 활용
✅ 압축 및 최적화
```

#### **커스텀 오리진 설정**
```yaml
Origin 설정:
- Origin Domain: your-company.com
- Origin Protocol: HTTPS (보안)
- Origin Port: 443 또는 80
- Origin Path: / (루트)

Cache Behavior:
- Default TTL: 0 (동적 콘텐츠)
- Maximum TTL: 86400 (정적 리소스)
- Compress Objects: Yes
- Viewer Protocol Policy: Redirect HTTP to HTTPS
```

#### **성능 향상 효과**
```yaml
개선 메커니즘:
1. 지리적 거리 단축
   - 미국 → 유럽: ~150ms 지연
   - 유럽 엣지 → 유럽: ~10-30ms

2. 네트워크 최적화
   - AWS 글로벌 백본 활용
   - 인터넷 라우팅 우회
   - TCP 최적화

3. 캐싱 효과
   - 정적 리소스 엣지 캐싱
   - 반복 요청 즉시 응답
   - 오리진 부하 감소

예상 성능 개선:
✅ 초기 로딩: 50-70% 개선
✅ 정적 리소스: 80-90% 개선
✅ 전체 사용자 경험: 현저한 향상
```

#### **정답 시나리오 (C번)**
```yaml
1. CloudFront Distribution 생성
   Origin Settings:
   - Origin Domain Name: your-onpremises-domain.com
   - Origin Protocol Policy: HTTPS Only
   - HTTP Port: 80, HTTPS Port: 443

2. Default Cache Behavior 설정
   - Viewer Protocol Policy: Redirect HTTP to HTTPS
   - Allowed HTTP Methods: GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE
   - Cache Based on Selected Request Headers: All
   - Object Caching: Use Origin Cache Headers

3. Distribution Settings
   - Price Class: Use All Edge Locations
   - Alternate Domain Names: www.yourcompany.eu
   - SSL Certificate: Request ACM Certificate

4. DNS 설정
   - 유럽 사용자를 CloudFront Distribution으로 라우팅
   - CNAME 또는 A 레코드 설정

5. 즉시 효과
   ✅ 15-20분 내 글로벌 배포 완료
   ✅ 유럽 사용자 즉시 성능 향상 체험
   ✅ 기존 인프라 변경 없음
   ✅ 백엔드 미국 유지
```

#### **다른 옵션들의 한계**

**A안 (EC2 마이그레이션) 문제점:**
```yaml
시간 소요:
❌ 애플리케이션 분석: 수일
❌ 환경 구성: 수일  
❌ 데이터 마이그레이션: 수일
❌ 테스트 및 검증: 수일
총 소요: 2-4주 (요구사항 초과)

위험 요소:
❌ 마이그레이션 중 장애 위험
❌ 데이터 손실 가능성
❌ 성능 저하 위험
❌ 롤백 복잡성
```

**B안 (S3) 기술적 제약:**
```yaml
동적 웹사이트 요구사항:
❌ 서버 사이드 처리 (PHP, Python, Java 등)
❌ 데이터베이스 연동
❌ 세션 관리
❌ 사용자 인증/권한
❌ API 엔드포인트

S3 제공 기능:
✅ 정적 파일 호스팅만
✅ 클라이언트 사이드 JavaScript만
```

**D안 (Route 53) 성능 한계:**
```yaml
DNS 라우팅의 한계:
❌ 물리적 거리는 그대로
❌ 네트워크 지연 지속
❌ 캐싱 효과 없음
❌ 백본 네트워크 활용 안됨

실제 개선 효과:
- 지연시간: 거의 변화 없음
- 처리량: 동일
- 사용자 경험: 미미한 개선
```
