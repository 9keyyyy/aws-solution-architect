## Question #45
A company has a data ingestion workflow that consists of the following:

• An Amazon Simple Notification Service (Amazon SNS) topic for notifications about new data deliveries

• An AWS Lambda function to process the data and record metadata

The company observes that the ingestion workflow fails occasionally because of network connectivity issues. 
When such a failure occurs, the Lambda function does not ingest the corresponding data unless the company manually reruns the job.

Which combination of actions should a solutions architect take to ensure that the Lambda function ingests all data in the future? (Choose two.)

A. Deploy the Lambda function in multiple Availability Zones.

B. Create an Amazon Simple Queue Service (Amazon SQS) queue, and subscribe it to the SNS topic.

C. Increase the CPU and memory that are allocated to the Lambda function.

D. Increase provisioned throughput for the Lambda function.

E. Modify the Lambda function to read from an Amazon Simple Queue Service (Amazon SQS) queue.

## Question #45 분석

### ✅ 요구사항
- **데이터 수집 워크플로우**: SNS → Lambda 처리
- **네트워크 연결 문제**로 간헐적 실패
- Lambda 함수가 **데이터 손실** 발생
- **모든 데이터 수집 보장** 필요
- **2개 조합** 선택

### ✅ 현재 아키텍처 문제점
```yaml
SNS → Lambda (직접 호출)

문제:
❌ 네트워크 실패 시 메시지 손실
❌ Lambda 장애 시 재시도 제한적
❌ 메시지 지속성 없음
❌ 수동 재실행 필요
```

### ✅ 선택지 분석

**A. 다중 AZ Lambda 배포**
- **AZ 장애 대응**: 인프라 장애에는 효과적 
- **네트워크 연결 문제 미해결**: 근본 원인 해결 안됨 
- Lambda는 기본적으로 다중 AZ 실행

**B. SQS 큐 생성 + SNS 구독** ⭐
- **메시지 지속성**: 큐에 메시지 안전 저장 ✅
- **장애 격리**: SNS와 Lambda 간 버퍼 역할 ✅
- **자동 재시도**: SQS 내장 재시도 메커니즘 ✅

**C. CPU/메모리 증가**
- **성능 문제 아님**: 네트워크 연결이 원인
- **리소스 증가로 해결 불가**: 근본 원인 다름 

**D. 프로비저닝된 동시성 증가**
- **콜드 스타트 해결**: 네트워크 문제와 무관 
- **처리량 증가**: 연결 안정성과 별개 

**E. Lambda가 SQS에서 읽기** ⭐
- **폴링 방식**: Lambda가 SQS에서 안정적으로 메시지 소비 ✅
- **내장 재시도**: 처리 실패 시 자동 재시도 ✅
- **배치 처리**: 효율적인 메시지 처리 ✅

### 📋 B + E 조합의 해결책

#### **개선된 아키텍처**
```yaml
기존: SNS → Lambda (불안정)
개선: SNS → SQS → Lambda (안정적)

데이터 흐름:
1. 데이터 도착 알림 → SNS
2. SNS → SQS 큐에 메시지 저장
3. Lambda가 SQS에서 폴링하여 처리
4. 처리 완료 후 메시지 삭제
```

#### **장애 복구 메커니즘**
```yaml
네트워크 장애 시:
1. SNS → SQS: 메시지 안전 저장
2. Lambda 연결 실패: 메시지 큐에 대기
3. 연결 복구: Lambda가 대기 중인 메시지 처리
4. 자동 재시도: 실패 시 지정 횟수만큼 재시도
```

### 🔄 SQS의 안정성 메커니즘

#### **메시지 지속성**
```yaml
Visibility Timeout:
- Lambda 처리 중: 메시지 다른 컨슈머에게 숨김
- 처리 완료: 메시지 삭제
- 처리 실패: 메시지 다시 가시화 (재처리)

Dead Letter Queue:
- 최대 재시도 초과 시: DLQ로 이동
- 수동 분석 및 처리 가능
- 데이터 손실 완전 방지
```

#### **자동 재시도 정책**
```yaml
SQS 재시도 설정:
- maxReceiveCount: 3 (최대 3회 재시도)
- visibilityTimeoutSeconds: 300 (5분 대기)
- 실패 시: DLQ 이동으로 데이터 보존
```
### 📊 솔루션 비교

#### **현재 vs 개선된 아키텍처**
```yaml
현재 (SNS → Lambda):
- 장애 시: 메시지 손실
- 복구: 수동 재실행 필요
- 안정성: 낮음

개선 (SNS → SQS → Lambda):
- 장애 시: 메시지 큐에서 대기
- 복구: 자동 재처리
- 안정성: 높음 (99.9% SLA)
```

#### **메시지 처리 보장**
```yaml
SQS 폴링 모델:
- Lambda가 능동적으로 메시지 요청
- 배치 크기: 1-10개 메시지
- 처리 실패 시: 자동 재큐잉
- 최종 실패: DLQ로 안전 보관
```
