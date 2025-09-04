## Question #40
A company has thousands of edge devices that collectively generate 1 TB of status alerts each day. 
Each alert is approximately 2 KB in size. 
A solutions architect needs to implement a solution to ingest and store the alerts for future analysis.
The company wants a highly available solution. 
However, the company needs to minimize costs and does not want to manage additional infrastructure. 
Additionally, the company wants to keep 14 days of data available for immediate analysis and archive any data older than 14 days.

What is the MOST operationally efficient solution that meets these requirements?

A. Create an Amazon Kinesis Data Firehose delivery stream to ingest the alerts. Configure the Kinesis Data Firehose stream to deliver the alerts to an Amazon S3 bucket. Set up an S3 Lifecycle configuration to transition data to Amazon S3 Glacier after 14 days.

B. Launch Amazon EC2 instances across two Availability Zones and place them behind an Elastic Load Balancer to ingest the alerts. Create a script on the EC2 instances that will store the alerts in an Amazon S3 bucket. Set up an S3 Lifecycle configuration to transition data to Amazon S3 Glacier after 14 days.

C. Create an Amazon Kinesis Data Firehose delivery stream to ingest the alerts. Configure the Kinesis Data Firehose stream to deliver the alerts to an Amazon OpenSearch Service (Amazon Elasticsearch Service) cluster. Set up the Amazon OpenSearch Service (Amazon Elasticsearch Service) cluster to take manual snapshots every day and delete data from the cluster that is older than 14 days.

D. Create an Amazon Simple Queue Service (Amazon SQS) standard queue to ingest the alerts, and set the message retention period to 14 days. Configure consumers to poll the SQS queue, check the age of the message, and analyze the message data as needed. If the message is 14 days old, the consumer should copy the message to an Amazon S3 bucket and delete the message from the SQS queue.

## Question #40 분석

### ✅ 요구사항
- **1TB/일 상태 알림** (엣지 디바이스에서 생성)
- 각 알림 **2KB 크기** (약 50만 개/일)
- **고가용성** 솔루션
- **비용 최소화** + **추가 인프라 관리 없음**
- **14일 즉시 분석** + **14일 후 아카이브**
- **운영 효율성 최대화**

### ✅ 선택지 분석

**A. Kinesis Data Firehose → S3 → Glacier** ⭐
- **완전 서버리스**: 인프라 관리 불필요 ✅
- **자동 수집**: Firehose가 자동으로 S3 저장 ✅
- **자동 아카이브**: S3 Lifecycle로 Glacier 전환 ✅
- **고가용성**: AWS 관리형 서비스 ✅
- **운영 효율성**: 설정 후 자동 운영 ✅

**B. EC2 + ELB + 스크립트**
- **인프라 관리**: EC2 패치, 모니터링 필요 
- **높은 운영 부담**: 스크립트 개발 및 유지보수 
- **추가 인프라**: 요구사항 위배 
- **복잡성**: 불필요한 서버 관리

**C. Kinesis Data Firehose → OpenSearch**
- **수동 스냅샷**: 자동화 부족 
- **복잡한 데이터 관리**: 수동 삭제 필요 
- **높은 비용**: OpenSearch 클러스터 24시간 운영 
- **과도한 기능**: 단순 저장에 검색 엔진 오버킬

**D. SQS + 컨슈머 + 스크립트**
- **복잡한 로직**: 메시지 나이 확인 및 처리 
- **추가 개발**: 컨슈머 애플리케이션 필요 
- **운영 부담**: 큐 모니터링 및 관리 
- **SQS 제한**: 대용량 메시지 저장에 부적합

### 📋 Kinesis Data Firehose 최적화

#### **Firehose의 대용량 데이터 처리**
```yaml
수집 능력:
  - 처리량: 제한 없음 (자동 확장)
  - 50만 개/일 메시지 여유롭게 처리
  - 배치 최적화: 자동으로 효율적 크기로 묶음
  - 압축: Gzip, Snappy 지원으로 스토리지 절약

고가용성:
  - 다중 AZ 자동 분산
  - 장애 시 자동 재시도
  - 데이터 손실 없는 전송 보장
  - 버퍼링으로 일시적 장애 극복
```

#### **자동 S3 저장 및 파티셔닝**
```yaml
데이터 조직:
  s3://alerts-bucket/
  ├── year=2024/month=03/day=15/hour=14/
  │   ├── alerts-001.gz
  │   └── alerts-002.gz
  └── year=2024/month=03/day=16/hour=09/
      └── alerts-003.gz

Firehose 배치 설정:
  - 버퍼 크기: 128MB 또는 900초 간격
  - 압축: Gzip으로 70% 용량 절약
  - 파티셔닝: 시간 기반 자동 분할
```

### 🔄 S3 Lifecycle을 통한 자동 아카이브

#### **Lifecycle 규칙 설정**
```yaml
규칙 구성:
  Rule Name: AlertsArchiving
  Filter: alerts/ prefix
  
  Transitions:
    - Days: 14
      StorageClass: GLACIER
    - Days: 90  
      StorageClass: DEEP_ARCHIVE (선택사항)
      
  Status: Enabled
```

#### **실제 데이터 흐름**
```yaml
Day 1-14: S3 Standard
  - 즉시 분석 가능
  - 빠른 액세스 (밀리초)
  - 높은 가용성 (99.999999999%)

Day 15+: S3 Glacier  
  - 자동 전환 (사용자 개입 없음)
  - 95% 저렴한 스토리지 비용
  - 복원 시간: 1-5분 (Expedited)
  - 아카이브 목적에 완벽
```

### 📊 운영 효율성 비교

#### **완전 자동화 (A번)**
```yaml
설정 후 자동 운영:
  ✅ 데이터 수집: Firehose 자동
  ✅ 저장 관리: S3 자동
  ✅ 아카이브: Lifecycle 자동
  ✅ 모니터링: CloudWatch 자동
  ✅ 확장: 트래픽 증가 시 자동

일일 운영 작업: 0시간
월간 운영 작업: 모니터링 1시간
연간 운영 작업: 설정 검토 4시간
```

#### **수동 관리 필요 (B, C, D번)**
```yaml
EC2 기반 솔루션 (B번):
  일일: 서버 상태 확인 30분
  주간: 로그 분석, 성능 튜닝 2시간
  월간: 패치 적용, 용량 계획 8시간
  긴급: 장애 대응 불규칙

OpenSearch (C번):
  일일: 수동 스냅샷 확인 15분
  주간: 인덱스 최적화 1시간  
  월간: 클러스터 관리 4시간
  긴급: 검색 성능 이슈 대응

SQS 솔루션 (D번):
  일일: 큐 상태 모니터링 20분
  주간: 컨슈머 성능 튜닝 3시간
  월간: 메시지 처리 로직 개선 6시간
```
