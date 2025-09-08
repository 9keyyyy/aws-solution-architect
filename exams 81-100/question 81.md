## Question #81
A solutions architect is designing the cloud architecture for a new application being deployed on AWS. 
The process should run in parallel while adding and removing application nodes as needed based on the number of jobs to be processed. 
The processor application is stateless. 
The solutions architect must ensure that the application is loosely coupled and the job items are durably stored.

Which design should the solutions architect use?

A. Create an Amazon SNS topic to send the jobs that need to be processed. Create an Amazon Machine Image (AMI) that consists of the processor application. Create a launch configuration that uses the AMI. Create an Auto Scaling group using the launch configuration. Set the scaling policy for the Auto Scaling group to add and remove nodes based on CPU usage.

B. Create an Amazon SQS queue to hold the jobs that need to be processed. Create an Amazon Machine Image (AMI) that consists of the processor application. Create a launch configuration that uses the AMI. Create an Auto Scaling group using the launch configuration. Set the scaling policy for the Auto Scaling group to add and remove nodes based on network usage.

C. Create an Amazon SQS queue to hold the jobs that need to be processed. Create an Amazon Machine Image (AMI) that consists of the processor application. Create a launch template that uses the AMI. Create an Auto Scaling group using the launch template. Set the scaling policy for the Auto Scaling group to add and remove nodes based on the number of items in the SQS queue.

D. Create an Amazon SNS topic to send the jobs that need to be processed. Create an Amazon Machine Image (AMI) that consists of the processor application. Create a launch template that uses the AMI. Create an Auto Scaling group using the launch template. Set the scaling policy for the Auto Scaling group to add and remove nodes based on the number of messages published to the SNS topic.

## Question #81 분석

### ✅ 요구사항
- **병렬 처리**: 여러 작업을 동시에 처리
- **동적 확장**: 작업 수에 따라 노드 추가/제거
- **스테이트리스**: 애플리케이션이 상태 정보 미보유
- **느슨한 결합**: 컴포넌트 간 독립성 확보
- **작업 항목 내구성**: 작업 데이터 안전하게 저장

### ✅ 핵심 아키텍처 요구사항
```yaml
작업 처리 패턴:
✅ 큐잉 시스템 (작업 대기열)
✅ 워커 노드 풀 (처리 인스턴스)
✅ 자동 확장 (작업량 기반)
✅ 내구성 보장 (작업 손실 방지)

느슨한 결합:
✅ 생산자 ↔ 소비자 분리
✅ 메시지 큐 중간 매개체
✅ 독립적 확장 가능
```

### ✅ 선택지 분석

**A. SNS + Auto Scaling (CPU 기반)**
- **SNS 문제**: 작업 지속성 없음 (일회성 전달) ❌
- **CPU 기반 확장**: 작업 수와 무관한 메트릭 ❌
- **작업 손실 위험**: SNS는 내구성 보장 안됨 ❌
- **느슨한 결합 부족**: 직접적인 푸시 방식 ❌

**B. SQS + Auto Scaling (네트워크 기반)**
- **SQS 적합성**: 작업 큐잉 및 내구성 보장 ✅
- **네트워크 기반 확장**: 작업 수와 무관한 메트릭 ❌
- **Launch Configuration**: 레거시 방식 ⚠️
- **확장 정책 부적절**: 네트워크 사용량은 부정확 ❌

**C. SQS + Auto Scaling (큐 길이 기반) + Launch Template** ⭐
- **SQS 큐잉**: 작업 내구성 및 순서 보장 ✅
- **큐 길이 기반 확장**: 실제 작업 수에 따른 정확한 확장 ✅
- **Launch Template**: 최신 방식, 더 유연한 설정 ✅
- **완벽한 느슨한 결합**: 큐 기반 디커플링 ✅
- **스테이트리스 적합**: 어떤 인스턴스든 큐에서 작업 처리 ✅

**D. SNS + Auto Scaling (메시지 발행 기반)**
- **SNS 한계**: 작업 지속성 없음 ❌
- **확장 메트릭**: SNS 발행 수는 실제 처리 대기와 무관 ❌
- **Launch Template**: 올바른 선택 ✅
- **내구성 부족**: 메시지 손실 위험 ❌

### 📋 핵심 개념 정리

#### **SQS vs SNS 작업 처리 용도**
```yaml
Amazon SQS (큐잉):
✅ 작업 대기열 (Work Queue Pattern)
✅ 메시지 지속성 (최대 14일)
✅ 중복 방지 (FIFO 큐)
✅ 가시성 타임아웃 (처리 중 보호)
✅ 재시도 메커니즘
✅ Dead Letter Queue

Amazon SNS (pub/sub):
✅ 즉시 알림 (Notification Pattern)
✅ 팬아웃 (1:N 전달)
❌ 메시지 지속성 없음
❌ 처리 실패 시 손실
❌ 작업 큐 용도 부적합
```

#### **Auto Scaling 메트릭 비교**
```yaml
CPU 사용률:
❌ 작업 수와 직접 연관 없음
❌ I/O 집약적 작업에서 부정확
❌ 처리 지연 반영 못함

네트워크 사용률:
❌ 작업 특성과 무관
❌ 대역폭과 작업 수 불일치
❌ 외부 요인 영향

SQS 큐 길이 (ApproximateNumberOfMessages):
✅ 실제 대기 작업 수 반영
✅ 처리 필요량 정확 측정
✅ 직관적이고 예측 가능
✅ 작업 처리 패턴과 완벽 매칭
```

#### **Launch Template vs Launch Configuration**
```yaml
Launch Template (권장):
✅ 최신 기능 지원
✅ 버전 관리 (템플릿 수정 이력)
✅ 다중 인스턴스 타입 지원
✅ Spot 인스턴스 통합
✅ 네트워크 인터페이스 고급 설정

Launch Configuration (레거시):
⚠️ 제한된 기능
⚠️ 버전 관리 없음
⚠️ 수정 불가 (새로 생성 필요)
⚠️ 향후 지원 종료 예정
```

#### **정답 시나리오 (C번)**
```yaml
1. SQS 큐 생성
   - Standard Queue (처리량 우선)
   - 또는 FIFO Queue (순서 보장 필요 시)
   - Message Retention: 14일
   - Visibility Timeout: 처리 시간 고려

2. AMI 생성
   - 처리 애플리케이션 설치
   - SQS 폴링 로직 포함
   - CloudWatch 에이전트 설정
   - 로그 수집 설정

3. Launch Template 생성
   {
     "LaunchTemplateName": "job-processor-template",
     "LaunchTemplateData": {
       "ImageId": "ami-12345678",
       "InstanceType": "t3.medium",
       "IamInstanceProfile": {
         "Name": "SQS-Reader-Role"
       },
       "UserData": "#!/bin/bash\n/opt/processor/start.sh"
     }
   }

4. Auto Scaling Group 설정
   - Launch Template 사용
   - 다중 AZ 배치
   - 최소/최대 인스턴스 수 설정

5. Scaling Policy 생성
   Target Tracking Policy:
   - Metric: ApproximateNumberOfMessages
   - Target Value: 5 (인스턴스당 5개 메시지)
   - Scale-out cooldown: 300초
   - Scale-in cooldown: 300초
```



#### **내구성 및 안정성**
```yaml
메시지 내구성:
✅ SQS 다중 AZ 복제
✅ 14일간 메시지 보존
✅ 처리 중 가시성 타임아웃
✅ 실패 시 자동 재대기열

처리 안정성:
✅ Dead Letter Queue 설정
✅ 재시도 정책 구성
✅ 독립적 인스턴스 처리
✅ 장애 격리 보장

확장성:
✅ 수평적 확장 (인스턴스 추가)
✅ 무제한 메시지 처리
✅ 자동 부하 분산
✅ 탄력적 비용 구조
```

