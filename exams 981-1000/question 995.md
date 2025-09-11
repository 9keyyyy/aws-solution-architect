## Question #995
A streaming media company is rebuilding its infrastructure to accommodate increasing demand for video content that users consume daily.

The company needs to process terabyte-sized videos to block some content in the videos. Video processing can take up to 20 minutes.

The company needs a solution that will scale with demand and remain cost-effective.

Which solution will meet these requirements?

A. Use AWS Lambda functions to process videos. Store video metadata in Amazon DynamoDB. Store video content in Amazon S3 Intelligent-Tiering.

B. Use Amazon Elastic Container Service (Amazon ECS) and AWS Fargate to implement microservices to process videos. Store video metadata in Amazon Aurora. Store video content in Amazon S3 Intelligent-Tiering.

C. Use Amazon EC2 instances in an Auto Scaling group behind an Application Load Balancer (ALB) to process videos. Store video content in Amazon S3 Standard. Use Amazon Simple Queue Service (Amazon SQS) for queuing and to decouple processing tasks.

D. Deploy a containerized video processing application on Amazon Elastic Kubernetes Service (Amazon EKS) on Amazon EC2. Store video metadata in Amazon RDS in a single Availability Zone. Store video content in Amazon S3 Glacier Deep Archive.

## Question #995 분석

### 문제 상황
- **스트리밍 미디어 회사** 인프라 재구축
- **테라바이트급 비디오** 처리
- **비디오 처리 시간: 최대 20분**
- **수요에 따른 확장** 필요
- **비용 효율성** 중요

### 핵심 요구사항
- **대용량 비디오 처리**
- **20분 처리 시간 지원**
- **확장성**
- **비용 효율성**

### 선택지 분석

**A. Lambda + DynamoDB + S3 Intelligent-Tiering**
- Lambda: 15분 제한 → 20분 처리 불가능 ❌
- 테라바이트급 처리에 부적합 ❌

**B. ECS + Fargate + Aurora + S3 Intelligent-Tiering** ⭐
- ECS/Fargate: 서버리스, 자동 스케일링 ✅
- 20분+ 처리 가능 ✅
- Aurora: 관리형 DB ✅
- S3 Intelligent-Tiering: 비용 최적화 ✅

**C. EC2 + Auto Scaling + ALB + SQS**
- EC2 관리 부담 ❌
- ALB: 비디오 처리에 불필요 ❌
- 비용 효율성 제한적 ❌

**D. EKS + EC2 + RDS + Glacier Deep Archive**
- EKS on EC2: 관리 복잡성 ❌
- Single AZ RDS: 고가용성 부족 ❌
- Glacier Deep Archive: 접근시 시간 지연 ❌

### 처리 시간 제약

**Lambda 제한**: 15분 최대 → 20분 처리 불가
**컨테이너 서비스**: 시간 제한 없음

### 비용 효율성 비교

```yaml
ECS + Fargate:
- 서버리스 (사용시만 과금)
- 자동 스케일링
- 관리 부담 최소

EC2 기반:
- 24/7 인스턴스 비용
- 관리 오버헤드
- 스케일링 복잡성
```

### 스토리지 선택

**S3 Intelligent-Tiering**
```yaml
장점:
- 액세스 패턴에 따른 자동 계층 이동
- 비용 최적화
- 즉시 액세스 가능
```

**Glacier Deep Archive**
```yaml
단점:
- 12시간 복원 시간
- 실시간 처리에 부적합
```
