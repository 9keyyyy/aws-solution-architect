## Question #985
A company stores user data in AWS. The data is used continuously with peak usage during business hours. Access patterns vary, with some data not being used for months at a time. A solutions architect must choose a cost-effective solution that maintains the highest level of durability while maintaining high availability.

Which storage solution meets these requirements?

A. Amazon S3 Standard

B. Amazon S3 Intelligent-Tiering

C. Amazon S3 Glacier Deep Archive

D. Amazon S3 One Zone-Infrequent Access (S3 One Zone-IA)

## Question #985 분석

### 문제 상황
- **사용자 데이터** 지속적 사용
- **피크 타임**: 업무 시간
- **접근 패턴 다양**: 일부 데이터는 몇 달간 미사용
- **비용 효율성** + **최고 내구성** + **고가용성** 필요

### 핵심 요구사항
- **예측 불가능한 접근 패턴**
- **비용 최적화**
- **최고 내구성 (99.999999999%)**
- **고가용성**

### S3 스토리지 클래스 비교

| 스토리지 클래스 | 내구성 | 가용성 | 비용 | 접근 패턴 |
|----------------|--------|--------|------|-----------|
| **Standard** | 11 9's | 99.99% | 높음 | 자주 접근 |
| **Intelligent-Tiering** | 11 9's | 99.9% | 자동 최적화 | 변동적 |
| **Glacier Deep Archive** | 11 9's | 99.99% | 매우 낮음 | 거의 접근 안함 |
| **One Zone-IA** | 11 9's | 99.5% | 낮음 | 단일 AZ |

### 선택지 분석

**A. S3 Standard**
- 높은 내구성 ✅
- 높은 가용성 ✅
- 비용 최적화 부족 ❌ (자주 안 쓰는 데이터도 높은 비용)

**B. S3 Intelligent-Tiering** ⭐
- 최고 내구성 (11 9's) ✅
- 높은 가용성 ✅
- 자동 비용 최적화 ✅
- 다양한 접근 패턴에 완벽 적합 ✅

**C. Glacier Deep Archive**
- 최고 내구성 ✅
- 복원 시간 12시간 → 지속적 사용에 부적합 ❌

**D. One Zone-IA**
- 단일 AZ → 가용성 낮음 (99.5%) ❌
- "최고 가용성" 요구사항 미충족 ❌

### Intelligent-Tiering 작동 방식

```yaml
자동 계층 이동:
- 자주 접근: Standard
- 30일 미접근: IA
- 90일 미접근: Archive
- 180일 미접근: Deep Archive

장점:
- 모니터링 비용만 추가
- 복원 지연 없음
- 자동 최적화
```
