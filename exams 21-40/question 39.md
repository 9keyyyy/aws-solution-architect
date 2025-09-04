## Question #39
A company maintains a searchable repository of items on its website. 
The data is stored in an Amazon RDS for MySQL database table that contains more than 10 million rows. 
The database has 2 TB of General Purpose SSD storage. 
There are millions of updates against this data every day through the company's website.
The company has noticed that some insert operations are taking 10 seconds or longer. 
The company has determined that the database storage performance is the problem.

Which solution addresses this performance issue?

A. Change the storage type to Provisioned IOPS SSD.

B. Change the DB instance to a memory optimized instance class.

C. Change the DB instance to a burstable performance instance class.

D. Enable Multi-AZ RDS read replicas with MySQL native asynchronous replication.

## Question #39 분석

### ✅ 요구사항
- **1천만 개 이상 행**의 MySQL 데이터베이스
- **2TB General Purpose SSD** 스토리지
- **일일 수백만 업데이트** (높은 쓰기 부하)
- **INSERT 작업 10초+ 지연** 발생
- **스토리지 성능이 문제**로 확인됨

### ✅ 선택지 분석

**A. Provisioned IOPS SSD로 변경** ⭐
- **스토리지 성능 문제 직접 해결**: 문제의 근본 원인 대응 ✅
- **높은 쓰기 IOPS**: INSERT 작업 성능 대폭 향상 ✅
- **일관된 성능**: 버스트 크레딧 고갈 문제 해결 ✅
- **대용량 워크로드**: 2TB + 수백만 쓰기에 최적 ✅

**B. 메모리 최적화 인스턴스**
- **CPU/메모리 업그레이드**: 스토리지 문제 해결 안됨 
- **근본 원인 미해결**: 여전히 스토리지 병목 존재
- 비용만 증가하고 효과 제한적

**C. 버스터블 성능 인스턴스**
- **성능 다운그레이드**: 문제를 더 악화시킴 
- **크레딧 기반 성능**: 높은 쓰기 부하에 부적합
- 완전 역방향 솔루션

**D. Multi-AZ + Read Replicas**
- **읽기 성능 개선**: INSERT 성능과 무관 
- **쓰기는 여전히 마스터**: 근본 문제 미해결 
- 복제 오버헤드로 쓰기 성능 더 저하 가능

#### 📋 스토리지 성능 이해

#### **General Purpose SSD (gp2/gp3) 한계**
```yaml
gp2 성능 특성:
  - 기본 IOPS: 3 IOPS per GB (최소 100, 최대 16,000)
  - 2TB = 6,000 IOPS 기본
  - 버스트 성능: 최대 3,000 IOPS (크레딧 기반)
  - 버스트 지속: 30분 (소진 시 기본 IOPS로 복귀)

문제점:
  ❌ 수백만 일일 쓰기 → 버스트 크레딧 빠르게 소진
  ❌ 6,000 IOPS로 복귀 → INSERT 성능 급격히 저하
  ❌ 일관되지 않은 성능 → 10초+ 지연 발생
```

#### **Provisioned IOPS SSD (io1/io2) 장점**
```yaml
성능 특성:
  - 프로비저닝된 IOPS: 최대 64,000 IOPS (io2)
  - 일관된 성능: 버스트 크레딧 개념 없음
  - 높은 처리량: GB당 최대 1,000MB/s
  - 지연 시간: 단일 자릿수 밀리초

2TB 워크로드 최적화:
  ✅ 20,000-30,000 IOPS 프로비저닝 가능
  ✅ 수백만 쓰기 작업 안정적 처리
  ✅ INSERT 지연 시간 100ms 이하로 감소
```

### 📊 INSERT 성능 병목 분석

#### **스토리지 성능이 INSERT에 미치는 영향**
```yaml
MySQL INSERT 과정:
  1. 데이터 검증 및 인덱스 확인 (메모리/CPU)
  2. 트랜잭션 로그 쓰기 (스토리지 I/O) ← 병목
  3. 데이터 페이지 쓰기 (스토리지 I/O) ← 병목  
  4. 인덱스 업데이트 (스토리지 I/O) ← 병목
  5. 트랜잭션 커밋 (스토리지 I/O) ← 병목

스토리지 IOPS 부족 시:
  - 단계 2-5에서 대기 시간 증가
  - 동시 INSERT들이 큐에서 대기
  - 전체 처리 시간 기하급수적 증가
```

#### **수치적 성능 개선 예상**
```yaml
현재 상황 (gp2, 크레딧 소진):
  - 가용 IOPS: ~6,000
  - INSERT 지연: 10초+
  - 동시 처리: 제한적

Provisioned IOPS (io2, 25,000 IOPS):
  - 가용 IOPS: 25,000 (4배 증가)
  - INSERT 지연: 100-500ms (95% 개선)
  - 동시 처리: 대폭 향상
```

### 🔍 다른 옵션들이 부적합한 이유

#### **메모리 최적화 인스턴스 (B번)**
```yaml
메모리 증가로 개선되는 것:
  ✅ 쿼리 캐싱
  ✅ 인덱스 캐싱
  ✅ 버퍼 풀 크기

INSERT 성능에 제한적 영향:
  - 새 데이터는 디스크 쓰기 필수
  - 트랜잭션 로그는 항상 디스크 I/O
  - 스토리지 병목은 여전히 존재
  
결론: 근본적 해결 불가 ❌
```

#### **Read Replicas (D번)**
```yaml
Read Replicas가 개선하는 것:
  ✅ SELECT 쿼리 분산
  ✅ 읽기 성능 향상
  ✅ 마스터 읽기 부하 감소

INSERT 성능에 미치는 영향:
  ❌ 모든 쓰기는 마스터에서만 처리
  ❌ 복제 오버헤드로 마스터 부하 증가
  ❌ 문제 해결 불가, 오히려 악화 가능
```

#### **버스터블 성능 (C번)**
```yaml
t3/t4g 인스턴스 특성:
  - CPU 크레딧 기반 성능
  - 기본 성능보다 낮은 베이스라인
  - 높은 부하 시 크레딧 소진 → 성능 급격히 저하

현재 워크로드 특성:
  - 이미 높은 부하 (수백만 업데이트)
  - 지속적 고성능 필요
  - 버스터블 인스턴스에 최악의 워크로드
```

