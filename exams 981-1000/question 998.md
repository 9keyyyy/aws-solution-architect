## Question #998
A company runs its legacy web application on AWS. The web application server runs on an Amazon EC2 instance in the public subnet of a VPC. The web application server collects images from customers and stores the image files in a locally attached Amazon Elastic Block Store (Amazon EBS) volume. The image files are uploaded every night to an Amazon S3 bucket for backup.

A solutions architect discovers that the image files are being uploaded to Amazon S3 through the public endpoint. The solutions architect needs to ensure that traffic to Amazon S3 does not use the public endpoint.

Which solution will meet these requirements?

A. Create a gateway VPC endpoint for the S3 bucket that has the necessary permissions for the VPC. Configure the subnet route table to use the gateway VPC endpoint.

B. Move the S3 bucket inside the VPC. Configure the subnet route table to access the S3 bucket through private IP addresses.

C. Create an Amazon S3 access point for the Amazon EC2 instance inside the VPConfigure the web application to upload by using the Amazon S3 access point.

D. Configure an AWS Direct Connect connection between the VPC that has the Amazon EC2 instance and Amazon S3 to provide a dedicated network path.

## Question #998 분석

### 문제 상황
- **Public subnet의 EC2**에서 웹 애플리케이션 실행
- 매일 밤 **S3로 이미지 백업**
- 현재 **S3 public endpoint** 사용 중
- **Public endpoint 사용 금지** 필요

### 핵심 요구사항
- **S3 트래픽이 public endpoint 거치지 않음**
- **VPC 내부 경로** 사용

### VPC에서 S3 접근 방법

**Public Endpoint**: 인터넷 경유 (현재 상황)
**VPC Endpoint**: AWS 내부망 경유 (목표)

### 선택지 분석

**A. Gateway VPC Endpoint + Route Table 설정** ⭐
- Gateway Endpoint: S3 전용 VPC 연결 ✅
- Route Table 설정으로 트래픽 라우팅 ✅
- AWS 내부망 사용 ✅
- 무료 ✅

**B. S3 버킷을 VPC 내부로 이동**
- S3는 글로벌 서비스, VPC 내부 이동 불가능 ❌
- 기술적으로 불가능 ❌

**C. S3 Access Point**
- Access Point: 버킷 접근 제어용 ❌
- 네트워크 경로 변경 기능 없음 ❌
- 여전히 public endpoint 사용 ❌

**D. Direct Connect**
- 온프레미스-AWS 연결용 ❌
- VPC 내부 EC2에는 부적합 ❌
- 과도하고 비용 비효율적 ❌

### VPC Endpoint 타입

**Gateway Endpoint**
```yaml
지원 서비스: S3, DynamoDB만
비용: 무료
구현: Route Table 수정
트래픽: AWS 내부망
```

**Interface Endpoint**
```yaml
지원 서비스: 대부분 AWS 서비스
비용: 시간당 요금
구현: ENI 생성
트래픽: AWS 내부망
```

### S3 Gateway Endpoint 구현

```yaml
설정:
1. VPC에 Gateway Endpoint 생성
2. S3 서비스 선택
3. Route Table에 S3 트래픽 라우팅 규칙 추가

결과:
- S3 API 호출이 VPC Endpoint 경유
- 인터넷 게이트웨이 거치지 않음
- AWS 내부망 사용
```
