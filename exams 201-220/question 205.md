## Question #205
A company hosts a marketing website in an on-premises data center. 
The website consists of static documents and runs on a single server. 
An administrator updates the website content infrequently and uses an SFTP client to upload new documents.

The company decides to host its website on AWS and to use Amazon CloudFront. 
The company’s solutions architect creates a CloudFront distribution. 
The solutions architect must design the most cost-effective and resilient architecture for website hosting to serve as the CloudFront origin.

Which solution will meet these requirements?

A. Create a virtual server by using Amazon Lightsail. Configure the web server in the Lightsail instance. Upload website content by using an SFTP client.

B. Create an AWS Auto Scaling group for Amazon EC2 instances. Use an Application Load Balancer. Upload website content by using an SFTP client.

C. Create a private Amazon S3 bucket. Use an S3 bucket policy to allow access from a CloudFront origin access identity (OAI). Upload website content by using the AWS CLI.

D. Create a public Amazon S3 bucket. Configure AWS Transfer for SFTP. Configure the S3 bucket for website hosting. Upload website content by using the SFTP client.

## Question #205 분석

### 요구사항
- **정적 웹사이트** 호스팅 (문서 기반)
- **CloudFront 오리진** 구성
- **가끔 업데이트** (SFTP 선호)
- **최고 비용 효율성**
- **복원력** 확보

### 선택지 분석

**A. Amazon Lightsail**
- **서버 관리**: 가상 서버 관리 부담 ❌
- **복원력 부족**: 단일 인스턴스, 장애 위험 ❌
- **비용**: 정적 사이트 대비 과도한 서버 비용 ❌

**B. Auto Scaling + ALB**
- **과도한 설계**: 정적 사이트에 불필요한 복잡성 ❌
- **높은 비용**: EC2 + ALB 운영 비용 ❌
- **관리 부담**: 서버 패치, 스케일링 설정 ❌

**C. 프라이빗 S3 + OAI + AWS CLI** ⭐
- **정적 호스팅 최적**: S3는 정적 사이트 전용 설계 ✅
- **최저 비용**: 스토리지 비용만, 서버 비용 없음 ✅
- **최고 복원력**: 99.999999999% 내구성 ✅
- **보안**: OAI로 CloudFront만 접근 허용 ✅
- **업로드 방식 변경**: SFTP → AWS CLI (학습 필요) ⚠️

**D. 퍼블릭 S3 + Transfer for SFTP**
- **보안 위험**: 퍼블릭 버킷으로 직접 접근 가능 ❌
- **추가 비용**: Transfer for SFTP 서비스 비용 ❌
- **불필요한 복잡성**: SFTP 서비스 운영 부담 ❌


