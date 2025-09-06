## Question #63
A company runs its infrastructure on AWS and has a registered base of 700,000 users for its document management application. 
The company intends to create a product that converts large .pdf files to .jpg image files. 
The .pdf files average 5 MB in size. 
The company needs to store the original files and the converted files. 
A solutions architect must design a scalable solution to accommodate demand that will grow rapidly over time.

Which solution meets these requirements MOST cost-effectively?

A. Save the .pdf files to Amazon S3. Configure an S3 PUT event to invoke an AWS Lambda function to convert the files to .jpg format and store them back in Amazon S3.

B. Save the .pdf files to Amazon DynamoDUse the DynamoDB Streams feature to invoke an AWS Lambda function to convert the files to .jpg format and store them back in DynamoDB.

C. Upload the .pdf files to an AWS Elastic Beanstalk application that includes Amazon EC2 instances, Amazon Elastic Block Store (Amazon EBS) storage, and an Auto Scaling group. Use a program in the EC2 instances to convert the files to .jpg format. Save the .pdf files and the .jpg files in the EBS store.

D. Upload the .pdf files to an AWS Elastic Beanstalk application that includes Amazon EC2 instances, Amazon Elastic File System (Amazon EFS) storage, and an Auto Scaling group. Use a program in the EC2 instances to convert the file to .jpg format. Save the .pdf files and the .jpg files in the EBS store.

## Question #63 분석

### ✅ 요구사항
- **70만 사용자** 기반 문서 관리 애플리케이션
- **PDF → JPG 변환** 제품 개발
- **평균 5MB PDF 파일** 처리
- **원본 파일과 변환 파일 모두 저장**
- **확장 가능한 솔루션** (급격한 수요 증가 대비)
- **최대 비용 효율성** 요구

### ✅ 선택지 분석

**A. S3 + S3 PUT 이벤트 + Lambda 변환** ⭐
- **S3 스토리지**: 대용량 파일 저장에 최적 ✅
- **이벤트 기반**: PUT 이벤트로 자동 트리거 ✅
- **Lambda 처리**: 서버리스로 운영 비용 최소화 ✅
- **자동 확장**: 동시 실행으로 수요 급증 대응 ✅
- **비용 효율**: 사용량 기반 과금 ✅

**B. DynamoDB + DynamoDB Streams + Lambda**
- **DynamoDB**: 문서 DB로 5MB 파일 저장 부적합 ❌
- **400KB 제한**: DynamoDB 아이템 크기 제한 초과 ❌
- **스토리지 비용**: 문서 저장에 매우 비효율적 ❌
- **기술적 불가능**: 대용량 파일 저장 불가 ❌

**C. Elastic Beanstalk + EC2 + EBS + Auto Scaling**
- **상시 인스턴스**: 24/7 EC2 비용 발생 ❌
- **EBS 스토리지**: 인스턴스별 로컬 스토리지 한계 ❌
- **확장성 제약**: EBS는 단일 인스턴스 연결 ❌
- **높은 운영 비용**: 지속적인 인프라 비용 ❌

**D. Elastic Beanstalk + EC2 + EFS + Auto Scaling**
- **EFS 공유**: 여러 인스턴스 파일 공유 가능 ✅
- **상시 비용**: EC2 인스턴스 지속 실행 비용 ❌
- **처리 지연**: 큐잉 시스템 없어 비효율적 ❌
- **과도한 인프라**: 서버리스 대비 복잡하고 비싼 구조 ❌

### 📋 핵심 개념 정리

#### **서버리스 vs 서버 기반 아키텍처**
```yaml
서버리스 (A번):
  ✅ 사용량 기반 과금
  ✅ 자동 확장 (0→수천 동시실행)
  ✅ 운영 관리 불필요
  ✅ 이벤트 기반 트리거
  
서버 기반 (C, D번):
  ❌ 상시 인스턴스 비용
  ❌ 용량 계획 필요
  ❌ 운영 관리 부담
  ⚠️ 오토스케일링 지연
```

#### **스토리지 옵션 비교**
```yaml
Amazon S3:
  ✅ 무제한 확장성
  ✅ 11 9s 내구성
  ✅ 다양한 스토리지 클래스
  ✅ 이벤트 알림 지원
  💰 저렴한 스토리지 비용

DynamoDB:
  ❌ 400KB 아이템 크기 제한
  ❌ 문서 저장 부적합
  💰 높은 스토리지 비용

EBS/EFS:
  ⚠️ 인스턴스 종속성
  ⚠️ 지역 제한
  💰 지속적인 프로비저닝 비용
```

#### **정답 시나리오 (A번)**
```yaml
1. 사용자가 PDF 파일 업로드
   ↓
2. S3 버킷에 PDF 파일 저장
   - 원본 파일 경로: /originals/filename.pdf
   ↓
3. S3 PUT 이벤트 트리거
   - 자동으로 Lambda 함수 호출
   ↓
4. Lambda 함수에서 PDF→JPG 변환
   - PDF 처리 라이브러리 사용
   - 메모리 효율적 처리
   ↓
5. 변환된 JPG 파일 S3에 저장
   - 변환 파일 경로: /converted/filename.jpg
   
비용 효율성:
  ✅ Lambda: 실행 시간만큼만 과금
  ✅ S3: 실제 사용 스토리지만 과금
  ✅ 운영 관리: Zero operational overhead
  
확장성:
  ✅ Lambda: 수천 동시 실행 자동 확장
  ✅ S3: 무제한 스토리지 확장
  ✅ 수요 급증: 즉시 대응 가능

예상 비용 (월 1만 변환 기준):
  S3 스토리지: ~$23 (10GB 저장)
  Lambda 실행: ~$0.20 (100ms × 1만회)
  S3 요청: ~$0.40 (PUT/GET 요청)
  총 비용: ~$24/월
```

#### **Lambda 제한사항 고려**
```yaml
Lambda 제한:
  ✅ 15분 최대 실행 시간
  ✅ 512MB-10GB 메모리
  ✅ 5MB PDF 처리에 충분
  
대안 (대용량 파일):
  ⚠️ Step Functions + ECS Fargate
  ⚠️ SQS + EC2 Auto Scaling
```
