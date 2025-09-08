## Question #209
A solutions architect is designing the architecture of a new application being deployed to the AWS Cloud. 
The application will run on Amazon EC2 On-Demand Instances and will automatically scale across multiple Availability Zones. 
The EC2 instances will scale up and down frequently throughout the day. 
An Application Load Balancer (ALB) will handle the load distribution. 
The architecture needs to support distributed session data management. 
The company is willing to make changes to code if needed.

What should the solutions architect do to ensure that the architecture supports distributed session data management?

A. Use Amazon ElastiCache to manage and store session data.
B. Use session affinity (sticky sessions) of the ALB to manage session data.
C. Use Session Manager from AWS Systems Manager to manage the session.
D. Use the GetSessionToken API operation in AWS Security Token Service (AWS STS) to manage the session.

## Question #209 분석

### 요구사항
- **EC2 On-Demand 인스턴스** 자동 확장
- **멀티 AZ** 배포
- **빈번한 스케일링** (up/down)
- **ALB** 로드 분산
- **분산 세션 데이터 관리** 필요
- **코드 변경 허용**

### 선택지 분석

**A. Amazon ElastiCache로 세션 관리** ⭐
- **외부 세션 저장소**: 인스턴스 독립적 세션 관리 ✅
- **분산 아키텍처**: 모든 인스턴스가 공유 접근 ✅
- **스케일링 대응**: 인스턴스 생성/종료와 무관 ✅
- **고성능**: 인메모리 캐시로 빠른 세션 조회 ✅

**B. ALB 스티키 세션**
- **스케일링 문제**: 인스턴스 종료 시 세션 손실 ❌
- **분산 미지원**: 특정 인스턴스에 세션 고정 ❌
- **가용성 위험**: 인스턴스 장애 시 세션 유실 ❌

**C. Systems Manager Session Manager**
- **용도 불일치**: SSH/RDP 원격 접속용 서비스 ❌
- **애플리케이션 세션 관리 아님**: 웹 세션과 무관 ❌

**D. STS GetSessionToken**
- **AWS API 인증용**: 임시 자격 증명 발급 서비스 ❌
- **애플리케이션 세션과 무관**: 보안 토큰 관리용 ❌

### 분산 세션 관리 원칙

```yaml
요구사항:
- 인스턴스 독립성
- 확장성 지원
- 고가용성
- 빠른 접근

ElastiCache 장점:
✅ Redis/Memcached 지원
✅ 클러스터 모드 고가용성
✅ 밀리초 응답시간
✅ 자동 장애조치
```

### 아키텍처

```yaml
사용자 요청 → ALB → 임의 EC2 인스턴스
EC2 인스턴스 → ElastiCache에서 세션 조회
세션 데이터 모든 인스턴스 공유
인스턴스 스케일링과 독립적 운영
```
