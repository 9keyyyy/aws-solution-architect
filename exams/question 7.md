## Question #7
A company has an application that ingests incoming messages. 
Dozens of other applications and microservices then quickly consume these messages. 
The number of messages varies drastically and sometimes increases suddenly to 100,000 each second. 
The company wants to decouple the solution and increase scalability.

Which solution meets these requirements?

A. Persist the messages to Amazon Kinesis Data Analytics. 
   Configure the consumer applications to read and process the messages.

B. Deploy the ingestion application on Amazon EC2 instances in an Auto Scaling group 
   to scale the number of EC2 instances based on CPU metrics.

C. Write the messages to Amazon Kinesis Data Streams with a single shard. 
   Use an AWS Lambda function to preprocess messages and store them in Amazon DynamoDB. 
   Configure the consumer applications to read from DynamoDB to process the messages.

D. Publish the messages to an Amazon Simple Notification Service (Amazon SNS) topic 
   with multiple Amazon Simple Queue Service (Amazon SOS) subscriptions. 
   Configure the consumer applications to process the messages from the queues.



### **✅ 요구사항:**
* 애플리케이션이 들어오는 메시지를 수집(ingest)
* 수십 개의 애플리케이션과 마이크로서비스가 이 메시지들을 빠르게 소비
* 메시지 수가 급격히 변동 (때때로 초당 100,000개까지 급증)
* 솔루션의 디커플링과 확장성 향상 필요

### **✅ 선택지 분석**

**A. Amazon Kinesis Data Analytics에 메시지 저장**
* Kinesis Data Analytics는 **스트리밍 데이터 분석 서비스**
* 메시지 저장 및 큐잉 목적이 아닌 **실시간 분석 처리용**
* 여러 소비자 애플리케이션의 메시지 소비에 부적합
* 근본적으로 잘못된 서비스 선택

**B. Auto Scaling Group의 EC2 인스턴스에 ingestion 애플리케이션 배포**
* CPU 메트릭 기반 스케일링은 메시지 큐잉 문제 해결 안됨
* **디커플링 해결 미흡** - 생산자와 소비자가 여전히 직접 연결
* 갑작스러운 트래픽 급증 시 스케일링 지연 문제
* 메시지 손실 위험 존재

**C. 단일 샤드 Kinesis Data Streams 사용**
* **단일 샤드**: 초당 1,000개 레코드 또는 1MB 제한
* 요구사항 (초당 100,000개)에 **성능 부족**
* 여러 소비자를 위한 복잡한 DynamoDB 구조 필요
* 확장성 제약으로 요구사항 미충족

**D. SNS + 다중 SQS 구독을 통한 팬아웃 패턴** ⭐
* **완벽한 디커플링**: 생산자와 소비자 완전 분리
* **자동 확장**: 초당 100,000개 메시지 처리 가능
* **팬아웃 패턴**: 하나의 메시지를 여러 소비자에게 전달
* **관리형 서비스**: 운영 오버헤드 최소화

### **✅ 개념 정리**

### **1. Amazon SNS (Simple Notification Service)**

#### **1.1 핵심 특징**
* **Pub/Sub 메시징 서비스**
* 하나의 메시지를 여러 구독자에게 동시 전달
* 완전 관리형 서비스로 자동 확장
* **팬아웃 패턴**의 핵심 구성 요소

**1.2 SNS 작동 방식**
```
Message Producer → SNS Topic → Multiple Subscribers
                              ↓
                         SQS Queue 1 → Consumer App 1
                         SQS Queue 2 → Consumer App 2  
                         SQS Queue 3 → Consumer App 3
                         ...
```

**1.3 SNS 성능 특성**
* **처리량**: 초당 수만~수십만 메시지 처리 가능
* **지연 시간**: 밀리초 단위의 낮은 지연
* **내구성**: 여러 AZ에 자동 복제
* **비용**: 메시지당 과금 (매우 저렴)

### **2. Amazon SQS (Simple Queue Service)**

**2.1 큐 타입별 특성**

| 특징 | Standard SQS | FIFO SQS |
|------|-------------|----------|
| **처리량** | 무제한 | 초당 3,000개 (배치: 30,000개) |
| **순서 보장** | ❌ | ✅ |
| **중복 처리** | 가능성 있음 | ❌ |
| **지연 시간** | 낮음 | 약간 높음 |

**2.2 SQS의 확장성**
```
Standard SQS 확장성:
- 초당 수백만 메시지 처리 가능
- 자동 파티셔닝 및 복제
- Dead Letter Queue 지원
- Visibility Timeout으로 메시지 안전성 보장
```

### **3. SNS + SQS 팬아웃 패턴 (D번 솔루션)**

**3.1 아키텍처 구조**
```
Ingestion App → SNS Topic → SQS Queue 1 → Microservice 1
                         → SQS Queue 2 → Microservice 2
                         → SQS Queue 3 → Microservice 3
                         → ...
                         → SQS Queue N → Microservice N
```

**3.2 메시지 플로우**
```bash
# 1. 메시지 발행
aws sns publish --topic-arn arn:aws:sns:region:account:my-topic \
                --message "User action data: {user_id: 123, action: 'purchase'}"

# 2. 자동 팬아웃
# SNS가 모든 구독된 SQS 큐에 동일한 메시지 전달

# 3. 각 마이크로서비스에서 독립적 처리
# Analytics Service: 구매 분석
# Notification Service: 이메일/푸시 발송  
# Inventory Service: 재고 업데이트
# Billing Service: 결제 처리
```

**3.3 디커플링 효과**
```
Before (Tight Coupling):
Ingestion App → Microservice 1
              → Microservice 2  
              → Microservice 3
              ↑ 직접 연결 - 장애 전파 위험

After (Loose Coupling):  
Ingestion App → SNS → SQS → Microservices
              ↑ 완전 분리 - 독립적 처리
```

### **4. 잘못된 선택지들 심화 분석**

**4.1 A번: Kinesis Data Analytics 오해**
```
Kinesis Data Analytics 실제 용도:
✅ SQL 쿼리로 스트리밍 데이터 실시간 분석
✅ 윈도우 기반 집계 (시간별, 개수별)
✅ 이상 탐지 및 패턴 분석

❌ 메시지 큐잉 및 소비자 관리 (용도 불일치)
```

**4.2 B번: EC2 Auto Scaling의 한계**
```
문제점:
1. 스케일링 지연: CPU 메트릭 → 임계값 도달 → 인스턴스 시작 (5-10분)
2. 메시지 버퍼링 없음: 급증하는 메시지 손실 위험
3. 디커플링 미흡: 여전히 동기식 처리
4. 복잡성: 로드밸런서, 상태 관리 등 추가 구성 필요
```

**4.3 C번: 단일 샤드 Kinesis의 병목**
```
Kinesis 샤드 제한:
- 초당 1,000개 레코드 OR 1MB 입력
- 초당 2MB 또는 2,000개 레코드 출력

요구사항 vs 용량:
- 필요: 초당 100,000개 메시지
- 제공: 초당 1,000개 메시지  
- 부족: 100배 용량 부족 😱

해결책: 100개 샤드 필요 → 복잡성 급증
```
