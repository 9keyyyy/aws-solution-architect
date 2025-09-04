## Question #32
A development team needs to host a website that will be accessed by other teams. 
The website contents consist of HTML, CSS, client-side JavaScript, and images.

Which method is the MOST cost-effective for hosting the website?

A. Containerize the website and host it in AWS Fargate.

B. Create an Amazon S3 bucket and host the website there.

C. Deploy a web server on an Amazon EC2 instance to host the website.

D. Configure an Application Load Balancer with an AWS Lambda target that uses the Express.js framework.

## Question #32 분석

### ✅ 요구사항
- **정적 웹사이트** (HTML, CSS, JavaScript, 이미지)
- 다른 팀들이 접근 가능
- **가장 비용 효율적인** 호스팅 방법

### ✅ 선택지 분석

**A. AWS Fargate 컨테이너 호스팅**
- **과도한 인프라**: 정적 파일에 컨테이너 오버킬 
- **높은 비용**: vCPU, 메모리 시간당 과금 
- **운영 복잡성**: 컨테이너 이미지 관리 필요

**B. Amazon S3 정적 웹사이트 호스팅** ⭐
- **정적 콘텐츠 전용**: HTML/CSS/JS에 완벽 적합 ✅
- **최저 비용**: 스토리지 + 요청 수만 과금 ✅
- **서버 관리 불필요**: 완전 관리형 ✅
- **고가용성**: 내장된 99% 내구성 ✅

**C. EC2 웹서버**
- **과도한 인프라**: 정적 파일에 서버 불필요 
- **높은 비용**: 24시간 인스턴스 운영 비용 
- **운영 오버헤드**: 서버 패치, 보안 관리 필요

**D. ALB + Lambda + Express.js**
- **과도한 복잡성**: 정적 파일에 서버리스 프레임워크 
- **높은 비용**: Lambda 실행 + ALB 시간당 과금 
- **불필요한 기술 스택**: Express.js가 정적 파일에 오버킬

### 📋 정적 웹사이트 vs 동적 웹사이트

### **정적 웹사이트 특성**
```yaml
구성 요소:
  ✅ HTML 파일 (구조)
  ✅ CSS 파일 (스타일)
  ✅ JavaScript 파일 (클라이언트 사이드 로직)
  ✅ 이미지 파일 (PNG, JPG, SVG 등)

서버 처리 불필요:
  - 데이터베이스 연결 없음
  - 서버사이드 렌더링 없음
  - 사용자 인증 없음 (클라이언트에서 처리 가능)
```

### **S3 정적 호스팅 적합성**
```yaml
완벽한 매칭:
  ✅ HTML/CSS/JS 파일 직접 서빙
  ✅ 브라우저가 파일 다운로드 후 렌더링
  ✅ 클라이언트 사이드 JavaScript 실행
  ✅ CDN 연동으로 성능 최적화 가능
```

### 💰 비용 비교 분석

### **S3 정적 호스팅 비용 (B번)**
```yaml
스토리지 비용:
  - Standard: $0.023 per GB/월
  - 예시: 1GB 웹사이트 = $0.023/월

요청 비용:
  - GET 요청: $0.0004 per 1,000 requests
  - 예시: 월 10만 페이지뷰 = $0.04/월

총 월 비용 예상: ~$0.10-1.00 ✅
```

### **EC2 웹서버 비용 (C번)**
```yaml
인스턴스 비용:
  - t3.micro: $8.5/월 (24시간 운영)
  - t3.small: $17/월

추가 비용:
  - EBS 스토리지: $8/월 (80GB)
  - 데이터 전송: 변동

총 월 비용 예상: ~$25-50 ❌
```

### **Fargate 비용 (A번)**
```yaml
컴퓨팅 리소스:
  - 0.25 vCPU + 0.5GB 메모리
  - $0.04048 per vCPU/시간
  - $0.004445 per GB/시간

24시간 운영:
  - vCPU: $0.04048 × 0.25 × 24 × 30 = $7.3/월
  - 메모리: $0.004445 × 0.5 × 24 × 30 = $1.6/월

총 월 비용 예상: ~$10-20 ❌
```

### **Lambda + ALB 비용 (D번)**
```yaml
ALB 비용:
  - 시간당: $0.0225 = $16.4/월
  - LCU: $0.008 per LCU

Lambda 비용:
  - 요청: $0.20 per 1M requests
  - 실행 시간: $0.0000166667 per GB-second

총 월 비용 예상: ~$20-40 ❌
```

### 🔄 S3 정적 호스팅 설정

### **간단한 설정 과정**
```yaml
1단계: S3 버킷 생성
   - 버킷명: company-internal-website
   - 리전 선택

2단계: 정적 호스팅 활성화
   - Properties → Static website hosting
   - Index document: index.html
   - Error document: error.html

3단계: 파일 업로드
   - HTML, CSS, JS, 이미지 파일 업로드
   - 퍼블릭 읽기 권한 설정

4단계: 접근
   - S3 웹사이트 엔드포인트로 접근
   - 예: http://bucket-name.s3-website-region.amazonaws.com
```

### **추가 최적화 옵션**
```yaml
CloudFront 연동:
  - 전 세계 캐싱으로 성능 향상
  - HTTPS 지원
  - 추가 비용: $1-5/월

Route 53 도메인:
  - 커스텀 도메인 연결
  - www.company.internal 같은 친숙한 URL
  - 추가 비용: $0.5/월
```

### 📊 운영 복잡도 비교

### **S3 호스팅 (B번) - 최소 복잡도**
```yaml
일일 운영: 0시간
배포: 파일 업로드만
보안: IAM 정책 설정
모니터링: CloudWatch 기본 메트릭
업데이트: 파일 교체만
```

### **EC2 호스팅 (C번) - 높은 복잡도**
```yaml
일일 운영: 서버 상태 확인
배포: SSH 접속 후 파일 복사
보안: OS 패치, 보안 그룹 관리
모니터링: 서버 리소스 모니터링
업데이트: 웹서버 재시작 등
```
