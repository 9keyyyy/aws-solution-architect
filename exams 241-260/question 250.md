## Question #250
A company’s security team requests that network traffic be captured in VPC Flow Logs. 
The logs will be frequently accessed for 90 days and then accessed intermittently.

What should a solutions architect do to meet these requirements when configuring the logs?

A. Use Amazon CloudWatch as the target. Set the CloudWatch log group with an expiration of 90 days

B. Use Amazon Kinesis as the target. Configure the Kinesis stream to always retain the logs for 90 days.

C. Use AWS CloudTrail as the target. Configure CloudTrail to save to an Amazon S3 bucket, and enable S3 Intelligent-Tiering.

D. Use Amazon S3 as the target. Enable an S3 Lifecycle policy to transition the logs to S3 Standard-Infrequent Access (S3 Standard-IA) after 90 days.

## Question #250 분석

### 요구사항
- **VPC Flow Logs** 네트워크 트래픽 캡처
- **90일간 빈번한 접근**
- **90일 후 간헐적 접근**

### 선택지 분석

**A. CloudWatch + 90일 만료**
- **만료 문제**: 90일 후 로그가 완전 삭제됨 ❌
- **간헐적 접근 불가**: 90일 후 데이터 손실 ❌
- **요구사항 미충족**: 장기 보관 필요하지만 삭제됨 ❌

**B. Kinesis Stream + 90일 보관**
- **용도 불일치**: Kinesis는 실시간 스트리밍용, 로그 저장용 아님 ❌
- **보관 제한**: Kinesis는 최대 365일 보관, 장기 저장에 부적합 ❌
- **비용 비효율**: 스트리밍 서비스로 로그 저장 시 고비용 ❌

**C. CloudTrail + S3 + Intelligent-Tiering**
- **서비스 혼동**: CloudTrail은 API 호출 로그, VPC Flow Logs와 별개 ❌
- **기능 오해**: VPC Flow Logs를 CloudTrail로 캡처 불가 ❌

**D. S3 + Lifecycle 정책 + S3 Standard-IA 전환** ⭐
- **VPC Flow Logs 지원**: S3를 VPC Flow Logs 대상으로 직접 지원 ✅
- **90일 빈번 접근**: S3 Standard로 빠른 접근 ✅
- **90일 후 비용 절약**: S3 Standard-IA로 자동 전환 ✅
- **간헐적 접근**: IA에서도 즉시 접근 가능 ✅

### VPC Flow Logs 대상 옵션

```yaml
지원되는 대상:
✅ Amazon S3
✅ CloudWatch Logs
✅ Kinesis Data Firehose

지원되지 않는 대상:
❌ CloudTrail (API 로그 전용)
❌ Kinesis Data Streams
```

### S3 Lifecycle 정책

```yaml
최적 구성:
0-90일: S3 Standard
- 빈번한 접근에 최적화
- 빠른 응답 시간

90일 후: S3 Standard-IA
- 간헐적 접근에 적합
- 스토리지 비용 약 45% 절약
- 여전히 즉시 접근 가능
```
