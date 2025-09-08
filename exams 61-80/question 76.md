## Question #76
A company receives 10 TB of instrumentation data each day from several machines located at a single factory. 
The data consists of JSON files stored on a storage area network (SAN) in an on-premises data center located within the factory. 
The company wants to send this data to Amazon S3 where it can be accessed by several additional systems that provide critical near-real-time analytics. 
A secure transfer is important because the data is considered sensitive.

Which solution offers the MOST reliable data transfer?

A. AWS DataSync over public internet

B. AWS DataSync over AWS Direct Connect

C. AWS Database Migration Service (AWS DMS) over public internet

D. AWS Database Migration Service (AWS DMS) over AWS Direct Connect

## Question #76 분석

### ✅ 요구사항
- **10TB/일** 대용량 계측 데이터
- **팩토리 내 단일 위치**에서 생성
- **JSON 파일** 형태로 SAN에 저장
- **Amazon S3**로 전송 필요
- **실시간에 가까운 분석** 시스템에서 사용
- **보안 전송** 필수 (민감한 데이터)
- **최고 신뢰성** 요구

### ✅ 핵심 고려사항
```yaml
데이터 특성:
- 볼륨: 10TB/일 (대용량)
- 형태: JSON 파일 (파일 기반)
- 소스: SAN 스토리지 (파일 시스템)
- 목적지: Amazon S3 (객체 스토리지)

성능 요구사항:
- 일일 10TB 전송
- 시간당 ~416GB
- 실시간에 가까운 처리 필요

보안 요구사항:
- 민감한 데이터
- 안전한 전송 채널 필요
```

### ✅ 선택지 분석

**A. AWS DataSync over Public Internet**
- **적합한 서비스**: 파일 → S3 전송에 최적화 ✅
- **대용량 처리**: 10TB 일일 전송 가능 ✅
- **보안 위험**: 공개 인터넷으로 민감 데이터 전송 ❌
- **신뢰성 이슈**: 인터넷 대역폭 변동성 ❌
- **성능 제한**: ISP 대역폭 제약 ❌

**B. AWS DataSync over AWS Direct Connect** ⭐
- **최적 서비스**: 파일 시스템 → S3 전송 전용 ✅
- **전용 연결**: 안정적이고 예측 가능한 대역폭 ✅
- **보안 강화**: 전용 네트워크로 민감 데이터 보호 ✅
- **최고 신뢰성**: 전용선의 99.95% SLA ✅
- **성능 최적화**: 멀티스레드, 압축, 증분 동기화 ✅
- **실시간 지원**: 지속적 동기화 가능 ✅

**C. AWS DMS over Public Internet**
- **부적합한 서비스**: 데이터베이스 마이그레이션 전용 ❌
- **파일 시스템 미지원**: JSON 파일 SAN 처리 불가 ❌
- **용도 불일치**: 파일 전송이 아닌 DB 복제 서비스 ❌
- **기술적 불가능**: SAN 스토리지 연결 불가 ❌

**D. AWS DMS over AWS Direct Connect**
- **서비스 오해**: DMS는 데이터베이스 전용 ❌
- **파일 처리 불가**: JSON 파일 시스템 지원 안함 ❌
- **아키텍처 부적합**: 파일 → 객체 스토리지 용도 아님ㅇ ❌
- **연결 방식 무관**: 서비스 자체가 부적합 ❌

### 📋 핵심 개념 정리

#### **AWS DataSync vs AWS DMS**
```yaml
AWS DataSync:
  목적: 파일/객체 데이터 전송
  소스: NFS, SMB, HDFS, S3, EFS, FSx
  대상: S3, EFS, FSx
  방식: 파일 레벨 동기화
  ✅ JSON 파일 SAN → S3 최적

AWS DMS:
  목적: 데이터베이스 마이그레이션/복제
  소스: 관계형/NoSQL 데이터베이스
  대상: 관계형/NoSQL 데이터베이스
  방식: 스키마/데이터 변환
  ❌ 파일 시스템 처리 불가
```

#### **네트워크 연결 옵션 비교**
```yaml
Public Internet:
  ✅ 빠른 설정
  ✅ 낮은 초기 비용
  ❌ 보안 위험 (민감 데이터)
  ❌ 대역폭 변동성
  ❌ 성능 예측 어려움

AWS Direct Connect:
  ✅ 전용 연결 보안
  ✅ 일관된 성능
  ✅ 예측 가능한 대역폭
  ✅ 99.95% SLA
  ⚠️ 초기 설정 시간
  ⚠️ 높은 초기 비용
```

#### **DataSync 최적화 기능**
```yaml
성능 최적화:
  ✅ 멀티스레드 전송
  ✅ 네트워크 압축
  ✅ 증분 동기화
  ✅ 대역폭 제한 설정

데이터 무결성:
  ✅ 체크섬 검증
  ✅ 전송 중 검증
  ✅ 메타데이터 보존
  ✅ 타임스탬프 유지

보안 기능:
  ✅ 전송 중 암호화
  ✅ IAM 권한 관리
  ✅ VPC 엔드포인트 지원
  ✅ CloudTrail 로깅
```

#### **정답 시나리오 (B번)**
```yaml
1. AWS Direct Connect 설정
   - 팩토리에 10Gbps Direct Connect 구성
   - AWS와 전용 네트워크 연결
   - VPC와 Virtual Interface 설정

2. DataSync 구성
   - 온프레미스 DataSync 에이전트 설치
   - SAN 스토리지를 NFS/SMB로 마운트
   - S3 버킷을 대상으로 설정

3. 전송 작업 설정
   - 일일 스케줄 또는 실시간 동기화
   - 증분 전송으로 효율성 극대화
   - 암호화 및 압축 활성화

4. 모니터링 및 최적화
   - CloudWatch 메트릭 모니터링
   - 전송 속도 및 성공률 추적
   - 필요 시 대역폭 조정

운영 결과:
✅ 일일 10TB 안정적 전송
✅ 실시간에 가까운 동기화
✅ 민감 데이터 보안 보장
✅ 99.95% 가용성 SLA
✅ 예측 가능한 성능
```

