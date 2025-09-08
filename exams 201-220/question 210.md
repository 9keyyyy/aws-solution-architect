## Question #210
A company offers a food delivery service that is growing rapidly. 
Because of the growth, the company’s order processing system is experiencing scaling problems during peak traffic hours. 
The current architecture includes the following:

• A group of Amazon EC2 instances that run in an Amazon EC2 Auto Scaling group to collect orders from the application

• Another group of EC2 instances that run in an Amazon EC2 Auto Scaling group to fulfill orders

The order collection process occurs quickly, but the order fulfillment process can take longer. 
Data must not be lost because of a scaling event.

A solutions architect must ensure that the order collection process and the order fulfillment process can both scale properly during peak traffic hours. 
The solution must optimize utilization of the company’s AWS resources.

Which solution meets these requirements?

A. Use Amazon CloudWatch metrics to monitor the CPU of each instance in the Auto Scaling groups. Configure each Auto Scaling group’s minimum capacity according to peak workload values.

B. Use Amazon CloudWatch metrics to monitor the CPU of each instance in the Auto Scaling groups. Configure a CloudWatch alarm to invoke an Amazon Simple Notification Service (Amazon SNS) topic that creates additional Auto Scaling groups on demand.

C. Provision two Amazon Simple Queue Service (Amazon SQS) queues: one for order collection and another for order fulfillment. Configure the EC2 instances to poll their respective queue. Scale the Auto Scaling groups based on notifications that the queues send.

D. Provision two Amazon Simple Queue Service (Amazon SQS) queues: one for order collection and another for order fulfillment. Configure the EC2 instances to poll their respective queue. Create a metric based on a backlog per instance calculation. Scale the Auto Scaling groups based on this metric.

## Question #210 분석

### 요구사항
- **음식 배달 서비스** 급성장
- **주문 수집**: 빠른 처리
- **주문 이행**: 긴 처리 시간
- **피크 시간대** 확장 문제
- **데이터 손실 방지** 필수
- **리소스 최적화**

### 핵심 문제
```yaml
현재 아키텍처:
- 주문 수집 Auto Scaling Group
- 주문 이행 Auto Scaling Group
- 직접 연결로 처리 속도 불일치

문제점:
- 처리 속도 차이로 병목 발생
- 스케일링 시 데이터 손실 위험
```

### 선택지 분석

**A. CloudWatch CPU 메트릭 + 최소 용량 설정**
- **비효율적**: 피크 기준 상시 운영으로 비용 증가 ❌
- **리소스 낭비**: 비피크 시간 불필요한 인스턴스 ❌

**B. CloudWatch CPU + SNS + 추가 Auto Scaling Group**
- **복잡성**: 불필요한 아키텍처 복잡화 ❌
- **근본 해결 실패**: 데이터 손실 문제 미해결 ❌

**C. SQS 큐 + 큐 알림 기반 확장**
- **큐 디커플링**: 주문 수집과 이행 분리 ✅
- **데이터 보존**: SQS가 메시지 지속성 보장 ✅
- **확장 방식 부정확**: "큐 알림" 방식 모호 ❌

**D. SQS 큐 + 백로그 per 인스턴스 메트릭** ⭐
- **완벽한 디커플링**: 두 프로세스 완전 분리 ✅
- **데이터 손실 방지**: SQS 메시지 지속성 ✅
- **정확한 확장**: 실제 작업 부하 기반 확장 ✅
- **리소스 최적화**: 필요한 만큼만 확장 ✅

### D안 아키텍처

```yaml
주문 수집:
사용자 → 수집 EC2 → Order Collection SQS

주문 이행:  
Order Collection SQS → 이행 EC2 → Order Fulfillment SQS

확장 메트릭:
백로그 per 인스턴스 = 큐 메시지 수 / 활성 인스턴스 수
```

### 백로그 per 인스턴스 계산

```yaml
예시:
- 대기 메시지: 1000개
- 활성 인스턴스: 5개
- 백로그 per 인스턴스: 200개

확장 조건:
- 200개 초과 시: 인스턴스 추가
- 50개 미만 시: 인스턴스 감소
```