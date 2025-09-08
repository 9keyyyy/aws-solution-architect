## Question #98
An image-processing company has a web application that users use to upload images. 
The application uploads the images into an Amazon S3 bucket. 
The company has set up S3 event notifications to publish the object creation events to an Amazon Simple Queue Service (Amazon SQS) standard queue. 
The SQS queue serves as the event source for an AWS Lambda function that processes the images and sends the results to users through email.
Users report that they are receiving multiple email messages for every uploaded image. 
A solutions architect determines that SQS messages are invoking the Lambda function more than once, resulting in multiple email messages.

What should the solutions architect do to resolve this issue with the LEAST operational overhead?

A. Set up long polling in the SQS queue by increasing the ReceiveMessage wait time to 30 seconds.

B. Change the SQS standard queue to an SQS FIFO queue. Use the message deduplication ID to discard duplicate messages.

C. Increase the visibility timeout in the SQS queue to a value that is greater than the total of the function timeout and the batch window timeout.

D. Modify the Lambda function to delete each message from the SQS queue immediately after the message is read before processing.

## Question #98 분석

### 요구사항
- **중복 이메일** 문제 해결
- **SQS 메시지**가 Lambda를 여러 번 호출
- **최소 운영 오버헤드** 솔루션

### 핵심 문제
```yaml
현재 상황:
❌ Lambda 함수가 같은 메시지를 여러 번 처리
❌ 처리 완료 전 메시지가 다시 visible 상태로 변경
❌ 중복 실행으로 다중 이메일 발송

원인: Visibility Timeout < 함수 실행 시간
```

### 선택지 분석

**A. Long Polling 설정**
- **목적 다름**: 폴링 효율성 개선용 ❌
- **중복 실행 해결 안됨**: 근본 문제 미해결 ❌

**B. FIFO Queue + 중복 제거**
- **복잡성 증가**: 큐 유형 변경 필요 ❌
- **순서 보장**: 이미지 처리에 불필요 ❌
- **운영 오버헤드**: 더 복잡한 설정 ❌

**C. Visibility Timeout 증가** ⭐
- **근본 원인 해결**: 함수 완료까지 메시지 숨김 ✅
- **간단한 설정**: 타임아웃 값만 조정 ✅
- **최소 오버헤드**: 설정 변경만으로 해결 ✅

**D. 즉시 메시지 삭제**
- **위험한 접근**: 처리 실패 시 메시지 손실 ❌
- **재시도 불가**: 오류 복구 메커니즘 상실 ❌

### Visibility Timeout 작동 방식

```yaml
현재 문제:
1. Lambda 함수 실행 시간: 60초
2. Visibility Timeout: 30초 (기본값)
3. 30초 후 메시지 재출현 → 중복 실행

해결책:
1. Visibility Timeout: 70초로 설정
2. 함수 완료까지 메시지 숨김 유지
3. 완료 후 자동 삭제 → 중복 방지
```

### 설정 공식

```yaml
권장 설정:
Visibility Timeout = Function Timeout + Batch Window + 여유시간

예시:
- Function Timeout: 60초
- Batch Window: 5초  
- 여유시간: 5초
- 총 Visibility Timeout: 70초
```
