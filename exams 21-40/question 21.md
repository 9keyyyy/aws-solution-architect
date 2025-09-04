## Question #21
An ecommerce company wants to launch a one-deal-a-day website on AWS. 
Each day will feature exactly one product on sale for a period of 24 hours. 
The company wants to be able to handle millions of requests each hour with millisecond latency during peak hours.

Which solution will meet these requirements with the LEAST operational overhead?

A. Use Amazon S3 to host the full website in different S3 buckets. Add Amazon CloudFront distributions. Set the S3 buckets as origins for the distributions. Store the order data in Amazon S3.

B. Deploy the full website on Amazon EC2 instances that run in Auto Scaling groups across multiple Availability Zones. Add an Application Load Balancer (ALB) to distribute the website traffic. Add another ALB for the backend APIs. Store the data in Amazon RDS for MySQL.

C. Migrate the full application to run in containers. Host the containers on Amazon Elastic Kubernetes Service (Amazon EKS). Use the Kubernetes Cluster Autoscaler to increase and decrease the number of pods to process bursts in traffic. Store the data in Amazon RDS for MySQL.

D. Use an Amazon S3 bucket to host the website's static content. Deploy an Amazon CloudFront distribution. Set the S3 bucket as the origin. Use Amazon API Gateway and AWS Lambda functions for the backend APIs. Store the data in Amazon DynamoDB.

## Question #21 분석

### ✅ 요구사항
- **하루 한 상품 특가 웹사이트** (단순한 비즈니스 모델)
- **시간당 수백만 요청** 처리 필요
- **밀리초 단위 지연 시간** (매우 낮은 latency)
- **최소한의 운영 오버헤드** 필요

### ✅ 선택지 분석

**A. S3 + CloudFront + S3 저장**
- **정적 콘텐츠**: S3+CloudFront 최적 ✅
- **주문 데이터를 S3 저장**: 트랜잭션 처리 부적합 ❌
- S3는 ACID 트랜잭션 미지원
- 동시 주문 처리에서 데이터 일관성 문제

**B. EC2 + Auto Scaling + ALB + RDS**
- **높은 운영 오버헤드**: 서버 관리 필요 ❌
- EC2 패치, 확장, 모니터링 필요
- RDS 용량 계획 및 관리
- 인프라 복잡성 증가

**C. EKS + Kubernetes + RDS**
- **매우 높은 운영 오버헤드**: K8s 클러스터 관리 ❌
- 컨테이너 오케스트레이션 복잡성
- RDS 관리 추가 필요
- 과도하게 복잡한 아키텍처

**D. S3 + CloudFront + API Gateway + Lambda + DynamoDB** ⭐
- **완전 서버리스**: 운영 오버헤드 최소화 ✅
- **자동 확장**: 수백만 요청 자동 처리 ✅
- **밀리초 지연**: CloudFront + DynamoDB ✅
- **관리형 서비스들**: 패치/확장 자동화 ✅

### 📋 서버리스 vs 서버 기반 비교

### **서버리스 아키텍처 (D번)**
```yaml
정적 콘텐츠:
  - S3: 무제한 확장성
  - CloudFront: 전 세계 엣지 캐싱
  - 지연시간: <10ms

동적 API:
  - API Gateway: 자동 스케일링
  - Lambda: 밀리초 단위 콜드스타트
  - 동시 실행: 1000개 → 무제한 확장

데이터 저장:
  - DynamoDB: 한 자릿수 밀리초 지연
  - 자동 확장: RCU/WCU 무제한
  - 글로벌 복제 지원
```

### **서버 기반 아키텍처 (B, C번)**
```yaml
확장성 제한:
  - EC2: 수동 또는 예측 기반 확장
  - RDS: 읽기 복제본 수동 관리
  - 스케일 아웃 지연 시간 존재

운영 부담:
  - 서버 패치 및 보안 관리
  - 용량 계획 및 비용 최적화
  - 모니터링 및 알람 설정
  - 장애 대응 및 복구
```

### 🔄 트래픽 패턴 분석

### **하루 한 상품 특가 특성**
```yaml
트래픽 패턴:
  - 오전 9시: 특가 시작 → 급격한 트래픽 증가
  - 오전 9-11시: 최대 트래픽 (수백만/시간)
  - 오후: 점진적 감소
  - 자정: 특가 종료 → 트래픽 급감

요구사항:
  - 순간적 스케일링 필요
  - 예측 불가능한 폭발적 증가
  - 비용 효율성 (트래픽 없을 때 최소 비용)
```

### **서버리스 vs 서버 기반 대응**
```yaml
서버리스 (Lambda + DynamoDB):
  ✅ 0→100만 요청 즉시 확장
  ✅ 사용량 기반 과금 (유휴 시간 비용 0)
  ✅ 자동 장애 복구
  ✅ 글로벌 분산 처리

서버 기반 (EC2 + RDS):
  ❌ Auto Scaling 지연 (2-5분)
  ❌ 고정 비용 (24시간 운영)
  ❌ 용량 계획 필요
  ❌ 단일 리전 제한
```

### 📊 성능 및 지연 시간 비교

### **밀리초 지연시간 달성 방법**
```yaml
CloudFront + S3:
  - 정적 콘텐츠: 엣지 캐시에서 즉시 응답
  - 지연시간: 5-20ms
  - 캐시 적중률: 95%+ 가능

API Gateway + Lambda:
  - 웜 람다: 1-10ms 응답
  - 콜드 스타트: 100-500ms (최초 1회만)
  - Provisioned Concurrency로 콜드 스타트 제거

DynamoDB:
  - 읽기: 1-2ms
  - 쓰기: 2-5ms
  - DAX 캐시 사용 시: <1ms
```

### **RDS vs DynamoDB 성능**
```yaml
RDS MySQL:
  - 읽기: 5-50ms (쿼리 복잡도에 따라)
  - 쓰기: 10-100ms
  - 연결 풀 관리 필요
  - 동시 연결 제한

DynamoDB:
  - 읽기: 1-2ms (일관적)
  - 쓰기: 2-5ms (일관적)
  - 연결 관리 불필요
  - 무제한 동시 요청
```

### 🔄 실제 워크플로우 비교

### **주문 처리 시나리오 (D번 서버리스)**
```
1. 사용자 → CloudFront (정적 페이지)
   지연: 10ms (엣지 캐시)

2. 주문 버튼 클릭 → API Gateway → Lambda
   지연: 5ms (웜 람다)

3. Lambda → DynamoDB 주문 저장
   지연: 2ms (단일 테이블 설계)

4. 응답 → 사용자
   총 지연: ~17ms ✅
```

### **주문 처리 시나리오 (B번 EC2+RDS)**
```
1. 사용자 → ALB → EC2
   지연: 20ms (로드밸런싱 + 앱 처리)

2. EC2 → RDS 주문 저장
   지연: 30ms (SQL 쿼리 + 네트워크)

3. 응답 → 사용자
   총 지연: ~50ms ❌
```
