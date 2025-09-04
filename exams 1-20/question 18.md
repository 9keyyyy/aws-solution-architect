## Question #18
An application development team is designing a microservice that will convert large images to smaller, compressed images. 
When a user uploads an image through the web interface, the microservice should store the image in an Amazon S3 bucket, process and compress the image with an AWS Lambda function, and store the image in its compressed form in a different S3 bucket.
A solutions architect needs to design a solution that uses durable, stateless components to process the images automatically.

Which combination of actions will meet these requirements? (Choose two.)

A. Create an Amazon Simple Queue Service (Amazon SQS) queue. Configure the S3 bucket to send a notification to the SQS queue when an image is uploaded to the S3 bucket.

B. Configure the Lambda function to use the Amazon Simple Queue Service (Amazon SQS) queue as the invocation source. When the SQS message is successfully processed, delete the message in the queue.

C. Configure the Lambda function to monitor the S3 bucket for new uploads. When an uploaded image is detected, write the file name to a text file in memory and use the text file to keep track of the images that were processed.

D. Launch an Amazon EC2 instance to monitor an Amazon Simple Queue Service (Amazon SQS) queue. When items are added to the queue, log the file name in a text file on the EC2 instance and invoke the Lambda function.

E. Configure an Amazon EventBridge (Amazon CloudWatch Events) event to monitor the S3 bucket. When an image is uploaded, send an alert to an Amazon ample Notification Service (Amazon SNS) topic with the application owner's email address for further processing.


### ✅ 요구사항
- 마이크로서비스: 대용량 이미지 → 압축 이미지 변환
- **내구성 있는, 상태 비저장** 컴포넌트 사용
- **자동 처리** 필요
- 워크플로우: S3 업로드 → Lambda 압축 → 다른 S3 저장

### ✅ 선택지 분석

**A. SQS 큐 + S3 이벤트 알림** ⭐
- **내구성**: SQS는 메시지 지속성 보장
- **상태 비저장**: 큐 기반 비동기 처리
- S3 이벤트 → SQS 자동 전송
- 표준 AWS 이벤트 처리 패턴

**B. Lambda + SQS 통합 + 메시지 삭제** ⭐
- **자동 처리**: SQS가 Lambda 트리거
- **내구성**: 처리 완료 후 메시지 삭제
- **상태 비저장**: Lambda는 무상태 함수
- 장애 시 메시지 재처리 가능

**C. Lambda S3 모니터링 + 메모리 파일**
- **상태 저장**: 메모리 텍스트 파일 사용 
- **비내구성**: Lambda 재시작 시 상태 손실 
- **폴링 방식**: 비효율적 

**D. EC2 SQS 모니터링 + 텍스트 파일**
- **상태 저장**: EC2 텍스트 파일 사용 
- **비내구성**: EC2 장애 시 상태 손실 
- **서버 기반**: 마이크로서비스 원칙 위배 

**E. EventBridge + SNS 이메일 알림**
- **수동 처리**: 이메일 알림만 제공 
- **자동화 없음**: 사람 개입 필요 
- 이미지 처리 자동화 불가

### 📋 내구성 있는 상태 비저장 아키텍처

### **Amazon SQS (Simple Queue Service)**
```yaml
내구성 특징:
  - 메시지 다중 AZ 복제
  - 최소 한 번 전달 보장
  - 메시지 보존 기간: 최대 14일
  - 처리 실패 시 재시도 가능

상태 비저장 특징:
  - 큐는 상태 정보 저장 안함
  - 각 메시지는 독립적 처리
  - 처리 순서와 무관한 병렬 처리
  - Lambda와 완벽한 통합
```

### **Lambda + SQS 통합**
```yaml
이벤트 처리 흐름:
  - SQS가 Lambda 자동 트리거
  - 배치 크기 조정 가능 (1-10개)
  - 동시 실행 제어
  - 자동 스케일링

오류 처리:
  - 처리 실패 시 메시지 재큐잉
  - DLQ (Dead Letter Queue) 지원
  - 재시도 횟수 제한 가능
  - 독성 메시지 격리
```

### 🔄 권장 아키텍처 흐름

### **이벤트 기반 처리 파이프라인**
```
사용자 업로드
    ↓
S3 Bucket (원본)
    ↓
S3 Event Notification
    ↓
SQS Queue ← 내구성 있는 메시지 저장
    ↓
Lambda Function ← SQS가 자동 트리거
    ↓
이미지 압축 처리
    ↓
S3 Bucket (압축본)
```

### **SQS 메시지 생명주기**
```yaml
1단계: 메시지 수신
   S3 이벤트 → SQS 큐 저장
   
2단계: 가시성 타임아웃
   Lambda가 메시지 처리 중
   다른 컨슈머에게 숨김 처리
   
3단계: 처리 완료
   Lambda 성공 시 메시지 삭제
   
4단계: 실패 처리
   타임아웃 시 메시지 다시 표시
   재처리 또는 DLQ로 이동
```

### **잘못된 접근법들**
```yaml
메모리 상태 저장 (C번):
  ❌ Lambda 컨테이너 재사용 불확실
  ❌ 동시 실행 시 상태 공유 불가
  ❌ 장애 시 상태 손실

EC2 파일 상태 (D번):
  ❌ 단일 장애점
  ❌ 확장성 제한
  ❌ 상태 관리 복잡성

이메일 알림 (E번):
  ❌ 자동화 부재
  ❌ 사람 개입 필요
  ❌ 확장성 없음
```

### **상태 비저장 vs 상태 저장**
```yaml
상태 비저장 (권장):
  ✅ 각 요청이 독립적
  ✅ 수평 확장 용이
  ✅ 장애 격리
  ✅ 복구 용이

상태 저장 (비권장):
  ❌ 세션 정보 유지 필요
  ❌ 확장성 제한
  ❌ 장애 파급 효과
  ❌ 복구 복잡성
```
