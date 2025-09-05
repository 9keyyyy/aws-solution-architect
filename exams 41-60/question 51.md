## Question #51
A company is developing an application that provides order shipping statistics for retrieval by a REST API. 
The company wants to extract the shipping statistics, organize the data into an easy-to-read HTML format, and send the report to several email addresses at the same time every morning.

Which combination of steps should a solutions architect take to meet these requirements? (Choose two.)

A. Configure the application to send the data to Amazon Kinesis Data Firehose.

B. Use Amazon Simple Email Service (Amazon SES) to format the data and to send the report by email.

C. Create an Amazon EventBridge (Amazon CloudWatch Events) scheduled event that invokes an AWS Glue job to query the application's API for the data.

D. Create an Amazon EventBridge (Amazon CloudWatch Events) scheduled event that invokes an AWS Lambda function to query the application's API for the data.

E. Store the application data in Amazon S3. Create an Amazon Simple Notification Service (Amazon SNS) topic as an S3 event destination to send the report by email.

## Question #51 분석 (2개 선택)

### ✅ 요구사항
- REST API로 **배송 통계 데이터 추출**
- **HTML 형식으로 정리**
- **매일 아침 같은 시간** 여러 이메일 주소로 발송
- **2개 조합** 선택

### ✅ 선택지 재분석

**A. Kinesis Data Firehose**
- **실시간 스트리밍**: 일일 보고서에 부적합 

**B. Amazon SES로 형식화 및 이메일 발송**
- **이메일 발송**: 최종 단계에 필요 ✅
- **HTML 이메일 지원**: 형식화된 보고서 발송 ✅
- **다중 수신자**: 여러 이메일 주소 동시 발송 ✅

**C. EventBridge + AWS Glue**
- **과도한 ETL**: 단순 API 호출에 부적합 

**D. EventBridge + Lambda** ✅
- **스케줄링**: 매일 아침 자동 실행 ✅
- **API 호출 및 데이터 처리**: 핵심 로직 ✅
- **HTML 변환**: Lambda에서 처리 가능 ✅

**E. S3 + SNS**
- **스케줄링 없음**: 요구사항 미충족 

### 📋 D + B 조합의 워크플로우

### **완전한 솔루션 아키텍처**
```yaml
1. EventBridge 스케줄 이벤트 (매일 오전 9시)
   ↓
2. Lambda 함수 실행
   - REST API 호출
   - 데이터 추출 및 HTML 변환
   ↓
3. SES를 통한 이메일 발송
   - HTML 형식 이메일
   - 여러 수신자 동시 발송
```

### **각 서비스의 역할 분담**
```yaml
EventBridge + Lambda (D번):
  ✅ 스케줄링 및 데이터 처리
  ✅ API 호출 및 HTML 변환
  ✅ 전체 워크플로우 오케스트레이션

Amazon SES (B번):
  ✅ HTML 이메일 발송
  ✅ 다중 수신자 관리
  ✅ 신뢰할 수 있는 이메일 전송
```
