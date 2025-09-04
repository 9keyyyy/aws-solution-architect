## Question #25
A company is designing an application. 
The application uses an AWS Lambda function to receive information through Amazon API Gateway and to store the information in an Amazon Aurora PostgreSQL database.
During the proof-of-concept stage, the company has to increase the Lambda quotas significantly to handle the high volumes of data that the company needs to load into the database. 
A solutions architect must recommend a new design to improve scalability and minimize the configuration effort.

Which solution will meet these requirements?

A. Refactor the Lambda function code to Apache Tomcat code that runs on Amazon EC2 instances. Connect the database by using native Java Database Connectivity (JDBC) drivers.

B. Change the platform from Aurora to Amazon DynamoDProvision a DynamoDB Accelerator (DAX) cluster. Use the DAX client SDK to point the existing DynamoDB API calls at the DAX cluster.

C. Set up two Lambda functions. Configure one function to receive the information. Configure the other function to load the information into the database. Integrate the Lambda functions by using Amazon Simple Notification Service (Amazon SNS).

D. Set up two Lambda functions. Configure one function to receive the information. Configure the other function to load the information into the database. Integrate the Lambda functions by using an Amazon Simple Queue Service (Amazon SQS) queue.

## Question #25 분석

### ✅ 요구사항
- Lambda 할당량 초과 문제 해결
- **확장성 개선** 필요
- **구성 노력 최소화**
- 높은 볼륨 데이터 처리

### ✅ 선택지 분석

**A. EC2 + Tomcat + JDBC**
- **높은 운영 오버헤드**: 서버 관리 필요 
- **구성 노력 증가**: 인프라 관리 복잡성 
- Lambda의 서버리스 장점 상실

**B. DynamoDB + DAX**
- **플랫폼 변경**: PostgreSQL → DynamoDB 
- **애플리케이션 대폭 수정** 필요
- 구성 노력 최소화 위배

**C. Lambda + SNS 분리**
- **Fan-out 패턴**: 메시지 브로드캐스트 
- **순서 보장 없음**: 데이터 처리 순서 문제
- **중복 처리 위험**: SNS는 최소 한 번 전달

**D. Lambda + SQS 분리** ⭐
- **비동기 처리**: DB 부하 분산 ✅
- **큐 기반 확장**: Lambda 할당량 문제 해결 ✅
- **최소 구성**: 기존 아키텍처 재사용 ✅

### 📋 핵심 문제점과 해결책

### **현재 문제점**
```yaml
Lambda 할당량 초과:
  - 동시 실행 한도 도달
  - DB 연결 풀 포화
  - 동기식 처리의 한계

병목 지점:
  - API 요청 → 즉시 DB 쓰기
  - DB 처리 속도가 전체 성능 제한
```

### **SQS 기반 해결책 (D번)**
```yaml
아키텍처 분리:
API Gateway → Lambda1 (수신) → SQS → Lambda2 (DB 저장)

장점:
  - Lambda1: 빠른 응답, 할당량 효율적 사용
  - Lambda2: DB 처리만 담당, 독립적 확장
  - SQS: 버퍼 역할, 트래픽 스파이크 흡수
```

### 🔄 성능 개선 효과

### **기존 방식 vs 새 방식**
```yaml
기존 (단일 Lambda):
  - API 응답시간: 2-5초 (DB 쓰기 대기)
  - 동시 처리: 1000개 제한 도달

새 방식 (Lambda + SQS):
  - API 응답시간: 100-200ms (SQS 전송)
  - 동시 처리: 수만 개 요청 가능
  - DB 처리: 백그라운드에서 안정적
```

### **SQS vs SNS 비교**
```yaml
SQS 장점 (D번):
  ✅ 순차 처리 보장
  ✅ 재시도 메커니즘
  ✅ Dead Letter Queue 지원
  ✅ 정확히 한 번 처리 가능

SNS 문제점 (C번):
  ❌ 브로드캐스트 방식 (불필요)
  ❌ 순서 보장 어려움
  ❌ 중복 메시지 가능성
```
