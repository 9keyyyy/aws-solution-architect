## Question #238
A company wants to experiment with individual AWS accounts for its engineer team. 
The company wants to be notified as soon as the Amazon EC2 instance usage for a given month exceeds a specific threshold for each account.

What should a solutions architect do to meet this requirement MOST cost-effectively?

A. Use Cost Explorer to create a daily report of costs by service. Filter the report by EC2 instances. Configure Cost Explorer to send an Amazon Simple Email Service (Amazon SES) notification when a threshold is exceeded.

B. Use Cost Explorer to create a monthly report of costs by service. Filter the report by EC2 instances. Configure Cost Explorer to send an Amazon Simple Email Service (Amazon SES) notification when a threshold is exceeded.

C. Use AWS Budgets to create a cost budget for each account. Set the period to monthly. Set the scope to EC2 instances. Set an alert threshold for the budget. Configure an Amazon Simple Notification Service (Amazon SNS) topic to receive a notification when a threshold is exceeded.

D. Use AWS Cost and Usage Reports to create a report with hourly granularity. Integrate the report data with Amazon Athena. Use Amazon EventBridge to schedule an Athena query. Configure an Amazon Simple Notification Service (Amazon SNS) topic to receive a notification when a threshold is exceeded.

## Question #238 분석

### 요구사항
- **개별 AWS 계정**별 엔지니어 팀 실험
- **월별 EC2 사용량** 임계값 초과 시 알림
- **최고 비용 효율성**

### 선택지 분석

**A. Cost Explorer 일일 리포트 + SES**
- **기능 제한**: Cost Explorer는 알림 기능 제공하지 않음 ❌
- **SES 연동 불가**: Cost Explorer에서 직접 SES 전송 불가능 ❌

**B. Cost Explorer 월간 리포트 + SES**
- **동일한 문제**: Cost Explorer 알림 기능 없음 ❌
- **기술적 불가능**: 설명된 기능이 존재하지 않음 ❌

**C. AWS Budgets + SNS 알림** ⭐
- **예산 관리 전용**: Budgets는 비용 임계값 모니터링 전용 서비스 ✅
- **계정별 설정**: 각 계정에 개별 예산 생성 가능 ✅
- **EC2 필터링**: 서비스별 예산 범위 설정 지원 ✅
- **자동 알림**: 임계값 초과 시 SNS로 즉시 알림 ✅
- **비용 효율적**: Budgets는 처음 2개 예산 무료 ✅

**D. Cost and Usage Reports + Athena + EventBridge**
- **과도한 복잡성**: 여러 서비스 조합으로 구성 복잡 ❌
- **높은 비용**: Athena 쿼리 비용 + 추가 서비스 비용 ❌
- **운영 부담**: 커스텀 쿼리 및 스케줄링 관리 필요 ❌

### AWS Budgets 솔루션

```yaml
설정 과정:
1. 각 AWS 계정에서 Budget 생성
2. Budget 유형: Cost budget
3. 기간: Monthly
4. 필터: Service = Amazon Elastic Compute Cloud
5. 임계값 설정 (예: $100)
6. 알림 설정: SNS 토픽으로 이메일/SMS

비용:
- 처음 2개 예산: 무료
- 추가 예산: $0.02/일
- 매우 경제적인 솔루션
```
