## Question #47
A company needs guaranteed Amazon EC2 capacity in three specific Availability Zones in a specific AWS Region for an upcoming event that will last 1 week.

What should the company do to guarantee the EC2 capacity?

A. Purchase Reserved Instances that specify the Region needed.

B. Create an On-Demand Capacity Reservation that specifies the Region needed.

C. Purchase Reserved Instances that specify the Region and three Availability Zones needed.

D. Create an On-Demand Capacity Reservation that specifies the Region and three Availability Zones needed.

## Question #47 분석

### ✅ 요구사항
- **특정 3개 AZ**에서 EC2 용량 보장
- **특정 AWS 리전**
- **1주일 행사** 기간 동안
- **보장된 용량** 필요

### ✅ 선택지 분석

**A. Reserved Instances (리전 지정)**
- **용량 보장 없음**: RI는 할인 혜택만 제공 
- **AZ 지정 불가**: 리전 레벨만 지정
- **1년 최소 약정**: 1주일 용도에 부적합 

**B. On-Demand Capacity Reservation (리전 지정)**
- **용량 보장**: Capacity Reservation의 핵심 기능 ✅
- **AZ 지정 불가**: 리전 레벨로는 특정 AZ 보장 안됨 
- **단기 사용**: 1주일 사용에 적합 ✅

**C. Reserved Instances (리전 + 3개 AZ 지정)**
- **기술적 불가능**: RI는 AZ별 지정 불가 
- **용량 보장 없음**: 할인만 제공, 용량 보장 안함 
- **잘못된 구성**: 존재하지 않는 옵션

**D. On-Demand Capacity Reservation (리전 + 3개 AZ 지정)** ⭐
- **용량 보장**: Capacity Reservation의 목적 ✅
- **AZ별 지정**: 각 AZ에 개별적으로 용량 예약 ✅
- **단기 예약**: 필요한 기간만 예약 가능 ✅
- **즉시 사용**: 예약 즉시 용량 확보 ✅

### 📋 Capacity Reservation vs Reserved Instances

#### **On-Demand Capacity Reservation**
```yaml
목적: 용량 보장 (인스턴스 가용성 확보)
기간: 시간 단위로 유연한 예약
요금: On-Demand 가격 (할인 없음)
AZ 지정: 특정 AZ별 개별 예약 가능
취소: 언제든 즉시 취소 가능
```

#### **Reserved Instances**
```yaml
목적: 비용 절약 (할인 혜택)
기간: 1년 또는 3년 약정
요금: 최대 75% 할인
AZ 지정: 불가능 (리전 레벨만)
용량 보장: 없음 (할인만 제공)
```
