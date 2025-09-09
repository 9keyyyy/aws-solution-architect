## Question #255
A company has an ecommerce checkout workflow that writes an order to a database and calls a service to process the payment. 
Users are experiencing timeouts during the checkout process. 
When users resubmit the checkout form, multiple unique orders are created for the same desired transaction.

How should a solutions architect refactor this workflow to prevent the creation of multiple orders?

A. Configure the web application to send an order message to Amazon Kinesis Data Firehose. Set the payment service to retrieve the message from Kinesis Data Firehose and process the order.

B. Create a rule in AWS CloudTrail to invoke an AWS Lambda function based on the logged application path request. Use Lambda to query the database, call the payment service, and pass in the order information.

C. Store the order in the database. Send a message that includes the order number to Amazon Simple Notification Service (Amazon SNS). Set the payment service to poll Amazon SNS, retrieve the message, and process the order.

D. Store the order in the database. Send a message that includes the order number to an Amazon Simple Queue Service (Amazon SQS) FIFO queue. Set the payment service to retrieve the message and process the order. Delete the message from the queue.

## Question #255 분석

### 문제 상황
- **이커머스 체크아웃 워크플로우** (주문 DB 저장 + 결제 처리)
- **타임아웃 발생** 중 사용자 재제출
- **중복 주문 생성** 문제

### 핵심 요구사항
- **중복 주문 방지**
- **안정적인 결제 처리**
- **타임아웃 문제 해결**

### 선택지 분석

**A. Kinesis Data Firehose + 메시지 검색**
- **실시간 스트리밍 오버킬**: 단순 주문 처리에 부적절 ❌
- **중복 방지 메커니즘 없음**: 메시지 중복 처리 위험 ❌
- **검색 방식**: Firehose는 실시간 검색용이 아님 ❌

**B. CloudTrail + Lambda**
- **용도 불일치**: CloudTrail은 API 감사용, 워크플로우 처리용 아님 ❌
- **성능 문제**: 로그 기반 처리로 지연 발생 ❌
- **중복 방지 부족**: 동일한 재제출 문제 지속 ❌

**C. SNS + 폴링**
- **중복 전송**: SNS는 메시지 중복 전송 가능 ❌
- **팬아웃 특성**: 여러 구독자에게 동일 메시지 전송 위험 ❌
- **메시지 순서 보장 없음**: 주문 처리 순서 문제 ❌

**D. SQS FIFO + 메시지 삭제** ⭐
- **중복 방지**: FIFO 큐의 중복 제거 기능 ✅
- **순서 보장**: 메시지 처리 순서 보장 ✅
- **정확히 한 번 처리**: 메시지 삭제로 재처리 방지 ✅
- **디커플링**: DB 저장과 결제 처리 분리로 타임아웃 해결 ✅

### SQS FIFO 중복 방지 메커니즘

```yaml
워크플로우:
1. 주문을 DB에 저장
2. 주문 번호를 SQS FIFO에 전송 (MessageDeduplicationId 사용)
3. 결제 서비스가 큐에서 메시지 검색
4. 결제 처리 완료 후 메시지 삭제

중복 방지:
- 같은 MessageDeduplicationId로 중복 전송 시 자동 제거
- 메시지 처리 후 삭제로 재처리 방지
- 정확히 한 번 처리 보장
```
