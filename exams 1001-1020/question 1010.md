## Question #1010
A company runs multiple workloads on virtual machines (VMs) in an on-premises data center. The company is expanding rapidly. The on-premises data center is not able to scale fast enough to meet business needs. The company wants to migrate the workloads to AWS.

The migration is time sensitive. The company wants to use a lift-and-shift strategy for non-critical workloads.

Which combination of steps will meet these requirements? (Choose three.)

A. Use the AWS Schema Conversion Tool (AWS SCT) to collect data about the VMs.

B. Use AWS Application Migration Service. Install the AWS Replication Agent on the VMs.

C. Complete the initial replication of the VMs. Launch test instances to perform acceptance tests on the VMs.

D. Stop all operations on the VMs. Launch a cutover instance.

E. Use AWS App2Container (A2C) to collect data about the VMs.

F. Use AWS Database Migration Service (AWS DMS) to migrate the VMs.

## Question #1010 분석

### 문제 상황
- **VM 워크로드**를 AWS로 마이그레이션
- **시간에 민감한** 마이그레이션
- **Lift-and-shift** 전략 (최소 변경)

### 핵심 요구사항
- **VM 그대로 이전**
- **빠른 마이그레이션**
- **non-critical 워크로드**

### VM 마이그레이션 프로세스

**AWS Application Migration Service (MGN)**가 표준 도구

### 선택지 분석

**A. AWS SCT로 VM 데이터 수집**
- SCT는 **데이터베이스 스키마 변환** 도구 ❌
- VM 마이그레이션과 무관 ❌

**B. MGN + Replication Agent 설치** ⭐
- VM 마이그레이션 표준 도구 ✅
- Agent로 실시간 복제 ✅

**C. 초기 복제 + 테스트 인스턴스** ⭐
- 복제 완료 후 테스트 필수 ✅
- 검증 단계 ✅

**D. VM 중지 + Cutover 인스턴스** ⭐
- 최종 전환 단계 ✅
- 다운타임 최소화 ✅

**E. App2Container로 VM 데이터 수집**
- A2C는 **컨테이너화** 도구 ❌
- Lift-and-shift와 맞지 않음 ❌

**F. DMS로 VM 마이그레이션**
- DMS는 **데이터베이스 마이그레이션** 전용 ❌
- VM 마이그레이션 불가 ❌

### MGN 마이그레이션 단계

```yaml
1. Replication Agent 설치 (B)
2. 초기 복제 + 테스트 (C)  
3. Cutover 실행 (D)
```

