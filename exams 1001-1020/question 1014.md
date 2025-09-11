## Question #1014
A digital image processing company wants to migrate its on-premises monolithic application to the AWS Cloud. The company processes thousands of images and generates large files as part of the processing workflow.

The company needs a solution to manage the growing number of image processing jobs. The solution must also reduce the manual tasks in the image processing workflow. The company does not want to manage the underlying infrastructure of the solution.

Which solution will meet these requirements with the LEAST operational overhead?

A. Use Amazon Elastic Container Service (Amazon ECS) with Amazon EC2 Spot Instances to process the images. Configure Amazon Simple Queue Service (Amazon SQS) to orchestrate the workflow. Store the processed files in Amazon Elastic File System (Amazon EFS).

B. Use AWS Batch jobs to process the images. Use AWS Step Functions to orchestrate the workflow. Store the processed files in an Amazon S3 bucket.

C. Use AWS Lambda functions and Amazon EC2 Spot Instances to process the images. Store the processed files in Amazon FSx.

D. Deploy a group of Amazon EC2 instances to process the images. Use AWS Step Functions to orchestrate the workflow. Store the processed files in an Amazon Elastic Block Store (Amazon EBS) volume.

## Question #1014 분석

### 문제 상황
- **대량 이미지 처리** (수천 개)
- **큰 파일 생성**
- **증가하는 작업량** 관리 필요
- **워크플로우 자동화** 필요
- **인프라 관리 원하지 않음**

### 핵심 요구사항
- **서버리스/관리형 서비스**
- **대량 작업 처리**
- **워크플로우 오케스트레이션**
- **최소 운영 오버헤드**

### 선택지 분석

**A. ECS + EC2 Spot + SQS + EFS**
- ECS: 컨테이너 관리 필요 ❌
- EC2 Spot: 인스턴스 관리 필요 ❌
- 인프라 관리 부담 ❌

**B. AWS Batch + Step Functions + S3** ⭐
- AWS Batch: 완전 관리형 배치 처리 ✅
- Step Functions: 워크플로우 오케스트레이션 ✅
- S3: 확장 가능한 저장소 ✅
- 인프라 관리 불필요 ✅

**C. Lambda + EC2 Spot + FSx**
- Lambda: 15분 제한 (대용량 이미지 처리에 부적합) ❌
- EC2 Spot: 인프라 관리 필요 ❌

**D. EC2 + Step Functions + EBS**
- EC2: 인프라 관리 필요 ❌
- EBS: 단일 인스턴스 연결 제한 ❌

### 서비스별 특징

**AWS Batch**
- 배치 작업 전용 관리형 서비스
- 자동 스케일링
- 컨테이너 기반 작업 실행

**Step Functions**
- 워크플로우 시각적 관리
- 서버리스 오케스트레이션
- 에러 처리 및 재시도

