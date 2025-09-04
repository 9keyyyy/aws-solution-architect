## Question #24
A company observes an increase in Amazon EC2 costs in its most recent bill. 
The billing team notices unwanted vertical scaling of instance types for a couple of EC2 instances. 
A solutions architect needs to create a graph comparing the last 2 months of EC2 costs and perform an in-depth analysis to identify the root cause of the vertical scaling.

How should the solutions architect generate the information with the LEAST operational overhead?

A. Use AWS Budgets to create a budget report and compare EC2 costs based on instance types.

B. Use Cost Explorer's granular filtering feature to perform an in-depth analysis of EC2 costs based on instance types.

C. Use graphs from the AWS Billing and Cost Management dashboard to compare EC2 costs based on instance types for the last 2 months.

D. Use AWS Cost and Usage Reports to create a report and send it to an Amazon S3 bucket. Use Amazon QuickSight with Amazon S3 as a source to generate an interactive graph based on instance types.

## Question #24 분석

### ✅ 요구사항
- EC2 비용 증가 분석
- **지난 2개월간 비교** 그래프 생성
- **인스턴스 타입별** 세부 분석
- **수직 확장(Vertical Scaling)** 원인 규명
- **최소한의 운영 오버헤드**

### ✅ 선택지 분석

**A. AWS Budgets**
- **예산 관리**: 비용 추적 및 알림 도구 
- **분석 기능 제한**: 상세 분석 도구 아님
- 예산 대비 지출 모니터링 중심
- 인스턴스 타입별 세부 분석 어려움

**B. Cost Explorer 세밀한 필터링** ⭐
- **내장 분석 도구**: AWS 네이티브 서비스 ✅
- **인스턴스 타입별 필터링**: 완벽 지원 ✅
- **2개월 비교**: 기간별 비교 그래프 ✅
- **운영 오버헤드**: 설정 없이 즉시 사용 ✅

**C. AWS Billing Dashboard 그래프**
- **기본 그래프**: 상세 분석 기능 제한 
- 인스턴스 타입별 세부 분석 불가
- 대시보드는 전반적인 개요만 제공

**D. Cost and Usage Reports + S3 + QuickSight**
- **높은 운영 오버헤드**: 복잡한 설정 필요 
- 리포트 생성 + S3 설정 + QuickSight 구성
- 과도하게 복잡한 솔루션

### 📋 AWS Cost Explorer 핵심 기능

### **Cost Explorer의 분석 기능**
```yaml
시간 범위 분석:
  - 최대 12개월 히스토리
  - 일/주/월 단위 그룹화
  - 기간별 비교 그래프

필터링 기능:
  - 서비스별 (EC2, RDS, S3 등)
  - 인스턴스 타입별 (t3.micro, m5.large 등)
  - 가용 영역별
  - 태그별 분석

시각화:
  - 막대 그래프, 선 그래프
  - 스택형 차트
  - 테이블 뷰
  - 비용 분해 분석
```

### **인스턴스 타입 분석 시나리오**
```yaml
분석 과정:
  1. Cost Explorer 접속
  2. 서비스 필터: Amazon EC2
  3. 기간 설정: 지난 2개월
  4. 그룹화: Instance Type
  5. 결과: 인스턴스 타입별 비용 추이 그래프

식별 가능한 정보:
  - t3.medium → m5.large 변경 시점
  - 비용 증가 규모 ($50/월 → $200/월)
  - 영향받은 인스턴스 수
  - 변경 일자 및 패턴
```


### 🔍 다른 옵션들의 한계

### **AWS Budgets (A번)**
```yaml
제한사항:
  ❌ 예산 관리 도구 (분석 도구 아님)
  ❌ 인스턴스 타입별 세부 분석 불가
  ❌ 히스토리컬 비교 기능 제한
  ❌ 그래프 생성 기능 부족

적합한 용도:
  - 예산 설정 및 알림
  - 비용 임계값 모니터링
```

### **Billing Dashboard (C번)**
```yaml
제한사항:
  ❌ 기본적인 개요만 제공
  ❌ 필터링 기능 제한
  ❌ 인스턴스 타입별 드릴다운 불가
  ❌ 상세 분석 도구 부족

적합한 용도:
  - 전체 비용 개요 확인
  - 월간 청구서 요약
```

### **CUR + QuickSight (D번)**
```yaml
오버킬 사유:
  ❌ 단순 분석에 과도한 복잡성
  ❌ 높은 설정 및 운영 비용
  ❌ 전문 지식 요구
  ❌ 실시간성 부족 (24시간 지연)

적합한 용도:
  - 복잡한 다차원 분석
  - 커스텀 대시보드 필요시
  - 대규모 조직의 정기 리포팅
```
