## Question #48
A company's website uses an Amazon EC2 instance store for its catalog of items. 
The company wants to make sure that the catalog is highly available and that the catalog is stored in a durable location.

What should a solutions architect do to meet these requirements?

A. Move the catalog to Amazon ElastiCache for Redis.

B. Deploy a larger EC2 instance with a larger instance store.

C. Move the catalog from the instance store to Amazon S3 Glacier Deep Archive.

D. Move the catalog to an Amazon Elastic File System (Amazon EFS) file system.

## Question #48 분석

### 요구사항
- 웹사이트가 **EC2 Instance Store**에 카탈로그 저장
- **고가용성** 필요
- **내구성 있는 위치**에 저장 필요

### 선택지 분석

**A. ElastiCache for Redis로 이동**
- **휘발성 메모리**: 캐시 용도로 설계됨 
- **내구성 부족**: 클러스터 재시작 시 데이터 손실 
- **비용 비효율**: 카탈로그 영구 저장용으로 부적합 

**B. 더 큰 EC2 Instance Store**
- **여전히 휘발성**: Instance Store 특성 변하지 않음 
- **내구성 없음**: 인스턴스 종료 시 데이터 손실 
- **근본 문제 미해결**: 크기만 키울 뿐 

**C. S3 Glacier Deep Archive로 이동**
- **아카이브 저장소**: 즉시 접근 불가 (12-48시간 복원 시간) 
- **웹사이트 부적합**: 실시간 카탈로그 조회 불가능 
- **접근성 문제**: 고가용성 요구사항 위배 

**D. Amazon EFS로 이동** ⭐
- **내구성**: 99.999999999% 데이터 내구성 ✅
- **고가용성**: 다중 AZ 자동 복제 ✅
- **즉시 접근**: 파일시스템으로 실시간 접근 ✅
- **확장성**: 자동 용량 확장 ✅

### Instance Store vs EFS 비교

### **EC2 Instance Store 문제점**
```yaml
휘발성 스토리지:
  - 인스턴스 중지/종료 시 데이터 손실
  - 하드웨어 장애 시 복구 불가능
  - 백업 메커니즘 없음
  - 재부팅 시에만 데이터 유지

고가용성 부족:
  - 단일 인스턴스 의존
  - AZ 장애 시 접근 불가
  - 자동 장애 조치 없음
```

### **Amazon EFS 장점**
```yaml
내구성 및 가용성:
  - 다중 AZ에 자동 복제
  - 99.999999999% 내구성
  - 자동 백업 및 복원
  - AZ 장애 시에도 접근 가능

파일시스템 인터페이스:
  - POSIX 호환 파일시스템
  - 여러 EC2에서 동시 마운트
  - 기존 애플리케이션 최소 수정
  - 실시간 읽기/쓰기 성능
```

### 다른 옵션들의 한계

### **ElastiCache 부적합성 (A번)**
```yaml
설계 목적:
  - 임시 캐시 저장소
  - 빠른 읽기 성능 최적화
  - 메모리 기반 휘발성

카탈로그 저장 문제점:
  - 영구 저장소 아님
  - 클러스터 재시작 시 초기화
  - 비용 대비 비효율적
```

### **Glacier Deep Archive 접근성 (C번)**
```yaml
복원 시간:
  - Standard: 12시간
  - Bulk: 48시간
  - 즉시 접근 불가능

웹사이트 요구사항:
  - 실시간 카탈로그 조회
  - 밀리초 단위 응답 필요
  - 사용자 경험상 부적합
```

### 실제 마이그레이션 과정

### **Instance Store → EFS 이동**
```yaml
1. EFS 파일시스템 생성
2. 기존 EC2에 EFS 마운트
3. 카탈로그 데이터 복사
4. 애플리케이션 경로 수정
5. Instance Store 사용 중단
```

### **고가용성 확보**
```yaml
EFS 활용:
  - 여러 AZ의 EC2에서 동일 EFS 접근
  - Auto Scaling Group과 연동
  - 로드밸런서 뒤의 다중 인스턴스
  - 단일 장애점 제거
```
