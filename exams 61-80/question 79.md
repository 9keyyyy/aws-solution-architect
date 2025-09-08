## Question #79
A company is planning to use an Amazon DynamoDB table for data storage. 
The company is concerned about cost optimization. 
The table will not be used on most mornings. 
In the evenings, the read and write traffic will often be unpredictable. 
When traffic spikes occur, they will happen very quickly.

What should a solutions architect recommend?

A. Create a DynamoDB table in on-demand capacity mode.

B. Create a DynamoDB table with a global secondary index.

C. Create a DynamoDB table with provisioned capacity and auto scaling.

D. Create a DynamoDB table in provisioned capacity mode, and configure it as a global table.

## Question #79 분석

### ✅ 요구사항
- **DynamoDB 테이블** 사용 계획
- **비용 최적화** 우선 고려
- **사용 패턴**: 대부분의 아침 시간대 미사용
- **저녁 시간**: 예측 불가능한 읽기/쓰기 트래픽
- **트래픽 급증**: 매우 빠르게 발생

### ✅ 핵심 사용 패턴 분석
```yaml
트래픽 특성:
- 불규칙적 사용 패턴
- 저사용/무사용 기간 존재
- 예측 불가능한 급증
- 빠른 스파이크 발생

비용 최적화 요구:
- 저사용 시간 비용 최소화
- 급증 시 성능 보장
- 전체적 비용 효율성
```

### ✅ 선택지 분석

**A. DynamoDB On-Demand 용량 모드** ⭐
- **사용량 기반 과금**: 실제 사용한 읽기/쓰기만 과금 ✅
- **자동 확장**: 즉시 무제한 확장 (초 단위) ✅
- **예측 불가능한 워크로드**: 완벽하게 대응 ✅
- **비용 최적화**: 미사용 시간 비용 없음 ✅
- **운영 부담 없음**: 용량 계획 불필요 ✅
- **빠른 스파이크 대응**: 사전 프로비저닝 없이 즉시 확장 ✅

**B. Global Secondary Index (GSI) 추가**
- **기능적 요소**: 인덱스는 용량 모드와 별개 ❌
- **비용 최적화**: 용량 모드 문제 해결 안됨 ❌
- **트래픽 패턴**: 워크로드 변동성 해결 불가 ❌
- **부적절한 답**: 문제 요구사항과 무관 ❌

**C. Provisioned Capacity + Auto Scaling**
- **사전 프로비저닝**: 최소 용량 항상 과금 ❌
- **확장 지연**: 스케일링에 수분 소요 ❌
- **빠른 스파이크**: 즉시 대응 불가 ❌
- **비용 비효율**: 미사용 시간에도 기본 용량 과금 ❌
- **복잡성**: Auto Scaling 정책 설정 필요 ❌

**D. Provisioned Capacity + Global Table**
- **Global Table**: 다중 리전 복제 기능 ❌
- **요구사항 무관**: 리전 복제 필요성 언급 없음 ❌
- **비용 증가**: 여러 리전 운영으로 비용 상승 ❌
- **Provisioned 한계**: 여전히 사전 용량 계획 필요 ❌

### 📋 핵심 개념 정리

#### **DynamoDB 용량 모드 비교**
```yaml
On-Demand 모드:
✅ 사용량 기반 과금 (Read/Write 단위당)
✅ 자동 확장 (무제한, 즉시)
✅ 용량 계획 불필요
✅ 예측 불가능한 워크로드 최적
⚠️ Provisioned 대비 25% 높은 단가

Provisioned 모드:
✅ 예측 가능한 워크로드에 저렴
✅ 예약 용량으로 추가 할인
⚠️ 사전 용량 계획 필요
⚠️ 확장/축소에 시간 소요
❌ 미사용 시간에도 기본 용량 과금
```

#### **Auto Scaling 한계**
```yaml
Auto Scaling 특성:
- 확장 시간: 1-3분 소요
- 축소 시간: 15분 간격 제한
- 최소 용량: 항상 유지 (과금)
- 예측 기반: 과거 패턴 분석

급속한 트래픽 스파이크:
❌ 확장 지연으로 스로틀링 발생 가능
❌ 예측 불가능한 패턴에 부적합
❌ 미사용 시간 비용 비효율
```

#### **비용 최적화 분석**
```yaml
사용 패턴별 비용 (월간 예시):

시나리오: 아침 0% 사용, 저녁 100% 사용

On-Demand:
- 아침 시간: $0 (사용량 없음)
- 저녁 시간: $125 (실사용량 기준)
- 총 비용: $125/월

Provisioned + Auto Scaling:
- 최소 용량: $58 (24시간 기본 용량)
- 스케일링: $50 (확장 시)
- 총 비용: $108/월
- 단, 스파이크 시 성능 위험

결론: 불규칙 패턴에서는 On-Demand가 더 효율적
```

#### **트래픽 스파이크 대응**
```yaml
빠른 스파이크 시나리오:
트래픽이 1분 내 10배 증가

On-Demand:
✅ 즉시 대응 (지연 없음)
✅ 무제한 확장
✅ 성능 보장

Provisioned + Auto Scaling:
❌ 1-3분 확장 지연
❌ 초기 스로틀링 발생
❌ 사용자 경험 저하
```

#### **정답 시나리오 (A번)**
```yaml
1. DynamoDB 테이블 생성
   - Capacity Mode: On-Demand
   - 초기 설정만으로 완료
   - 용량 계획 불필요

2. 운영 특성
   아침 시간 (저사용):
   - 사용량: 거의 0
   - 비용: 거의 0
   - 성능: 필요시 즉시 제공

   저녁 시간 (고사용):
   - 사용량: 예측 불가
   - 비용: 실사용량만 과금
   - 성능: 자동 무제한 확장

3. 급속 스파이크 대응
   - 0초 지연: 즉시 확장
   - 무제한 용량: 어떤 스파이크도 처리
   - 자동 축소: 스파이크 후 즉시 비용 절감

4. 비용 최적화 결과
   ✅ 미사용 시간: 비용 0
   ✅ 사용 시간: 정확한 사용량만 과금
   ✅ 예측 불가능: 항상 최적 비용
   ✅ 운영 부담: 0 (완전 자동)
```


