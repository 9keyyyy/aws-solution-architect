## Question #1005
A company tracks customer satisfaction by using surveys that the company hosts on its website. The surveys sometimes reach thousands of customers every hour. Survey results are currently sent in email messages to the company so company employees can manually review results and assess customer sentiment.

The company wants to automate the customer survey process. Survey results must be available for the previous 12 months.

Which solution will meet these requirements in the MOST scalable way?

A. Send the survey results data to an Amazon API Gateway endpoint that is connected to an Amazon Simple Queue Service (Amazon SQS) queue. Create an AWS Lambda function to poll the SQS queue, call Amazon Comprehend for sentiment analysis, and save the results to an Amazon DynamoDB table. Set the TTL for all records to 365 days in the future.

B. Send the survey results data to an API that is running on an Amazon EC2 instance. Configure the API to store the survey results as a new record in an Amazon DynamoDB table, call Amazon Comprehend for sentiment analysis, and save the results in a second DynamoDB table. Set the TTL for all records to 365 days in the future.

C. Write the survey results data to an Amazon S3 bucket. Use S3 Event Notifications to invoke an AWS Lambda function to read the data and call Amazon Rekognition for sentiment analysis. Store the sentiment analysis results in a second S3 bucket. Use S3 lifecycle policies on each bucket to expire objects after 365 days.

D. Send the survey results data to an Amazon API Gateway endpoint that is connected to an Amazon Simple Queue Service (Amazon SQS) queue. Configure the SQS queue to invoke an AWS Lambda function that calls Amazon Lex for sentiment analysis and saves the results to an Amazon DynamoDB table. Set the TTL for all records to 365 days in the future.

## Question #1005 분석

### 문제 상황
- **수천 명 고객 설문조사** (시간당)
- 현재 **수동 이메일 검토**
- **자동화** 필요
- **12개월 데이터 보관**

### 핵심 요구사항
- **확장성** (수천 명/시간)
- **감정 분석** 자동화
- **12개월 보관**
- **최대 확장성**

### 감정 분석 서비스

**Amazon Comprehend**: 텍스트 감정 분석 전용 ✅
**Amazon Rekognition**: 이미지/비디오 분석 ❌
**Amazon Lex**: 챗봇/음성 인식 ❌

### 선택지 분석

**A. API Gateway + SQS + Lambda + Comprehend + DynamoDB** ⭐
- 완전 서버리스 아키텍처 ✅
- SQS로 트래픽 버퍼링 ✅
- Comprehend로 올바른 감정 분석 ✅
- DynamoDB TTL로 자동 삭제 ✅

**B. EC2 API + DynamoDB + Comprehend**
- EC2: 서버 관리 필요 → 확장성 제약 ❌
- 수천 명/시간 처리에 부적합 ❌

**C. S3 + Lambda + Rekognition**
- Rekognition: 이미지 분석 서비스 (텍스트 감정 분석 부적합) ❌

**D. API Gateway + SQS + Lambda + Lex**
- Lex: 챗봇 서비스 (감정 분석 부적합) ❌

### 확장성 비교

```yaml
Option A (서버리스):
- API Gateway: 자동 스케일링
- SQS: 무제한 메시지 버퍼링
- Lambda: 동시 실행 자동 확장
- DynamoDB: 자동 스케일링

Option B (EC2):
- EC2: 수동 스케일링 필요
- 트래픽 급증시 병목
```
 DynamoDB로 구성된 완전 서버리스 아키텍처가 최대 확장성을 제공합니다.