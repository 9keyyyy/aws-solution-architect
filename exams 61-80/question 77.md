## Question #77
A company needs to configure a real-time data ingestion architecture for its application. 
The company needs an API, a process that transforms data as the data is streamed, and a storage solution for the data.

Which solution will meet these requirements with the LEAST operational overhead?

A. Deploy an Amazon EC2 instance to host an API that sends data to an Amazon Kinesis data stream. Create an Amazon Kinesis Data Firehose delivery stream that uses the Kinesis data stream as a data source. Use AWS Lambda functions to transform the data. Use the Kinesis Data Firehose delivery stream to send the data to Amazon S3.

B. Deploy an Amazon EC2 instance to host an API that sends data to AWS Glue. Stop source/destination checking on the EC2 instance. Use AWS Glue to transform the data and to send the data to Amazon S3.

C. Configure an Amazon API Gateway API to send data to an Amazon Kinesis data stream. Create an Amazon Kinesis Data Firehose delivery stream that uses the Kinesis data stream as a data source. Use AWS Lambda functions to transform the data. Use the Kinesis Data Firehose delivery stream to send the data to Amazon S3.

D. Configure an Amazon API Gateway API to send data to AWS Glue. Use AWS Lambda functions to transform the data. Use AWS Glue to send the data to Amazon S3.

## Question #77 분석

### ✅ 요구사항
- **실시간 데이터 수집** 아키텍처 구성
- **API**: 데이터 수집 엔드포인트
- **스트림 데이터 변환**: 실시간 데이터 처리
- **스토리지 솔루션**: 변환된 데이터 저장
- **최소 운영 오버헤드**: 관리 부담 최소화

### ✅ 핵심 아키텍처 구성요소
```yaml
필요 구성요소:
1. API: 데이터 수집 엔드포인트
2. 스트리밍: 실시간 데이터 파이프라인  
3. 변환: 데이터 처리/변환
4. 저장: 최종 데이터 저장소

운영 효율성:
✅ 관리형 서비스 우선
✅ 서버리스 아키텍처
✅ 자동 확장
✅ 최소 인프라 관리
```

### ✅ 선택지 분석

**A. EC2 API + Kinesis Data Streams + Firehose + Lambda**
- **EC2 API**: 서버 관리 필요 ❌
- **Kinesis 조합**: 적절한 스트리밍 아키텍처 ✅
- **Lambda 변환**: 서버리스 처리 ✅
- **S3 저장**: 적절한 저장소 ✅
- **운영 부담**: EC2 관리로 인한 오버헤드 ❌

**B. EC2 API + AWS Glue**
- **EC2 API**: 서버 관리 부담 ❌
- **AWS Glue**: 배치 처리용, 실시간 스트리밍 부적합 ❌
- **실시간 처리**: Glue는 실시간 스트림 처리 불가 ❌
- **아키텍처 부적합**: 요구사항과 맞지 않음 ❌

**C. API Gateway + Kinesis Data Streams + Firehose + Lambda** ⭐
- **API Gateway**: 완전 관리형 API 서비스 ✅
- **Kinesis Data Streams**: 실시간 스트리밍 ✅
- **Kinesis Firehose**: 자동 배치 및 저장 ✅
- **Lambda 변환**: 서버리스 데이터 처리 ✅
- **완전 서버리스**: 인프라 관리 불필요 ✅
- **자동 확장**: 모든 구성요소 자동 스케일링 ✅

**D. API Gateway + AWS Glue + Lambda**
- **API Gateway**: 관리형 API ✅
- **AWS Glue**: 실시간 스트리밍 부적합 ❌
- **Lambda**: 서버리스 처리 ✅
- **아키텍처 혼재**: Glue와 Lambda 역할 중복 ❌
- **실시간 처리 한계**: Glue는 배치 처리 중심 ❌

### 📋 핵심 개념 정리

#### **실시간 데이터 처리 아키텍처**
```yaml
스트리밍 데이터 파이프라인:
Data Source → API → Stream → Transform → Store

최적 AWS 서비스 조합:
Producer → API Gateway → Kinesis → Lambda → S3
- 완전 서버리스
- 자동 확장
- 관리 부담 최소
```

#### **API 서비스 비교**
```yaml
Amazon API Gateway:
  ✅ 완전 관리형
  ✅ 자동 확장 (수천 RPS)
  ✅ 인증/권한 내장
  ✅ 모니터링 자동화
  ✅ 서버 관리 불필요

Amazon EC2 API:
  ❌ 서버 관리 필요
  ❌ 확장성 수동 관리
  ❌ 보안 설정 복잡
  ❌ 패치/업데이트 관리
  ❌ 고가용성 수동 구성
```

#### **스트리밍 처리 서비스**
```yaml
Amazon Kinesis Data Streams:
  ✅ 실시간 스트리밍
  ✅ 밀리초 지연시간
  ✅ 순서 보장
  ✅ 재처리 가능
  ✅ 사용자 정의 처리

Amazon Kinesis Data Firehose:
  ✅ 자동 배치 처리
  ✅ S3/Redshift 직접 전송
  ✅ 압축 및 암호화
  ✅ 내장 변환 기능
  ✅ 완전 관리형

AWS Glue:
  ❌ 배치 처리 중심
  ❌ 분 단위 지연
  ❌ 실시간 부적합
  ✅ ETL 작업 최적화
```

#### **정답 시나리오 (C번)**
```yaml
1. API Gateway 설정
   - REST API 생성
   - Kinesis 통합 설정
   - 인증/권한 구성
   - 스로틀링 설정

2. Kinesis Data Streams
   - 샤드 수 설정
   - 파티션 키 정의
   - 보존 기간 설정
   - 모니터링 활성화

3. Kinesis Data Firehose
   - 소스: Kinesis Data Streams
   - 변환: Lambda 함수 연결
   - 대상: S3 버킷
   - 배치 설정 최적화

4. Lambda 변환 함수
   - 실시간 데이터 처리
   - 형식 변환/정규화
   - 필터링/집계
   - 에러 핸들링

5. S3 저장소
   - 파티셔닝 전략
   - 압축 설정
   - 수명주기 정책
   - 분석 도구 연동
```

#### **운영 오버헤드 비교**
```yaml
C안 (완전 서버리스):
관리 대상:
  ✅ 비즈니스 로직만
  ✅ API 설정
  ✅ 데이터 변환 로직

자동 관리:
  ✅ 인프라 프로비저닝
  ✅ 확장/축소
  ✅ 패치/업데이트
  ✅ 모니터링
  ✅ 고가용성

A안 (EC2 포함):
추가 관리 필요:
  ❌ EC2 인스턴스 관리
  ❌ 운영체제 패치
  ❌ 애플리케이션 배포
  ❌ 보안 설정
  ❌ 모니터링 설정
```

#### **실시간 데이터 흐름**
```yaml
데이터 수집:
Client → API Gateway → Kinesis Data Streams
- 밀리초 단위 수집
- 자동 파티셔닝
- 순서 보장

데이터 변환:
Kinesis → Lambda → 변환된 데이터
- 실시간 처리
- 병렬 실행
- 오류 처리

데이터 저장:
Kinesis Firehose → S3
- 자동 배치
- 압축/암호화
- 파티셔닝
```

#### **확장성 및 성능**
```yaml
API Gateway:
- 기본: 10,000 RPS
- 버스트: 5,000 RPS
- 글로벌 배포 가능

Kinesis Data Streams:
- 샤드당: 1,000 레코드/초
- 1MB/초 처리량
- 동적 샤드 조정

Lambda:
- 동시 실행: 1,000개 (기본)
- 확장: 최대 수천 개
- 100ms 단위 처리

Kinesis Firehose:
- 처리량: 무제한
- 배치 크기: 조정 가능
- 압축 비율: 80-90%
```

