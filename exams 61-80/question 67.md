## Question #67
A company hosts an application on multiple Amazon EC2 instances. 
The application processes messages from an Amazon SQS queue, writes to an Amazon RDS table, and deletes the message from the queue. 
Occasional duplicate records are found in the RDS table. 
The SQS queue does not contain any duplicate messages.

What should a solutions architect do to ensure messages are being processed once only?

A. Use the CreateQueue API call to create a new queue.

B. Use the AddPermission API call to add appropriate permissions.

C. Use the ReceiveMessage API call to set an appropriate wait time.

D. Use the ChangeMessageVisibility API call to increase the visibility timeout.

## Question #67 분석

### ✅ 요구사항
- **다중 EC2 인스턴스**에서 애플리케이션 실행
- **SQS 큐**에서 메시지 처리 → **RDS 테이블**에 쓰기 → **큐에서 메시지 삭제**
- **RDS에 중복 레코드** 발견
- **SQS 큐에는 중복 메시지 없음**
- **메시지 한 번만 처리** 보장 필요

### ✅ 문제 분석

#### **중복 처리 발생 원인**
```yaml
시나리오: 메시지 처리 중 타임아웃
1. 인스턴스 A가 메시지 수신 (Visibility Timeout 시작)
2. 인스턴스 A가 RDS에 데이터 삽입 (처리 중)
3. Visibility Timeout 만료 (처리가 아직 완료되지 않음)
4. 인스턴스 B가 같은 메시지 수신 (재처리)
5. 인스턴스 B도 RDS에 데이터 삽입 → 중복!
6. 두 인스턴스 모두 메시지 삭제 시도
```

### ✅ 선택지 분석

**A. CreateQueue API로 새 큐 생성**
- **새 큐 생성**: 기존 문제 해결과 무관 ❌
- **근본 원인**: Visibility Timeout 문제이지 큐 자체 문제 아님 ❌
- **불필요한 변경**: 기존 큐는 정상 작동 ❌

**B. AddPermission API로 적절한 권한 추가**
- **권한 문제**: 메시지 처리는 이미 정상 작동 중 ❌
- **접근 권한**: 중복 처리와 무관한 이슈 ❌
- **근본 원인 미해결**: Visibility Timeout 문제 지속 ❌

**C. ReceiveMessage API로 적절한 대기 시간 설정**
- **Long Polling**: 메시지 수신 효율성 개선 ⚠️
- **중복 처리**: 대기 시간은 중복과 무관 ❌
- **근본 원인**: Visibility Timeout 문제 해결 안 함 ❌

**D. ChangeMessageVisibility API로 Visibility Timeout 증가** ⭐
- **근본 원인 해결**: 처리 시간보다 긴 타임아웃 설정 ✅
- **중복 방지**: 처리 완료 전까지 다른 인스턴스 접근 차단 ✅
- **정확한 솔루션**: 문제의 직접적 해결책 ✅

### 📋 핵심 개념 정리

#### **SQS Visibility Timeout**
```yaml
정의:
  ✅ 메시지 수신 후 다른 컨슈머에게 보이지 않는 시간
  ✅ 메시지 처리 중 중복 수신 방지
  ✅ 기본값: 30초

작동 방식:
  1. 컨슈머가 메시지 수신
  2. 메시지가 "비가시" 상태로 변경
  3. Visibility Timeout 동안 다른 컨슈머 접근 불가
  4. 처리 완료 시 메시지 삭제
  5. 타임아웃 만료 시 메시지 다시 가시화
```
