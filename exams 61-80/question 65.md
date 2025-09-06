## Question #65
A hospital recently deployed a RESTful API with Amazon API Gateway and AWS Lambda. 
The hospital uses API Gateway and Lambda to upload reports that are in PDF format and JPEG format. 
The hospital needs to modify the Lambda code to identify protected health information (PHI) in the reports.

Which solution will meet these requirements with the LEAST operational overhead?

A. Use existing Python libraries to extract the text from the reports and to identify the PHI from the extracted text.

B. Use Amazon Textract to extract the text from the reports. Use Amazon SageMaker to identify the PHI from the extracted text.

C. Use Amazon Textract to extract the text from the reports. Use Amazon Comprehend Medical to identify the PHI from the extracted text.

D. Use Amazon Rekognition to extract the text from the reports. Use Amazon Comprehend Medical to identify the PHI from the extracted text.

## Question #65 분석

### ✅ 요구사항
- **병원에서 RESTful API** (API Gateway + Lambda)
- **PDF/JPEG 형식 리포트** 업로드
- **보호 대상 건강 정보(PHI)** 식별 필요
- **최소 운영 오버헤드** 요구

### ✅ 선택지 분석

**A. Python 라이브러리로 텍스트 추출 + PHI 식별**
- **수동 구현**: OCR 및 NLP 로직 직접 개발 ❌
- **운영 부담**: 라이브러리 관리, 정확도 조정 ❌
- **복잡성**: PDF/이미지 처리 + 의료 용어 학습 ❌
- **유지보수**: 지속적인 모델 업데이트 필요 ❌

**B. Amazon Textract + Amazon SageMaker**
- **Textract**: 문서 텍스트 추출 전용 서비스 ✅
- **SageMaker**: 커스텀 ML 모델 개발 플랫폼 ⚠️
- **운영 오버헤드**: 모델 훈련, 배포, 관리 필요 ❌
- **복잡성**: ML 전문 지식 및 개발 시간 소요 ❌

**C. Amazon Textract + Amazon Comprehend Medical** ⭐
- **Textract**: PDF/JPEG에서 텍스트 정확 추출 ✅
- **Comprehend Medical**: PHI 식별 전용 서비스 ✅
- **완전 관리형**: 개발/관리 부담 최소 ✅
- **의료 특화**: 의료 용어 및 PHI 사전 훈련됨 ✅
- **API 기반**: Lambda에서 간단 호출 ✅

**D. Amazon Rekognition + Amazon Comprehend Medical**
- **Rekognition**: 이미지 분석 서비스 (텍스트 추출 기능 있음) ⚠️
- **텍스트 추출 한계**: 복잡한 문서 레이아웃 처리 제한 ❌
- **PDF 지원**: Rekognition은 PDF 직접 처리 불가 ❌
- **Comprehend Medical**: PHI 식별에는 적합 ✅

### 📋 핵심 개념 정리

#### **텍스트 추출 서비스 비교**
```yaml
Amazon Textract:
  ✅ 문서 전용 OCR 서비스
  ✅ PDF, JPEG, PNG 지원
  ✅ 테이블, 폼, 레이아웃 이해
  ✅ 의료 문서 최적화
  ✅ 높은 정확도

Amazon Rekognition:
  ✅ 이미지/비디오 분석 서비스
  ✅ 텍스트 감지 기능 포함
  ❌ 복잡한 문서 레이아웃 제한
  ❌ PDF 직접 지원 안 함
  ⚠️ 단순 텍스트 추출용
```

#### **PHI 식별 서비스 비교**
```yaml
Amazon Comprehend Medical:
  ✅ 의료 텍스트 분석 전용
  ✅ PHI 자동 식별 및 분류
  ✅ HIPAA 준수 설계
  ✅ 의료 엔티티 추출
  ✅ 관리형 서비스

Amazon SageMaker:
  ✅ 커스텀 ML 모델 플랫폼
  ⚠️ 모델 개발/훈련 필요
  ⚠️ 높은 운영 복잡성
  ⚠️ ML 전문 지식 요구
```

#### **정답 시나리오 (C번)**
```yaml
1. API Gateway → Lambda 함수 트리거
   
2. Textract로 문서 텍스트 추출
   - PDF/JPEG 파일 처리
   - OCR을 통한 정확한 텍스트 추출
   - 테이블/폼 구조 인식
   
3. Comprehend Medical로 PHI 식별
   - 추출된 텍스트 분석
   - 환자명, 생년월일, 주소 등 식별
   - 의료 기록 번호, 보험 번호 탐지
   
4. 결과 반환
   - PHI 위치 및 유형 정보
   - 마스킹 또는 제거 처리
   - 준수 보고서 생성
```

