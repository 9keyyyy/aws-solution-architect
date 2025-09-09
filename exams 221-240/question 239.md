## Question #239
A solutions architect needs to design a new microservice for a company’s application. 
Clients must be able to call an HTTPS endpoint to reach the microservice. 
The microservice also must use AWS Identity and Access Management (IAM) to authenticate calls. 
The solutions architect will write the logic for this microservice by using a single AWS Lambda function that is written in Go 1.x.

Which solution will deploy the function in the MOST operationally efficient way?

A. Create an Amazon API Gateway REST API. Configure the method to use the Lambda function. Enable IAM authentication on the API.

B. Create a Lambda function URL for the function. Specify AWS_IAM as the authentication type.

C. Create an Amazon CloudFront distribution. Deploy the function to Lambda@Edge. Integrate IAM authentication logic into the Lambda@Edge function.

D. Create an Amazon CloudFront distribution. Deploy the function to CloudFront Functions. Specify AWS_IAM as the authentication type.

## Question #239 분석

### 요구사항
- **마이크로서비스** HTTPS 엔드포인트
- **IAM 인증** 필요
- **Go 1.x Lambda 함수** 사용
- **최고 운영 효율성**

### 선택지 분석

**A. API Gateway REST API + Lambda + IAM 인증**
- **완전한 기능**: HTTPS 엔드포인트 + IAM 인증 지원 ✅
- **운영 복잡성**: API Gateway 설정 및 관리 필요 ❌
- **추가 비용**: API Gateway 요청당 비용 발생 ❌

**B. Lambda Function URL + AWS_IAM 인증** ⭐
- **직접 HTTPS 엔드포인트**: Lambda 함수가 직접 웹 엔드포인트 제공 ✅
- **네이티브 IAM 지원**: AWS_IAM 인증 타입 내장 ✅
- **최소 복잡성**: 추가 서비스 없이 Lambda만으로 완성 ✅
- **비용 효율**: API Gateway 비용 없이 Lambda 비용만 ✅

**C. CloudFront + Lambda@Edge + IAM 로직**
- **과도한 복잡성**: 글로벌 배포가 불필요한 마이크로서비스에 오버킬 ❌
- **커스텀 인증**: IAM 로직을 직접 구현해야 함 ❌
- **운영 부담**: Edge 배포 관리 복잡 ❌

**D. CloudFront + CloudFront Functions + AWS_IAM**
- **기능 제한**: CloudFront Functions는 Go 미지원 ❌
- **IAM 지원 없음**: CloudFront Functions는 AWS_IAM 인증 미지원 ❌

### Lambda Function URL 장점

```yaml
운영 효율성:
- 설정 한 번으로 HTTPS 엔드포인트 생성
- IAM 인증 자동 처리
- 추가 서비스 관리 불필요
- 직접적인 함수 호출

비용 효율성:
- API Gateway 비용 제거
- Lambda 실행 비용만 발생
- 간단한 요금 구조
```
