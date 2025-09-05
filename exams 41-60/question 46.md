## Question #46
A company has an application that provides marketing services to stores. 
The services are based on previous purchases by store customers. 
The stores upload transaction data to the company through SFTP, and the data is processed and analyzed to generate new marketing offers. 
Some of the files can exceed 200 GB in size.
Recently, the company discovered that some of the stores have uploaded files that contain personally identifiable information (PII) that should not have been included. 
The company wants administrators to be alerted if PII is shared again. 
The company also wants to automate remediation.

What should a solutions architect do to meet these requirements with the LEAST development effort?

A. Use an Amazon S3 bucket as a secure transfer point. Use Amazon Inspector to scan the objects in the bucket. If objects contain PII, trigger an S3 Lifecycle policy to remove the objects that contain PII.

B. Use an Amazon S3 bucket as a secure transfer point. Use Amazon Macie to scan the objects in the bucket. If objects contain PII, use Amazon Simple Notification Service (Amazon SNS) to trigger a notification to the administrators to remove the objects that contain PII.

C. Implement custom scanning algorithms in an AWS Lambda function. Trigger the function when objects are loaded into the bucket. If objects contain PII, use Amazon Simple Notification Service (Amazon SNS) to trigger a notification to the administrators to remove the objects that contain PII.

D. Implement custom scanning algorithms in an AWS Lambda function. Trigger the function when objects are loaded into the bucket. If objects contain PII, use Amazon Simple Email Service (Amazon SES) to trigger a notification to the administrators and trigger an S3 Lifecycle policy to remove the meats that contain PII.

## Question #46 분석

### ✅ 요구사항
- **SFTP로 대용량 파일 업로드** (최대 200GB)
- **PII 포함 파일 탐지** 필요
- **관리자 알림** + **자동 수정** 필요
- **최소 개발 노력**

### ✅ 선택지 분석

**A. S3 + Inspector + Lifecycle**
- **Inspector 한계**: EC2/컨테이너 보안 스캔 전용 
- **PII 탐지 불가**: Inspector는 PII 스캔 기능 없음 
- **서비스 미스매치**: 용도가 완전히 다름

**B. S3 + Macie + SNS** ⭐
- **Macie**: PII/민감 데이터 탐지 전용 서비스 ✅
- **관리형 서비스**: 별도 개발 불필요 ✅
- **자동 알림**: SNS 통합 지원 ✅
- **최소 개발**: GUI 설정만으로 완료 ✅

**C. Lambda 커스텀 스캔 + SNS**
- **높은 개발 비용**: PII 탐지 알고리즘 직접 구현 
- **복잡한 로직**: 대용량 파일 처리 어려움 
- **자동 수정 없음**: 알림만 제공 

**D. Lambda 커스텀 스캔 + SES + Lifecycle**
- **최고 개발 비용**: 커스텀 스캔 + 복잡한 통합 
- **대용량 파일 한계**: Lambda 처리 제약 
- **불필요한 복잡성**: 관리형 서비스 대안 존재

### 📋 Amazon Macie 핵심 개념

#### **Macie = PII 탐지 전문 서비스**
```yaml
서비스 목적: 민감 데이터 자동 발견 및 보호
지원 데이터: S3 객체 내 PII, 금융 정보, 개인정보
탐지 방식: ML 기반 자동 분류 + 정규식 패턴
알림 연동: EventBridge, SNS 자동 통합
```

#### **PII 탐지 능력**
```yaml
탐지 가능한 정보:
- 신용카드 번호, SSN
- 이름, 주소, 전화번호  
- 이메일, 여권번호
- 사용자 정의 패턴

파일 형식 지원:
- 텍스트, CSV, JSON
- PDF, Word, Excel
- 압축 파일 내부까지
```

### 🔄 솔루션 비교

#### **Macie 방식 (B번) - 최소 개발**
```yaml
구현 과정:
1. S3 버킷을 SFTP 전송 지점으로 설정
2. Macie에서 해당 버킷 스캔 활성화
3. PII 발견 시 SNS 토픽으로 알림 설정
4. 완료 (코딩 없이 설정만)

운영:
- 자동 스캔: 파일 업로드 시
- 자동 알림: PII 발견 시 즉시
- 관리: AWS 콘솔에서 대시보드 확인
```

#### **커스텀 Lambda 방식 (C, D번) - 높은 개발**
```yaml
구현 과정:
1. PII 탐지 알고리즘 개발 (수주-수개월)
2. 대용량 파일 처리 로직 구현
3. 에러 처리 및 재시도 메커니즘
4. 알림 시스템 통합
5. 테스트 및 검증

문제점:
- 200GB 파일: Lambda 15분/10GB 제한
- 복잡한 PII 패턴: 정확도 낮음
- 유지보수: 지속적 개발 필요
```


### 🔍 Inspector vs Macie 차이

#### **Amazon Inspector**
```yaml
목적: 애플리케이션 보안 취약점 스캔
대상: EC2 인스턴스, 컨테이너 이미지
기능: CVE, 보안 결함, 네트워크 도달성
PII 탐지: 지원 안함 ❌
```

#### **Amazon Macie**
```yaml
목적: 데이터 보안 및 프라이버시 보호
대상: S3 객체 내 데이터 콘텐츠
기능: PII, 민감 데이터 자동 분류
PII 탐지: 전문 기능 ✅
```
