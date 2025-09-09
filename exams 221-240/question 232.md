## Question #232
A company runs demonstration environments for its customers on Amazon EC2 instances. 
Each environment is isolated in its own VPC. 
The company’s operations team needs to be notified when RDP or SSH access to an environment has been established.

A. Configure Amazon CloudWatch Application Insights to create AWS Systems Manager OpsItems when RDP or SSH access is detected.

B. Configure the EC2 instances with an IAM instance profile that has an IAM role with the AmazonSSMManagedInstanceCore policy attached.

C. Publish VPC flow logs to Amazon CloudWatch Logs. Create required metric filters. Create an Amazon CloudWatch metric alarm with a notification action for when the alarm is in the ALARM state.

D. Configure an Amazon EventBridge rule to listen for events of type EC2 Instance State-change Notification. Configure an Amazon Simple Notification Service (Amazon SNS) topic as a target. Subscribe the operations team to the topic.

## Question #232 분석

### 요구사항
- **데모 환경** EC2 인스턴스 (격리된 VPC)
- **RDP/SSH 접근** 감지 및 알림
- **운영팀 알림** 필요

### 선택지 분석

**A. CloudWatch Application Insights + Systems Manager**
- **용도 불일치**: Application Insights는 애플리케이션 성능 모니터링용 ❌
- **RDP/SSH 감지 기능 없음**: 네트워크 접근 감지 불가 ❌

**B. IAM 인스턴스 프로파일 + SSM 정책**
- **접근 감지와 무관**: SSM 관리 기능만 제공 ❌
- **알림 기능 없음**: RDP/SSH 접근 모니터링과 별개 ❌

**C. VPC Flow Logs + CloudWatch + 메트릭 필터** ⭐
- **네트워크 트래픽 감지**: Flow Logs로 포트 22(SSH), 3389(RDP) 모니터링 ✅
- **메트릭 필터**: 특정 포트 접근 패턴 감지 ✅
- **자동 알림**: CloudWatch 알람으로 즉시 통지 ✅
- **정확한 감지**: 실제 네트워크 연결 기반 모니터링 ✅

**D. EventBridge + EC2 상태 변경 이벤트**
- **상태 변경 감지**: 인스턴스 시작/중지만 감지 ❌
- **접근 감지 불가**: RDP/SSH 연결과 무관한 이벤트 ❌

### VPC Flow Logs 솔루션

```yaml
설정 과정:
1. VPC Flow Logs → CloudWatch Logs 전송
2. 메트릭 필터 생성:
   - SSH: 포트 22 연결 감지
   - RDP: 포트 3389 연결 감지
3. CloudWatch 알람 설정
4. SNS 토픽으로 운영팀 알림

Flow Logs 필터 예시:
[srcaddr, dstaddr, srcport, dstport, protocol, action]
- dstport = 22 AND action = ACCEPT (SSH)
- dstport = 3389 AND action = ACCEPT (RDP)
```
