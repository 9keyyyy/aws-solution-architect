## Question #33
A company runs an online marketplace web application on AWS. 
The application serves hundreds of thousands of users during peak hours. The company needs a scalable, near-real-time solution to share the details of millions of financial transactions with several other internal applications. 
Transactions also need to be processed to remove sensitive data before being stored in a document database for low-latency retrieval.

What should a solutions architect recommend to meet these requirements?

A. Store the transactions data into Amazon DynamoDB. Set up a rule in DynamoDB to remove sensitive data from every transaction upon write. Use DynamoDB Streams to share the transactions data with other applications.

B. Stream the transactions data into Amazon Kinesis Data Firehose to store data in Amazon DynamoDB and Amazon S3. Use AWS Lambda integration with Kinesis Data Firehose to remove sensitive data. Other applications can consume the data stored in Amazon S3.

C. Stream the transactions data into Amazon Kinesis Data Streams. Use AWS Lambda integration to remove sensitive data from every transaction and then store the transactions data in Amazon DynamoDB. Other applications can consume the transactions data off the Kinesis data stream.

D. Store the batched transactions data in Amazon S3 as files. Use AWS Lambda to process every file and remove sensitive data before updating the files in Amazon S3. The Lambda function then stores the data in Amazon DynamoDB. Other applications can consume transaction files stored in Amazon S3.

## Question #33 분석

### ✅ 요구사항
- **수십만 사용자** 피크 시간 처리
- **수백만 금융 거래** 데이터 공유
- **확장 가능한, 거의 실시간** 솔루션
- **민감 데이터 제거** 처리 필요
- **여러 내부 애플리케이션**과 데이터 공유
- **문서 DB에 저장** (낮은 지연 시간 조회)

### ✅ 선택지 분석

**A. DynamoDB + DynamoDB Streams**
- **DynamoDB 규칙 없음**: DynamoDB에는 쓰기 시 데이터 변환 규칙 기능 없음 ❌
- **잘못된 아키텍처**: 기술적으로 불가능한 구성

**B. Kinesis Data Firehose + Lambda + DynamoDB + S3**
- **배치 처리**: Firehose는 배치 전송 (거의 실시간 아님) ❌
- **복잡한 아키텍처**: 불필요한 S3 스토리지 추가
- 실시간 요구사항 미충족

**C. Kinesis Data Streams + Lambda + DynamoDB** ⭐
- **실시간 스트리밍**: Data Streams는 밀리초 단위 처리 ✅
- **Lambda 통합**: 민감 데이터 제거 처리 ✅
- **확장성**: 자동 확장으로 수백만 거래 처리 ✅
- **다중 소비자**: 여러 애플리케이션이 동시 소비 ✅
- **DynamoDB**: 낮은 지연 시간 조회 ✅

**D. S3 배치 파일 처리**
- **배치 처리**: 실시간 요구사항 위배 ❌
- **파일 기반**: 확장성 및 성능 한계
- 거의 실시간 솔루션 아님

### 📋 실시간 스트리밍 아키텍처

### **Kinesis Data Streams 핵심 기능**
```yaml
실시간 처리:
  - 밀리초 단위 데이터 ingestion
  - 초당 수백만 레코드 처리
  - 샤드별 병렬 처리

확장성:
  - 샤드 추가로 처리량 증가
  - 자동 샤딩 지원
  - 피크 트래픽 대응

다중 소비자:
  - 여러 애플리케이션이 동일 스트림 소비
  - 각자 독립적인 처리 가능
  - Fan-out 패턴 지원
```

### **실시간 데이터 흐름 (C번)**
```yaml
1. 온라인 마켓플레이스 → 거래 발생
2. Kinesis Data Streams → 실시간 수집
3. Lambda 함수 → 민감 데이터 제거 처리
4. DynamoDB → 정제된 데이터 저장
5. 다른 내부 애플리케이션들 → Kinesis에서 직접 소비
```

### 🔄 데이터 처리 패턴 비교

### **실시간 vs 배치 처리**
```yaml
실시간 (Kinesis Data Streams):
  - 지연 시간: 밀리초-초 단위
  - 처리 방식: 스트리밍, 연속적
  - 확장성: 샤드 기반 수평 확장
  - 적합 용도: 금융 거래, 실시간 알림

배치 처리 (S3 파일, Firehose):
  - 지연 시간: 분-시간 단위
  - 처리 방식: 파일 단위, 주기적
  - 확장성: 파일 크기/개수 제한
  - 적합 용도: 데이터 분석, 보고서
```

### **민감 데이터 처리**
```yaml
금융 거래 데이터 예시:
  원본: {
    "user_id": "12345",
    "credit_card": "4532-1234-5678-9012", ← 민감
    "ssn": "123-45-6789", ← 민감  
    "amount": 149.99,
    "merchant": "Amazon",
    "timestamp": "2024-03-15T10:30:00Z"
  }

Lambda 처리 후: {
    "user_id": "12345",
    "credit_card": "****-****-****-9012", ← 마스킹
    "amount": 149.99,
    "merchant": "Amazon", 
    "timestamp": "2024-03-15T10:30:00Z"
  }
```

### 📊 선택지별 성능 비교

### **지연 시간 분석**
```yaml
거래 발생 → 내부 앱 소비까지:

A번 (DynamoDB): 기술적 불가능
B번 (Firehose): 1-15분 지연 (배치)
C번 (Data Streams): 100ms-1초 ✅
D번 (S3 배치): 5-30분 지연
```

### **처리량 분석**
```yaml
피크 시간 수십만 사용자:
  - 초당 거래 수: 10,000-50,000 TPS

C번 (Kinesis Data Streams):
  ✅ 샤드당 1,000 TPS
  ✅ 50개 샤드로 50,000 TPS 처리
  ✅ 자동 확장 가능

D번 (S3 배치):
  ❌ 파일 생성/처리 오버헤드
  ❌ Lambda 동시 실행 제한
  ❌ 확장성 한계
```

### 🔍 다른 옵션들의 문제점

### **DynamoDB 규칙 (A번)**
```yaml
기술적 불가능:
  ❌ DynamoDB에는 쓰기 시 데이터 변환 기능 없음
  ❌ Triggers나 Rules 같은 기능 미지원
  ❌ 애플리케이션 레벨에서만 처리 가능

실제로는:
  - Lambda 함수가 필요
  - DynamoDB Streams는 변경 후 이벤트만 제공
```

### **Firehose 한계 (B번)**
```yaml
배치 처리 특성:
  ❌ 최소 1분 배치 간격
  ❌ 버퍼 크기 도달 시까지 대기
  ❌ 거의 실시간 요구사항 미충족

적합한 용도:
  ✅ 데이터 레이크 구축
  ✅ 분석용 데이터 적재
  ✅ 장기 보관 목적
```

### **S3 파일 처리 (D번)**
```yaml
파일 기반 한계:
  ❌ 파일 생성 → 처리 → 업데이트 지연
  ❌ 동시 파일 처리 시 충돌 위험
  ❌ 확장성 제한 (Lambda 동시 실행)

배치 처리 지연:
  - 파일 크기가 찰 때까지 대기
  - 처리 완료까지 추가 대기
  - 전체 프로세스: 분-시간 단위
```
