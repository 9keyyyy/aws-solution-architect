## Security
### Security Group
Security Group 연결 가능 = VPC 내부 리소스
```
EC2, RDS, ALB/NLB, EFS, Lambda(VPC)
네트워크 인터페이스(ENI)가 있는 서비스
```
Security Group 연결 불가능 = VPC 외부 관리형 서비스
```
API Gateway, CloudFront, S3, DynamoDB
대안: Resource Policy, WAF, IAM Policy
```
<br>

### IAM
**서비스별 특화 방식**
- **EC2**: Instance Profile (IAM Role 연결)
- **Lambda**: Execution Role
- **EKS**: IRSA (IAM Role for Service Accounts)
- **ECS**: Task Role

<br>

### Global Accelerator
- Global Accelerator는 S3와 직접 연동 안됨
- S3 Transfer Acceleration 이용 필요

<br>

### AWS Batch vs Lambda 비교

| 구분 | AWS Batch | AWS Lambda |
|------|-----------|------------|
| **실행 시간** | 무제한 | 15분 제한 |
| **리소스** | 대용량 CPU/메모리 | 제한된 리소스 |
| **용도** | 배치/대용량 처리 | 짧은 이벤트 처리 |
| **관리** | 컨테이너 관리 | 완전 서버리스 |

<br>


### Amazon Cognito

- **User Pool**: 인증 (Authentication)
- **Identity Pool**: 권한 부여 (Authorization) → AWS 서비스 접근

<br>

### S3 Bucket Key

```yaml
기존 SSE-KMS:
각 객체마다 KMS 호출 → 높은 비용

S3 Bucket Key:
버킷 레벨에서 한 번만 KMS 호출 → 비용 99% 절약
동일한 암호화 강도 유지
```

<br>

### S3 File Gateway

- S3를 NFS/SMB로 노출
- 온프레미스 로컬 캐싱
- 자주 사용하는 데이터는 로컬에 유지
- 백그라운드에서 S3와 동기화

<br>

### Spot Instances vs Spot Fleet

**Spot Instance**
```yaml
정의: AWS 여분 용량을 할인가로 제공
할인율: On-Demand 대비 최대 90% 절약
리스크: 용량 부족시 2분 통지 후 중단
가격: 실시간 수급에 따라 변동
```


**Spot Fleet = 여러 Spot Instance 관리 도구**
```yaml
기능:
- 다양한 인스턴스 타입 조합
- 여러 가용 영역에 분산
- 목표 용량 자동 유지
- 가격/다양성 최적화

예시:
- t3.medium (AZ-a)
- t3.large (AZ-b)  
- m5.large (AZ-c)
```

<br>

### EMR vs Glue

| 구분 | EMR | Glue |
|------|-----|------|
| **관리** | 클러스터 관리 필요 | 서버리스 |
| **복잡도** | 높음 | 낮음 |
| **확장성** | 매우 높음 | 중간 |
| **비용** | 클러스터 운영비 | 사용량 기반 |


**대용량 복잡 처리 = EMR**, **간단 ETL + 관리 최소화 = Glue**

<br>

### 분석 서비스
- **Amazon Comprehend**: 텍스트 감정 분석 전용 
- **Amazon Rekognition**: 이미지/비디오 분석 
- **Amazon Lex**: 챗봇/음성 인식 

<br>

### CloudFront vs Global Accelerator

| 구분 | CloudFront | Global Accelerator |
|------|------------|-------------------|
| **용도** | 정적 콘텐츠 캐싱 | 동적 트래픽 가속 |
| **API** | 캐싱 제한적 | 최적화됨 |
| **비용** | 낮음 | 중간 |
| **계층** | Layer 7 (HTTP/HTTPS) | Layer 4 (TCP/UDP) |
| **캐싱** | 있음 | 없음 |
| **프로토콜** | HTTP/HTTPS만 | TCP/UDP |

<br>




### AWS Direct Connect (DC)

#### 핵심 개념
**Direct Connect = 온프레미스와 AWS 간 전용 네트워크 연결**

#### 특징
```yaml
연결 방식:
- 물리적 전용선 (인터넷 X)
- 1Gbps ~ 100Gbps 대역폭
- 일관된 네트워크 성능

비용:
- 초기 설치비 + 월 고정비
- 매우 비쌈 (수천~수만 달러/월)

설정 시간:
- 몇 주 ~ 몇 달 소요
```

<br>
