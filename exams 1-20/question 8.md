## Question #8
A company is migrating a distributed application to AWS. 
The application serves variable workloads. 
The legacy platform consists of a primary server that coordinates jobs across multiple compute nodes. 
The company wants to modernize the application with a solution that maximizes resiliency and scalability.

How should a solutions architect design the architecture to meet these requirements?

A. Configure an Amazon Simple Queue Service (Amazon SQS) queue as a destination for the jobs. Implement the compute nodes with Amazon EC2 instances that are managed in an Auto Scaling group. Configure EC2 Auto Scaling to use scheduled scaling.

B. Configure an Amazon Simple Queue Service (Amazon SQS) queue as a destination for the jobs. Implement the compute nodes with Amazon EC2 instances that are managed in an Auto Scaling group. Configure EC2 Auto Scaling based on the size of the queue.

C. Implement the primary server and the compute nodes with Amazon EC2 instances that are managed in an Auto Scaling group. Configure AWS CloudTrail as a destination for the jobs. Configure EC2 Auto Scaling based on the load on the primary server.

D. Implement the primary server and the compute nodes with Amazon EC2 instances that are managed in an Auto Scaling group. Configure Amazon EventBridge (Amazon CloudWatch Events) as a destination for the jobs. Configure EC2 Auto Scaling based on the load on the compute nodes.


### **✅ 요구사항:**
* 분산 애플리케이션을 AWS로 마이그레이션
* 가변적인 워크로드를 처리
* 기존: 기본 서버가 여러 컴퓨팅 노드에 작업을 조정
* **복원력(resiliency)과 확장성(scalability) 최대화** 필요

### **✅ 선택지 분석**

**A. SQS 큐 + EC2 Auto Scaling Group + 스케줄 기반 스케일링**
* SQS 큐를 작업 대상으로 사용 ✅
* 컴퓨팅 노드를 Auto Scaling Group으로 관리 ✅
* **스케줄 기반 스케일링** ❌ - 가변 워크로드에 부적합
* 실제 큐 크기와 무관한 스케일링으로 비효율적

**B. SQS 큐 + EC2 Auto Scaling Group + 큐 크기 기반 스케일링** ⭐
* SQS 큐를 작업 대상으로 사용 ✅
* **큐 크기 기반 동적 스케일링** ✅ - 실제 워크로드에 따른 반응형 스케일링
* 기본 서버 제거로 **단일 실패점 해결** ✅
* 완벽한 디커플링과 확장성 제공

**C. 기본 서버 + 컴퓨팅 노드를 Auto Scaling Group + CloudTrail**
* **CloudTrail은 감사 로깅 서비스** - 작업 큐 용도 부적합 ❌
* 기본 서버 유지로 **단일 실패점** 문제 해결 안됨 ❌
* 근본적으로 잘못된 서비스 선택

**D. 기본 서버 + 컴퓨팅 노드를 Auto Scaling Group + EventBridge**
* EventBridge는 이벤트 라우팅 서비스 - 작업 큐로는 부적합
* 기본 서버 유지로 **단일 실패점** 문제 지속 ❌
* 컴퓨팅 노드 부하 기반 스케일링은 지연된 반응

### **✅ 개념 정리**

### **1. 현재 아키텍처의 문제점**

**1.1 기존 레거시 구조**
```
Primary Server (단일 실패점)
    ↓ 작업 분배
Compute Node 1 ← 작업 조정 → Compute Node 2 ← → Compute Node 3
```

**1.2 문제점**
* **단일 실패점(SPOF)**: 기본 서버 장애 시 전체 시스템 중단
* **확장성 제약**: 기본 서버가 병목 지점
* **복원력 부족**: 노드 장애 시 작업 손실 위험
* **수동 관리**: 워크로드 변화에 대한 수동 대응

**2. Amazon SQS 기반 현대화 (B번 솔루션)**

**2.1 현대화된 아키텍처**
```
Job Producer → SQS Queue → Auto Scaling Group
                             ↓
                    EC2 Instance 1 (polls SQS)
                    EC2 Instance 2 (polls SQS)  
                    EC2 Instance 3 (polls SQS)
                    ...
                    EC2 Instance N (동적 확장)
```

**2.2 SQS의 핵심 역할**
* **작업 버퍼링**: 작업 요청의 임시 저장소
* **디커플링**: 생산자와 소비자 완전 분리
* **내구성**: 메시지 손실 방지 (최소 한 번 전달 보장)
* **확장성**: 무제한 메시지 처리 능력


### **3. 큐 크기 기반 Auto Scaling**

- **CloudWatch 메트릭 기반 스케일링**
- **Auto Scaling 정책 구성**
- **스케일링 동작**
    ```
    큐에 메시지 많음 (>5개/인스턴스) → Scale Out
    큐에 메시지 적음 (<5개/인스턴스) → Scale In

    실제 워크로드에 따른 반응형 스케일링 ✅
    ```

### **4. 잘못된 선택지들 분석**

**4.1 A번: 스케줄 기반 스케일링의 문제**
```
스케줄 기반 스케일링:
- 오전 9시: 인스턴스 10개로 확장
- 오후 6시: 인스턴스 2개로 축소

문제점:
❌ 실제 워크로드와 무관한 스케일링
❌ 예상치 못한 워크로드 급증 시 대응 불가
❌ 비용 비효율 (불필요한 리소스 운영)
```

**4.2 C번: CloudTrail 오용**
```
CloudTrail의 실제 목적:
✅ API 호출 로깅
✅ 보안 감사
✅ 규정 준수

❌ 작업 큐나 메시지 전달 (완전 다른 서비스)
```

**4.3 D번: EventBridge의 한계**
```
EventBridge 특성:
✅ 이벤트 기반 아키텍처
✅ 서비스 간 이벤트 라우팅
✅ 규칙 기반 이벤트 필터링

한계:
❌ 작업 큐로는 부적합 (메시지 지속성 부족)
❌ 여전히 기본 서버 단일 실패점 존재
```
