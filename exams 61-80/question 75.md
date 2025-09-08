## Question #75
A company wants to move a multi-tiered application from on premises to the AWS Cloud to improve the application's performance. 
The application consists of application tiers that communicate with each other by way of RESTful services. 
Transactions are dropped when one tier becomes overloaded. 
A solutions architect must design a solution that resolves these issues and modernizes the application.

Which solution meets these requirements and is the MOST operationally efficient?

A. Use Amazon API Gateway and direct transactions to the AWS Lambda functions as the application layer. Use Amazon Simple Queue Service (Amazon SQS) as the communication layer between application services.

B. Use Amazon CloudWatch metrics to analyze the application performance history to determine the servers' peak utilization during the performance failures. Increase the size of the application server's Amazon EC2 instances to meet the peak requirements.

C. Use Amazon Simple Notification Service (Amazon SNS) to handle the messaging between application servers running on Amazon EC2 in an Auto Scaling group. Use Amazon CloudWatch to monitor the SNS queue length and scale up and down as required.

D. Use Amazon Simple Queue Service (Amazon SQS) to handle the messaging between application servers running on Amazon EC2 in an Auto Scaling group. Use Amazon CloudWatch to monitor the SQS queue length and scale up when communication failures are detected.

## Question #75 분석

### ✅ 요구사항
- **멀티 티어 애플리케이션** 온프레미스 → AWS 마이그레이션
- **RESTful 서비스**로 티어간 통신
- **트랜잭션 드롭** 문제: 과부하 시 통신 실패
- **성능 개선** 및 **애플리케이션 현대화**
- **운영 효율성 최대화** 필요

### ✅ 핵심 문제 분석
```yaml
현재 문제점:
❌ 티어간 직접 통신으로 결합도 높음
❌ 과부하 시 트랜잭션 손실
❌ 확장성 부족
❌ 장애 전파 위험

해결 방향:
✅ 비동기 통신으로 디커플링
✅ 메시지 큐잉으로 안정성 확보
✅ 자동 확장으로 탄력성 제공
✅ 현대적 아키텍처 패턴 적용
```

### ✅ 선택지 분석

**A. API Gateway + Lambda + SQS** ⭐
- **서버리스 아키텍처**: 완전 관리형 서비스 ✅
- **API Gateway**: RESTful API 관리 및 확장성 ✅
- **Lambda**: 자동 확장, 운영 부담 최소 ✅
- **SQS 디커플링**: 티어간 안정적 비동기 통신 ✅
- **트랜잭션 보장**: 메시지 큐로 데이터 손실 방지 ✅
- **운영 효율성**: 인프라 관리 불필요 ✅
- **현대화**: 완전한 클라우드 네이티브 아키텍처 ✅

**B. CloudWatch 분석 + EC2 크기 증설**
- **리액티브 접근**: 사후 대응 방식 ❌
- **수직 확장**: 확장성 한계 존재 ❌
- **직접 통신 유지**: 근본 문제 해결 안됨 ❌
- **트랜잭션 드롭**: 여전히 과부하 시 손실 위험 ❌
- **운영 부담**: 지속적 모니터링 및 조정 필요 ❌
- **현대화 미흡**: 기존 아키텍처 패턴 유지 ❌

**C. SNS + EC2 Auto Scaling**
- **잘못된 서비스**: SNS는 큐가 아닌 pub/sub ❌
- **큐 길이 모니터링**: SNS에는 큐 개념 없음 ❌
- **메시지 손실 위험**: SNS는 일회성 전달 ❌
- **기술적 오류**: SNS 특성 오해 ❌

**D. SQS + EC2 Auto Scaling**
- **SQS 디커플링**: 티어간 안정적 통신 ✅
- **Auto Scaling**: 자동 확장성 제공 ✅
- **EC2 관리**: 여전히 서버 관리 부담 ❌
- **운영 효율성**: A에 비해 상대적으로 낮음 ❌
- **현대화 수준**: 부분적 현대화 ⚠️

### 📋 핵심 개념 정리

#### **서버리스 vs 서버 기반 비교**
```yaml
서버리스 (A안 - Lambda):
  ✅ 자동 확장 (0 → 수천 동시 실행)
  ✅ 사용량 기반 과금
  ✅ 인프라 관리 불필요
  ✅ 고가용성 자동 보장
  ✅ 운영 부담 최소

서버 기반 (D안 - EC2):
  ⚠️ Auto Scaling 설정 필요
  ⚠️ 인스턴스 관리 필요
  ⚠️ 패치/보안 관리
  ⚠️ 용량 계획 필요
  ❌ 운영 부담 상대적 높음
```

#### **메시징 패턴 비교**
```yaml
Amazon SQS (큐):
  ✅ 메시지 지속성 보장
  ✅ Dead Letter Queue 지원
  ✅ 순서 보장 (FIFO)
  ✅ 재시도 메커니즘
  ✅ 백프레셔 처리

Amazon SNS (pub/sub):
  ✅ 팬아웃 패턴
  ✅ 실시간 알림
  ❌ 메시지 지속성 없음
  ❌ 큐 길이 개념 없음
  ❌ 재시도 제한적
```

#### **트랜잭션 드롭 해결 메커니즘**
```yaml
기존 문제:
Tier A → (직접 HTTP) → Tier B (과부하) → 드롭

SQS 해결책:
Tier A → SQS Queue → Tier B (처리 가능할 때)
- 메시지 보존: 최대 14일
- 백프레셔: 자동 대기열 관리
- 재시도: 자동 재처리
- DLQ: 실패 메시지 별도 처리
```

#### **정답 시나리오 (A번)**
```yaml
1. API Gateway 설정
   - RESTful API 엔드포인트 생성
   - Lambda 함수 통합
   - 자동 확장 및 부하 분산
   - 스로틀링으로 백엔드 보호

2. Lambda 함수 아키텍처
   - 각 티어를 별도 Lambda 함수로 구현
   - 이벤트 기반 처리
   - 자동 확장 (동시 실행수 자동 조정)
   - 상태 비저장 설계

3. SQS 통신 계층
   - 티어간 메시지 큐 구성
   - FIFO 큐로 순서 보장
   - Dead Letter Queue 설정
   - 메시지 가시성 타임아웃 최적화

4. 운영 결과
   ✅ 트랜잭션 손실 방지
   ✅ 자동 확장으로 성능 개선
   ✅ 운영 부담 최소화
   ✅ 비용 최적화 (사용량 기반)
   ✅ 고가용성 자동 보장
```

#### **현대화 아키텍처 패턴**
```yaml
Before (온프레미스):
[Web] ↔ [App] ↔ [DB]
- 직접 결합
- 확장성 제한
- 장애 전파

After (서버리스):
[API Gateway] → [Lambda] → [SQS] → [Lambda] → [RDS]
- 완전 디커플링
- 무제한 확장
- 장애 격리
- 메시지 지속성
```

#### **운영 효율성 비교**
```yaml
A안 (서버리스):
- 관리 대상: 코드만
- 확장: 자동
- 모니터링: CloudWatch 자동
- 패치: AWS 담당
- 가용성: 99.95%+ SLA

D안 (EC2):
- 관리 대상: EC2, OS, 애플리케이션
- 확장: Auto Scaling 설정 필요
- 모니터링: 커스텀 설정 필요
- 패치: 사용자 담당
- 가용성: 사용자 구성에 따라
```

#### **비용 최적화**
```yaml
서버리스 모델 (A안):
- 실행 시간만 과금
- 유휴 시간 비용 없음
- 자동 리소스 최적화
- 예측 가능한 비용

EC2 모델 (D안):
- 인스턴스 시간 과금
- 유휴 시간에도 비용
- 용량 계획 필요
- 과잉/부족 프로비저닝 위험
```
