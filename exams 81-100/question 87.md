## Question #87
A company hosts an application on AWS Lambda functions that are invoked by an Amazon API Gateway API. 
The Lambda functions save customer data to an Amazon Aurora MySQL database. 
Whenever the company upgrades the database, the Lambda functions fail to establish database connections until the upgrade is complete. 
The result is that customer data is not recorded for some of the event.
A solutions architect needs to design a solution that stores customer data that is created during database upgrades.

Which solution will meet these requirements?

A. Provision an Amazon RDS proxy to sit between the Lambda functions and the database. Configure the Lambda functions to connect to the RDS proxy.

B. Increase the run time of the Lambda functions to the maximum. Create a retry mechanism in the code that stores the customer data in the database.

C. Persist the customer data to Lambda local storage. Configure new Lambda functions to scan the local storage to save the customer data to the database.

D. Store the customer data in an Amazon Simple Queue Service (Amazon SQS) FIFO queue. Create a new Lambda function that polls the queue and stores the customer data in the database.


## Question #87 분석

### ✅ 요구사항
- **Lambda 함수 → Aurora MySQL** 데이터 저장
- **DB 업그레이드 중 연결 실패** 문제
- **고객 데이터 손실 방지** 필요
- **업그레이드 중에도 데이터 보존** 솔루션

### ✅ 핵심 문제
```yaml
현재 상황:
❌ DB 업그레이드 중 Lambda 연결 실패
❌ 업그레이드 기간 동안 데이터 손실
❌ 실시간 처리 의존성

해결 방향:
✅ 임시 데이터 저장소 필요
✅ 비동기 처리 패턴
✅ 업그레이드 완료 후 데이터 처리
```

### ✅ 선택지 분석

**A. RDS Proxy + Lambda 연결**
- **연결 개선**: RDS Proxy로 연결 풀링 ✅
- **업그레이드 중 한계**: 여전히 DB 접근 불가 시점 존재 ❌
- **근본 해결 실패**: 업그레이드 중 데이터 저장 불가 ❌

**B. Lambda 런타임 증대 + 재시도**
- **15분 최대 제한**: Lambda 최대 실행 시간 한계 ❌
- **DB 업그레이드 시간**: 보통 15분 이상 소요 ❌
- **리소스 낭비**: 대기 시간 동안 비용 발생 ❌

**C. Lambda 로컬 스토리지 + 스캔**
- **휘발성 저장소**: Lambda 종료 시 데이터 손실 ❌
- **512MB 제한**: 로컬 스토리지 용량 한계 ❌
- **아키텍처 부적절**: 임시 저장용 설계 아님 ❌

**D. SQS FIFO 큐 + 별도 Lambda 폴링** ⭐
- **내구성 보장**: SQS 메시지 최대 14일 보존 ✅
- **비동기 처리**: DB 복구 후 순차 처리 ✅
- **순서 보장**: FIFO 큐로 데이터 순서 유지 ✅
- **완전 분리**: 즉시 저장과 DB 처리 분리 ✅

### 📋 아키텍처 비교

**현재 (문제 상황)**
```yaml
API Gateway → Lambda → Aurora MySQL (업그레이드 중 실패)
결과: 데이터 손실
```

**D안 (해결책)**
```yaml
API Gateway → Lambda → SQS FIFO Queue (즉시 저장)
SQS Queue → 별도 Lambda → Aurora MySQL (DB 복구 후 처리)

장점:
✅ 업그레이드 중에도 API 응답 정상
✅ 데이터 손실 방지
✅ DB 복구 후 순차 처리
✅ 순서 보장 (FIFO)
```


### 🔄 장애 복구 프로세스

**정상 운영**
```yaml
1. API 요청 → Lambda → SQS 저장 → 즉시 응답
2. SQS → DB Lambda → Aurora 저장
3. 실시간 데이터 처리
```

**DB 업그레이드 중**
```yaml
1. API 요청 → Lambda → SQS 저장 → 즉시 응답 (정상)
2. DB Lambda → Aurora 연결 실패 → SQS 메시지 유지
3. 고객은 정상 응답 수신, 데이터는 큐에 보존
```

**DB 복구 후**
```yaml
1. Aurora 연결 복구
2. SQS 메시지들이 순차적으로 처리
3. 업그레이드 중 누적된 모든 데이터 저장
4. 데이터 손실 없이 정상 운영 재개
```

