## Question #220
A solutions architect is designing a new API using Amazon API Gateway that will receive requests from users. 
The volume of requests is highly variable; several hours can pass without receiving a single request. 
The data processing will take place asynchronously, but should be completed within a few seconds after a request is made.

Which compute service should the solutions architect have the API invoke to deliver the requirements at the lowest cost?

A. An AWS Glue job

B. An AWS Lambda function

C. A containerized service hosted in Amazon Elastic Kubernetes Service (Amazon EKS)

D. A containerized service hosted in Amazon ECS with Amazon EC2

## Question #220 분석

### 요구사항
- **API Gateway** 새로운 API 설계
- **가변적 요청량**: 수 시간 동안 요청 없을 수도 있음
- **비동기 처리**: 요청 후 수 초 내 완료
- **최저 비용**

### 선택지 분석

**A. AWS Glue Job**
- **용도 불일치**: ETL 배치 처리용, 실시간 API 처리 부적합 ❌
- **시작 시간**: 수 분 소요, 수 초 요구사항 미충족 ❌
- **비용**: 최소 실행 시간 과금으로 비효율 ❌

**B. AWS Lambda 함수** ⭐
- **서버리스**: 요청 없을 때 비용 없음 ✅
- **즉시 실행**: 콜드 스타트도 수 초 내 ✅
- **사용량 기반**: 실행 시간만큼만 과금 ✅
- **API Gateway 네이티브 통합**: 직접 연동 지원 ✅

**C. Amazon EKS 컨테이너**
- **상시 운영 비용**: 클러스터 관리 비용 지속 발생 ❌
- **복잡성**: Kubernetes 관리 오버헤드 ❌
- **가변 트래픽 비효율**: 저사용량 시에도 리소스 비용 ❌

**D. Amazon ECS + EC2**
- **EC2 인스턴스 비용**: 사용하지 않아도 지속 과금 ❌
- **관리 부담**: 컨테이너 및 인프라 관리 필요 ❌
- **오버프로비저닝**: 피크 대비 용량 유지 필요 ❌

### Lambda 최적성

```yaml
가변 트래픽 대응:
- 요청 없음: $0 비용
- 요청 급증: 자동 확장
- 수 초 처리: 최적 실행 모델

비용 구조:
- 요청당: $0.0000002
- 실행 시간: $0.0000166667/GB-초
- 유휴 시간: $0

API Gateway 통합:
- 직접 트리거 지원
- 설정 간소화
- 비동기 호출 지원
```
