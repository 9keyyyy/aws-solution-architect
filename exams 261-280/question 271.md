## Question #271
A solutions architect observes that a nightly batch processing job is automatically scaled up for 1 hour before the desired Amazon EC2 capacity is reached. 
The peak capacity is the ‘same every night and the batch jobs always start at 1 AM. 
The solutions architect needs to find a cost-effective solution that will allow for the desired EC2 capacity to be reached quickly and allow the Auto Scaling group to scale down after the batch jobs are complete.

What should the solutions architect do to meet these requirements?

A. Increase the minimum capacity for the Auto Scaling group.

B. Increase the maximum capacity for the Auto Scaling group.

C. Configure scheduled scaling to scale up to the desired compute level.

D. Change the scaling policy to add more EC2 instances during each scaling operation.

## Question #271 분석

### 문제 상황
- **야간 배치 작업**이 매일 오전 1시 시작
- **원하는 용량 도달**까지 1시간 소요
- **피크 용량**은 매일 동일
- **배치 완료 후** 스케일 다운 필요

### 요구사항
- **빠른 용량 도달**
- **비용 효율성**
- **자동 스케일 다운**

### 선택지 분석

**A. 최소 용량 증가**
- **24시간 고용량**: 배치 작업 외 시간에도 높은 용량 유지 ❌
- **비용 비효율**: 불필요한 시간대 리소스 낭비 ❌

**B. 최대 용량 증가**
- **스케일 업 속도 무관**: 최대 용량 증가로는 초기 확장 속도 개선 안됨 ❌
- **근본 해결 실패**: 여전히 1시간 확장 시간 필요 ❌

**C. 스케줄링된 스케일링 설정** ⭐
- **예측 가능한 패턴**: 매일 오전 1시 정확한 시작 시간 ✅
- **사전 확장**: 배치 작업 시작 전 미리 용량 확보 ✅
- **즉시 가용**: 작업 시작과 동시에 필요 용량 제공 ✅
- **자동 스케일 다운**: 작업 완료 후 스케줄링으로 축소 ✅

**D. 스케일링 정책 수정**
- **부분적 개선**: 한 번에 더 많은 인스턴스 추가하지만 여전히 시간 소요 ❌
- **완전한 해결 아님**: 점진적 확장으로는 즉시 용량 확보 불가 ❌

### 스케줄링된 스케일링 구성

```yaml
스케줄 설정:
- 12:45 AM: 피크 용량으로 스케일 업
- 3:00 AM: 최소 용량으로 스케일 다운

효과:
- 오전 1시 배치 시작 시 이미 필요 용량 준비
- 작업 완료 후 자동 비용 절약
- 예측 가능한 패턴 활용
```
