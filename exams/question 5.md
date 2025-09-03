## Question #5
A company is hosting a web application on AWS using a single Amazon EC2 instance that stores user-uploaded documents in an Amazon EBS volume. 
For better scalability and availability, the company duplicated the architecture and created a second EC2 instance and EBS volume in another Availability Zone, placing both behind an Application Load Balancer. 
After completing this change, users reported that, each time they refreshed the website, they could see one subset of their documents or the other, 
but never all of the documents at the same time.
What should a solutions architect propose to ensure users see all of their documents at once?

A. Copy the data so both EBS volumes contain all the documents <br>
B. Configure the Application Load Balancer to direct a user to the server with the documents <br>
C. Copy the data from both EBS volumes to Amazon EFS. Modify the application to save new documents to Amazon EFS <br>
D. Configure the Application Load Balancer to send the request to both servers. Return each document from the correct server <br>

### ✅ 요구사항:
- 웹 애플리케이션이 사용자 업로드 문서를 처리
- 확장성과 가용성을 위해 두 개의 AZ에 EC2 인스턴스 배치
- Application Load Balancer 뒤에 배치
- 사용자가 새로고침할 때마다 다른 문서 세트만 보이는 문제 발생
- 모든 문서를 동시에 볼 수 있어야 함

### ✅ 선택지 분석
A. 두 EBS 볼륨 모두에 데이터 복사하여 모든 문서 포함
- 데이터 일관성 문제 발생 가능
- 새 문서 업로드 시 실시간 동기화 어려움
- 스토리지 비용 2배 증가
- 복잡한 동기화 메커니즘 필요

B. 문서가 있는 서버로 사용자를 직접 라우팅하도록 ALB 구성
- Sticky Session 방식이지만 근본적 해결책 아님
- 특정 서버 장애 시 해당 문서들 접근 불가
- 확장성 제약 (서버별로 고정된 사용자)
- 로드 분산 효과 감소

**C. 두 EBS 볼륨 데이터를 Amazon EFS로 복사, 애플리케이션이 EFS에 저장하도록 수정** ⭐
- 완전한 공유 스토리지 솔루션
- 모든 EC2 인스턴스가 동일한 파일시스템 접근
- 자동 확장 및 고가용성
- 새 문서도 즉시 모든 인스턴스에서 접근 가능

D. 요청을 두 서버 모두에 보내고 각 서버에서 올바른 문서 반환
```
Client → ALB → Server A (문서 1, 3, 5 검색)
             → Server B (문서 2, 4, 6 검색)
             → 결과 병합 → Client 응답
```
- 네트워크 트래픽 2배 증가
- 응답 병합 로직의 복잡성
- 성능 저하 및 지연 시간 증가
- 애플리케이션 로직 대폭 수정 필요

### ✅ 개념 정리
### 1. 문제의 근본 원인
#### 1.1 현재 아키텍처
```
ALB → EC2 Instance A (AZ-1) → EBS Volume A (문서 세트 1)
    → EC2 Instance B (AZ-2) → EBS Volume B (문서 세트 2)
```
#### 1.2 문제 상황
- 라운드 로빈 로드 밸런싱: 요청이 번갈아 두 인스턴스로 분산
- 독립적인 스토리지: 각 EBS는 해당 인스턴스에만 연결
- 데이터 분할: 사용자 문서가 두 볼륨에 나뉘어 저장
- 일관성 없는 뷰: 접속할 때마다 다른 인스턴스의 다른 데이터 세트 확인


### 2. 스토리지 옵션 비교
#### 2.1 Amazon EBS (Elastic Block Store) - 현재 사용

- 특징
    - 단일 AZ 내에서만 사용 가능
    - 하나의 EC2 인스턴스에만 연결 (Multi-Attach 제외)
    - 고성능 블록 스토리지
    - 스냅샷을 통한 백업
- 한계
    - 공유 불가능: 여러 인스턴스 간 데이터 공유 어려움
    - AZ 종속성: 다른 AZ의 인스턴스와 연결 불가
    - 확장성 제약: 인스턴스 수 증가 시 데이터 분산 문제

#### 2.2 Amazon EFS (Elastic File System) - 권장 솔루션

- 핵심 특징
    - 다중 AZ 지원: 여러 AZ의 인스턴스가 동시 접근
    - 공유 파일 시스템: 무제한 동시 연결
    - 자동 확장: 파일 추가/삭제에 따라 용량 자동 조정
    - POSIX 호환: 기존 파일 시스템 API 그대로 사용
- 아키텍처
  ```
  ALB → EC2 Instance A (AZ-1) ↘
                                  → Amazon EFS (모든 문서)
      → EC2 Instance B (AZ-2) ↗
  ```
- 장점
    - 일관된 데이터 뷰: 모든 인스턴스가 동일한 파일 시스템 접근
    - 자동 동기화: 파일 변경사항 즉시 모든 인스턴스에 반영
    - 고가용성: 여러 AZ에 자동 복제
    - 확장성: 인스턴스 추가 시에도 동일한 데이터 접근
  
