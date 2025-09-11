## Question #1013
A company's production environment consists of Amazon EC2 On-Demand Instances that run constantly between Monday and Saturday. 
The instances must run for only 12 hours on Sunday and cannot tolerate interruptions. The company wants to cost-optimize the production environment.

Which solution will meet these requirements MOST cost-effectively?

A. Purchase Scheduled Reserved Instances for the EC2 instances that run for only 12 hours on Sunday. Purchase Standard Reserved Instances for the EC2 instances that run constantly between Monday and Saturday.

B. Purchase Convertible Reserved Instances for the EC2 instances that run for only 12 hours on Sunday. Purchase Standard Reserved Instances for the EC2 instances that run constantly between Monday and Saturday.

C. Use Spot Instances for the EC2 instances that run for only 12 hours on Sunday. Purchase Standard Reserved Instances for the EC2 instances that run constantly between Monday and Saturday.

D. Use Spot Instances for the EC2 instances that run for only 12 hours on Sunday. Purchase Convertible Reserved Instances for the EC2 instances that run constantly between Monday and Saturday.

## Question #1013 분석

### 문제 상황
- **월-토**: 상시 실행 인스턴스
- **일요일**: 12시간만 실행
- **중단 불가** (운영 환경)
- **비용 최적화** 필요

### 핵심 요구사항
- **예측 가능한 워크로드** (월-토 상시)
- **짧은 시간 워크로드** (일요일 12시간)
- **중단 허용 안됨**
- **최대 비용 절약**

### EC2 인스턴스 타입별 특징

| 타입 | 비용 | 중단 위험 | 적합 용도 |
|------|------|-----------|----------|
| **On-Demand** | 높음 | 없음 | 예측 불가능한 워크로드 |
| **Reserved** | 낮음 | 없음 | 장기간 예측 가능한 워크로드 |
| **Spot** | 매우 낮음 | 있음 | 중단 허용 가능한 워크로드 |

### 선택지 분석

**A. Scheduled RI (일요일) + Standard RI (월-토)** ⭐
```yaml
월-토: Standard RI (1-3년 약정, 최대 72% 할인)
일요일: Scheduled RI (특정 시간대만, 5-10% 할인)
중단: 없음 ✅
비용: 최적화 ✅
```

**B. Convertible RI (일요일) + Standard RI (월-토)**
```yaml
문제: Convertible RI는 1-3년 약정인데 일요일 12시간용으로 부적합
비용: 비효율적 ❌
```

**C. Spot (일요일) + Standard RI (월-토)**
```yaml
문제: "중단 불가" 요구사항 위배
위험: Spot 인스턴스는 언제든 중단 가능 ❌
```

**D. Spot (일요일) + Convertible RI (월-토)**
```yaml
문제: 중단 불가 + Convertible RI는 Standard보다 비쌈
위험: 중단 + 높은 비용 ❌
```
