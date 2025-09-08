## Question #85
A company has primary and secondary data centers that are 500 miles (804.7 km) apart and interconnected with high-speed fiber-optic cable. 
The company needs a highly available and secure network connection between its data centers and a VPC on AWS for a mission-critical workload. 
A solutions architect must choose a connection solution that provides maximum resiliency.

Which solution meets these requirements?

A. Two AWS Direct Connect connections from the primary data center terminating at two Direct Connect locations on two separate devices

B. A single AWS Direct Connect connection from each of the primary and secondary data centers terminating at one Direct Connect location on the same device

C. Two AWS Direct Connect connections from each of the primary and secondary data centers terminating at two Direct Connect locations on two separate devices

D. A single AWS Direct Connect connection from each of the primary and secondary data centers terminating at one Direct Connect location on two separate devices

## Question #85 분석 

### 📝 용어 설명
```yaml
DC (Data Center): 데이터센터
- 회사의 물리적 서버/네트워크 시설
- Primary DC: 주 데이터센터
- Secondary DC: 보조 데이터센터 (500마일 떨어짐)

DX (Direct Connect): AWS Direct Connect
- AWS와 온프레미스 간 전용 네트워크 연결
- Direct Connect Location: AWS 직접 연결 지점
- Device: 연결 장비 (라우터/스위치)
```

### ✅ 요구사항
- **미션 크리티컬** 워크로드
- **최고 가용성** 및 보안 연결
- **최대 복원력(resiliency)** 제공
- 주 데이터센터/보조 데이터센터 (500마일 떨어짐)

### ✅ 선택지 분석 (용어 풀어서)

**A. 주 데이터센터 → 2개 Direct Connect 지점 → 2개 장비**
- **단일 데이터센터 의존**: 주 데이터센터 장애 시 완전 중단 ❌
- **지리적 단일점**: 복원력 부족 ❌

**B. 양쪽 데이터센터 → 1개 Direct Connect 지점 → 같은 장비**
- **단일 Direct Connect 지점**: 해당 지점 장애 시 완전 중단 ❌
- **단일 장비**: 장비 장애 시 완전 중단 ❌

**C. 양쪽 데이터센터 → 2개 Direct Connect 지점 → 2개 장비** ⭐
- **다중 데이터센터**: 지리적 이중화 ✅
- **다중 Direct Connect 지점**: 지점별 이중화 ✅
- **다중 장비**: 장비 이중화 ✅
- **완전 이중화**: 모든 단일 장애점 제거 ✅

**D. 양쪽 데이터센터 → 1개 Direct Connect 지점 → 2개 장비**
- **단일 Direct Connect 지점**: 지점 장애 시 완전 중단 ❌
- **지리적 단일점**: 복원력 제한 ❌

### 📋 아키텍처 상세 설명

**C안 (최대 복원력) - 총 4개 연결**
```yaml
연결 구성:
1. 주 데이터센터 → Direct Connect 지점 A → 장비 1
2. 주 데이터센터 → Direct Connect 지점 B → 장비 2  
3. 보조 데이터센터 → Direct Connect 지점 A → 장비 3
4. 보조 데이터센터 → Direct Connect 지점 B → 장비 4

장애 시나리오 대응:
✅ 주 데이터센터 장애: 보조 데이터센터로 완전 대체
✅ Direct Connect 지점 A 장애: 지점 B로 트래픽 전환
✅ 특정 장비 장애: 다른 장비 사용
✅ 개별 연결 장애: 나머지 3개 연결로 서비스 지속
```
