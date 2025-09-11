## Question #1001
A company operates a food delivery service. Because of recent growth, the company's order processing system is experiencing scaling problems during peak traffic hours. The current architecture includes Amazon EC2 instances in an Auto Scaling group that collect orders from an application. A second group of EC2 instances in an Auto Scaling group fulfills the orders.

The order collection process occurs quickly, but the order fulfillment process can take longer. Data must not be lost because of a scaling event.

A solutions architect must ensure that the order collection process and the order fulfillment process can both scale adequately during peak traffic hours.

Which solution will meet these requirements?

A. Use Amazon CloudWatch to monitor the CPUUtilization metric for each instance in both Auto Scaling groups. Configure each Auto Scaling group's minimum capacity to meet its peak workload value.

B. Use Amazon CloudWatch to monitor the CPUUtilization metric for each instance in both Auto Scaling groups. Configure a CloudWatch alarm to invoke an Amazon Simple Notification Service (Amazon SNS) topic to create additional Auto Scaling groups on demand.

C. Provision two Amazon Simple Queue Service (Amazon SQS) queues. Use one SQS queue for order collection. Use the second SQS queue for order fulfillment. Configure the EC2 instances to poll their respective queues. Scale the Auto Scaling groups based on notifications that the queues send.

D. Provision two Amazon Simple Queue Service (Amazon SQS) queues. Use one SQS queue for order collection. Use the second SQS queue for order fulfillment. Configure the EC2 instances to poll their respective queues. Scale the Auto Scaling groups based on the number of messages in each queue.

## Question #1001 분석

### 문제 상황
- **음식 배달 서비스** 급성장
- **주문 수집** vs **주문 처리** 속도 차이
- **피크 시간 확장 문제**
- **데이터 손실 방지** 필요

### 핵심 요구사항
- **두 프로세스 독립적 확장**
- **데이터 손실 방지**
- **피크 트래픽 대응**

### 아키텍처 문제점
현재: 직접 연결 → 처리 속도 차이로 인한 병목

### 선택지 분석

**A. CPU 기반 스케일링 + 최소 용량 설정**
- CPU만으로는 대기열 상황 파악 불가 ❌
- 최소 용량 고정 → 비용 비효율 ❌

**B. CPU 기반 + SNS로 추가 Auto Scaling 그룹**
- 복잡하고 비효율적 ❌
- 근본적 해결책 아님 ❌

**C. SQS + 큐 알림 기반 스케일링**
- "큐가 보내는 알림" → 부정확한 표현 ❌
- SQS는 알림을 "보내지" 않음 ❌

**D. SQS + 큐 메시지 수 기반 스케일링** ⭐
- SQS로 디커플링 → 데이터 손실 방지 ✅
- 메시지 수 기반 정확한 스케일링 ✅
- 두 프로세스 독립적 확장 ✅

### SQS 기반 아키텍처 장점

```yaml
디커플링:
- 주문 수집과 처리 분리
- 속도 차이 흡수
- 장애 격리

데이터 보호:
- 메시지 지속성
- 처리 실패시 재시도
- 스케일링 중 데이터 손실 방지

스케일링:
- ApproximateNumberOfMessages 메트릭
- 실제 부하 기반 정확한 확장
```

### 메트릭 기반 스케일링

```yaml
CPU 기반 문제점:
- 대기열 상황 반영 안됨
- 처리 대기중인 작업 파악 불가

SQS 메시지 수 기반:
- 실제 대기 작업량 반영
- 정확한 확장 타이밍
```
