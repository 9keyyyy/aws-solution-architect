## Question #10
A company is building an ecommerce web application on AWS. 
The application sends information about new orders to an Amazon API Gateway REST API to process. 
The company wants to ensure that orders are processed in the order that they are received.

Which solution will meet these requirements?

A. Use an API Gateway integration to publish a message to an Amazon Simple Notification Service (Amazon SNS) topic when the application receives an order. Subscribe an AWS Lambda function to the topic to perform processing.

B. Use an API Gateway integration to send a message to an Amazon Simple Queue Service (Amazon SQS) FIFO queue when the application receives an order. Configure the SQS FIFO queue to invoke an AWS Lambda function for processing.

C. Use an API Gateway authorizer to block any requests while the application processes an order.

D. Use an API Gateway integration to send a message to an Amazon Simple Queue Service (Amazon SQS) standard queue when the application receives an order. Configure the SQS standard queue to invoke an AWS Lambda function for processing.



### **✅ 요구사항:**
* 이커머스 웹 애플리케이션이 AWS에서 구축됨
* 새로운 주문 정보를 API Gateway REST API로 전송
* **주문이 수신된 순서대로 처리되어야 함** (핵심 요구사항)

### **✅ 선택지 분석**

**A. API Gateway → SNS Topic → Lambda 구독**
* SNS는 **Pub/Sub 메시징** - 순서 보장 없음 ❌
* 여러 Lambda 함수가 동시에 메시지 받을 수 있음
* **메시지 순서 처리 불가능**
* 팬아웃 패턴에는 좋지만 순차 처리에는 부적합

**B. API Gateway → SQS FIFO 큐 → Lambda 처리** ⭐
* **FIFO (First In, First Out)** - 순서 보장 ✅
* 메시지가 **정확히 수신된 순서대로** 처리됨
* **중복 메시지 방지** (정확히 한 번 처리)
* 순차 처리 요구사항에 완벽하게 부합

**C. API Gateway authorizer로 처리 중 요청 차단**
* **동기식 블로킹** - 확장성 없음 ❌
* 하나씩 순차 처리하지만 **성능 매우 저하**
* 동시 주문 처리 불가능
* 실제 운영 환경에서 비현실적

**D. API Gateway → SQS Standard 큐 → Lambda 처리**  
* Standard SQS는 **순서 보장 없음** ❌
* **Best Effort Ordering** - 대부분 순서대로 오지만 보장 안됨
* 높은 처리량이지만 순서가 뒤바뀔 가능성 존재

### **✅ 개념 정리**


### **1. SQS FIFO vs Standard 비교 (B번 vs D번)**

**1.1 SQS Standard 큐 (D번)**
```
특징:
✅ 무제한 처리량 (초당 수만 메시지)
✅ 높은 성능
❌ 순서 보장 없음 (Best Effort)
❌ 메시지 중복 가능성

메시지 처리 순서 예시:
전송 순서: [주문1, 주문2, 주문3, 주문4, 주문5]
처리 순서: [주문1, 주문3, 주문2, 주문5, 주문4] ❌
```

**1.2 SQS FIFO 큐 (B번)**
```
특징:
✅ 완벽한 순서 보장 (FIFO)
✅ 정확히 한 번 처리 (중복 방지)
✅ 순차 처리 보장
⚠️ 처리량 제한 (초당 3,000개 메시지, 배치시 30,000개)

메시지 처리 순서:
전송 순서: [주문1, 주문2, 주문3, 주문4, 주문5]
처리 순서: [주문1, 주문2, 주문3, 주문4, 주문5] ✅
```

**1.3 성능 비교**

| 특징 | Standard SQS | FIFO SQS |
|------|-------------|----------|
| **처리량** | 무제한 | 초당 3,000개 (배치: 30,000개) |
| **순서 보장** | ❌ | ✅ |
| **중복 방지** | ❌ | ✅ |
| **지연 시간** | 낮음 | 약간 높음 |
| **비용** | 낮음 | 동일 |

### **2. SNS의 한계 (A번 분석)**

**2.1 SNS 메시지 전달 방식**
```
API Gateway → SNS Topic → Lambda 1 (동시 실행)
                        → Lambda 2 (동시 실행)  
                        → Lambda 3 (동시 실행)

문제점:
- 여러 Lambda가 동시에 메시지 받음
- 어떤 Lambda가 먼저 처리할지 예측 불가
- 순서 제어 불가능
```

**2.2 SNS 적합한 사용 사례**
```
✅ 좋은 사용 사례:
- 알림 발송 (이메일, SMS, 푸시)
- 팬아웃 패턴 (하나의 이벤트를 여러 서비스에 전달)
- 이벤트 기반 아키텍처

❌ 부적합한 사용 사례:  
- 순차 처리가 필요한 작업
- 메시지 순서가 중요한 비즈니스 로직
```
