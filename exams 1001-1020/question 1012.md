## Question #1012
A company has a three-tier web application that processes orders from customers. The web tier consists of Amazon EC2 instances behind an Application Load Balancer. The processing tier consists of EC2 instances. The company decoupled the web tier and processing tier by using Amazon Simple Queue Service (Amazon SQS). The storage layer uses Amazon DynamoDB.

At peak times, some users report order processing delays and halls. The company has noticed that during these delays, the EC2 instances are running at 100% CPU usage, and the SQS queue fills up. The peak times are variable and unpredictable.

The company needs to improve the performance of the application.

Which solution will meet these requirements?

A. Use scheduled scaling for Amazon EC2 Auto Scaling to scale out the processing tier instances for the duration of peak usage times. Use the CPU Utilization metric to determine when to scale.

B. Use Amazon ElastiCache for Redis in front of the DynamoDB backend tier. Use target utilization as a metric to determine when to scale.

C. Add an Amazon CloudFront distribution to cache the responses for the web tier. Use HTTP latency as a metric to determine when to scale.

D. Use an Amazon EC2 Auto Scaling target tracking policy to scale out the processing tier instances. Use the ApproximateNumberOfMessages attribute to determine when to scale.

## Question #1012 분석

### 문제 상황
- **처리 계층 EC2**가 100% CPU 사용률
- **SQS 큐가 가득 참**
- **피크 시간이 예측 불가능**
- 주문 처리 지연 발생

### 핵심 요구사항
- **처리 용량 확장**
- **예측 불가능한 피크 대응**
- **자동 스케일링**

### 선택지 분석

**A. Scheduled Scaling + CPU 메트릭**
- 예측 불가능한 피크 → Scheduled Scaling 부적합 ❌
- CPU는 적절한 메트릭이지만 스케일링 방식이 잘못됨

**B. ElastiCache + Target Utilization**
- 문제는 처리 계층 용량 부족인데 캐싱으로 해결 안됨 ❌
- DynamoDB 성능 문제가 아님

**C. CloudFront + HTTP Latency**
- 웹 계층 캐싱은 백엔드 처리 부하와 무관 ❌
- SQS 큐 문제 해결 안됨

**D. Target Tracking + SQS 메트릭** ⭐
- SQS 큐 메시지 수로 스케일링 ✅
- 예측 불가능한 피크에 자동 대응 ✅
- 직접적인 문제 해결 ✅

