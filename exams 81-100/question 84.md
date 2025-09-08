## Question #84
A company wants to reduce the cost of its existing three-tier web architecture. 
The web, application, and database servers are running on Amazon EC2 instances for the development, test, and production environments. 
The EC2 instances average 30% CPU utilization during peak hours and 10% CPU utilization during non-peak hours.
The production EC2 instances run 24 hours a day. 
The development and test EC2 instances run for at least 8 hours each day. 
The company plans to implement automation to stop the development and test EC2 instances when they are not in use.

Which EC2 instance purchasing solution will meet the company's requirements MOST cost-effectively?

A. Use Spot Instances for the production EC2 instances. Use Reserved Instances for the development and test EC2 instances.

B. Use Reserved Instances for the production EC2 instances. Use On-Demand Instances for the development and test EC2 instances.

C. Use Spot blocks for the production EC2 instances. Use Reserved Instances for the development and test EC2 instances.

D. Use On-Demand Instances for the production EC2 instances. Use Spot blocks for the development and test EC2 instances.

## Question #84 분석

### ✅ 요구사항
- **3계층 웹 아키텍처** 비용 절감
- **CPU 사용률**: 피크 30%, 비피크 10%
- **운영 패턴**:
  - Production: 24시간 연중무휴
  - Dev/Test: 최소 8시간/일, 자동화로 미사용시 중지
- **최고 비용 효율성** 달성

### ✅ 핵심 분석
```yaml
Production 환경:
✅ 24시간 안정적 운영 필요
✅ 예측 가능한 사용 패턴
✅ 중단 불가 (비즈니스 크리티컬)

Dev/Test 환경:
✅ 간헐적 사용 (8시간+)
✅ 중단 허용 가능
✅ 유연한 운영 패턴
```

### ✅ 선택지 분석

**A. Production Spot + Dev/Test Reserved**
- **Production Spot**: 중단 위험으로 부적절 ❌
- **Dev/Test Reserved**: 간헐적 사용에 비효율 ❌

**B. Production Reserved + Dev/Test On-Demand** ⭐
- **Production Reserved**: 24시간 운영에 최적 ✅
- **Dev/Test On-Demand**: 유연한 사용에 적합 ✅

**C. Production Spot Blocks + Dev/Test Reserved**
- **Spot Blocks**: 더 이상 제공되지 않는 서비스 

**D. Production On-Demand + Dev/Test Spot Blocks**
- **Spot Blocks**: 서비스 종료 

### 📋 인스턴스 타입별 특성

**Reserved Instances (1-3년 약정)**
- 할인: 최대 75%
- 용도: 안정적, 예측 가능한 워크로드
- Production 24시간 운영에 최적

**On-Demand**
- 할인: 없음
- 용도: 유연한 사용, 단기간
- Dev/Test 간헐적 사용에 적합

**Spot Instances**
- 할인: 최대 90%
- 위험: 언제든 중단 가능
- Production에 부적절

### 💰 비용 분석 예시
```yaml
Production (24시간 × 365일):
- On-Demand: $1,000/월
- Reserved: $400/월 (60% 할인)
- Spot: $100/월 (하지만 중단 위험)

Dev/Test (8시간 × 22일):
- On-Demand: $300/월
- Reserved: $400/월 (사용률 낮아 비효율)
- Spot: $30/월 (중단 허용 시)
```

**정답: B**

**핵심 이유**: Production은 안정적 운영을 위한 Reserved, Dev/Test는 유연한 사용을 위한 On-Demand가 최적의 비용 효율성을 제공합니다.