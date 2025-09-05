## Question #59
A company hosts more than 300 global websites and applications. 
The company requires a platform to analyze more than 30 TB of clickstream data each day.

What should a solutions architect do to transmit and process the clickstream data?

A. Design an AWS Data Pipeline to archive the data to an Amazon S3 bucket and run an Amazon EMR cluster with the data to generate analytics.

B. Create an Auto Scaling group of Amazon EC2 instances to process the data and send it to an Amazon S3 data lake for Amazon Redshift to use for analysis.

C. Cache the data to Amazon CloudFront. Store the data in an Amazon S3 bucket. When an object is added to the S3 bucket. run an AWS Lambda function to process the data for analysis.

D. Collect the data from Amazon Kinesis Data Streams. Use Amazon Kinesis Data Firehose to transmit the data to an Amazon S3 data lake. Load the data in Amazon Redshift for analysis.


## Question #59 분석

### ✅ 요구사항
- **300개 이상의 글로벌 웹사이트/애플리케이션**
- **일일 30TB 이상의 클릭스트림 데이터** 처리
- **실시간 데이터 전송 및 처리** 플랫폼 필요
- **대규모 데이터 분석** 요구

### ✅ 선택지 분석

**A. AWS Data Pipeline + S3 + EMR**
- **Data Pipeline**: 배치 처리용, 실시간 스트리밍 부적합 ❌
- **아카이브 방식**: 실시간 데이터 수집에 제한적 ❌
- **EMR**: 분석 엔진으로는 적합하나 실시간 처리 한계 ⚠️
- **지연 문제**: 대용량 실시간 스트림 처리에 부적합 ❌

**B. Auto Scaling EC2 + S3 + Redshift**
- **EC2 Auto Scaling**: 수동 관리 및 복잡성 증가 ❌
- **처리 후 전송**: 비효율적인 아키텍처 ❌
- **관리 오버헤드**: 인스턴스 관리 부담 ❌
- **확장성 제한**: 대규모 스트리밍에 부적합 ❌

**C. CloudFront + S3 + Lambda**
- **CloudFront 캐싱**: 클릭스트림 데이터 수집 용도 부적합 ❌
- **Lambda 제한**: 15분 실행 시간, 대용량 처리 한계 ❌
- **S3 이벤트 기반**: 실시간 스트리밍이 아닌 배치 처리 ❌
- **확장성 부족**: 30TB 일일 처리에 비현실적 ❌

**D. Kinesis Data Streams + Firehose + S3 + Redshift** ⭐
- **Kinesis Data Streams**: 실시간 대용량 스트리밍 수집 ✅
- **Kinesis Data Firehose**: S3로 자동 배치 전송 ✅
- **S3 Data Lake**: 대용량 데이터 저장소 ✅
- **Redshift**: 페타바이트급 분석 엔진 ✅
- **완전 관리형**: 서버리스 스트리밍 파이프라인 ✅

### 📋 핵심 개념 정리

#### **대용량 스트리밍 데이터 아키텍처**
```yaml
데이터 수집층:
  Amazon Kinesis Data Streams
  ✅ 실시간 스트리밍 수집
  ✅ 초당 수백만 레코드 처리
  ✅ 다중 프로듀서/컨슈머 지원

데이터 전송층:
  Amazon Kinesis Data Firehose
  ✅ 자동 배치 처리 (1MB 또는 60초)
  ✅ S3 압축 및 형식 변환
  ✅ 오류 처리 및 재시도

저장층:
  Amazon S3 Data Lake
  ✅ 무제한 확장성
  ✅ 비용 효율적
  ✅ 다양한 분석 도구 연동

분석층:
  Amazon Redshift
  ✅ 페타바이트급 분석
  ✅ 컬럼형 저장
  ✅ MPP(Massively Parallel Processing)
```

#### **클릭스트림 데이터 특성**
```yaml
대용량:
  ✅ 30TB/일 = 약 347MB/초
  ✅ 수백만 이벤트/초
  
실시간성:
  ✅ 즉시 수집 필요
  ✅ 낮은 지연 시간
  
확장성:
  ✅ 300개 사이트 동시 처리
  ✅ 글로벌 트래픽 패턴
```

#### **정답 시나리오 (D번)**
```yaml
1. 웹사이트/앱에서 클릭스트림 생성
   ↓
2. Kinesis Data Streams로 실시간 수집
   - 다중 샤드로 병렬 처리
   - 초당 수백만 레코드 수집
   ↓
3. Kinesis Data Firehose로 배치 전송
   - 1MB 또는 60초마다 S3 전송
   - 자동 압축 (gzip, snappy)
   - Parquet 형식 변환 (선택사항)
   ↓
4. S3 Data Lake 저장
   - 파티셔닝 (년/월/일/시간)
   - Lifecycle 정책으로 비용 최적화
   ↓
5. Redshift로 분석
   - COPY 명령으로 데이터 로드
   - 복잡한 분석 쿼리 실행
   - BI 도구 연동

장점:
  ✅ 완전 관리형 서비스
  ✅ 자동 확장
  ✅ 실시간 + 배치 처리
  ✅ 비용 효율적
  ✅ 내구성 및 가용성
```
