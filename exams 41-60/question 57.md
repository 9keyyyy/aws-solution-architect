## Question #57
A company is running a popular social media website. 
The website gives users the ability to upload images to share with other users. 
The company wants to make sure that the images do not contain inappropriate content. 
The company needs a solution that minimizes development effort.

What should a solutions architect do to meet these requirements?

A. Use Amazon Comprehend to detect inappropriate content. Use human review for low-confidence predictions.

B. Use Amazon Rekognition to detect inappropriate content. Use human review for low-confidence predictions.

C. Use Amazon SageMaker to detect inappropriate content. Use ground truth to label low-confidence predictions.

D. Use AWS Fargate to deploy a custom machine learning model to detect inappropriate content. Use ground truth to label low-confidence predictions.

## Question #57 분석

### ✅ 요구사항
- **소셜 미디어 웹사이트**에서 **이미지 업로드**
- **부적절한 콘텐츠 감지** 필요
- **개발 노력 최소화** (핵심 요구사항)

### ✅ 선택지 분석

**A. Amazon Comprehend + 사람 검토**
- **Comprehend 용도**: **텍스트 분석** 서비스 ❌
- **이미지 처리 불가**: 자연어 처리 전용 ❌
- **요구사항 불일치**: 이미지 콘텐츠 모더레이션 불가능 ❌

**B. Amazon Rekognition + 사람 검토** ⭐
- **Rekognition 용도**: **이미지/비디오 분석** 서비스 ✅
- **콘텐츠 모더레이션**: 부적절한 콘텐츠 자동 감지 ✅
- **관리형 서비스**: 개발 노력 최소화 ✅
- **신뢰도 점수**: 낮은 신뢰도 시 사람 검토 가능 ✅

**C. Amazon SageMaker + Ground Truth**
- **SageMaker**: 커스텀 ML 모델 개발 플랫폼 ⚠️
- **개발 노력 증가**: 모델 훈련, 배포, 관리 필요 ❌
- **Ground Truth**: 데이터 라벨링 서비스 (예측이 아님) ❌
- **복잡성**: 최소 개발 노력 요구사항과 상충 ❌

**D. AWS Fargate + 커스텀 ML 모델**
- **Fargate**: 컨테이너 실행 플랫폼 ⚠️
- **커스텀 모델**: 자체 개발 및 관리 필요 ❌
- **최대 개발 노력**: 모델 개발, 컨테이너화, 배포 ❌
- **운영 복잡성**: 인프라 관리 부담 ❌

### 📋 핵심 개념 정리

#### **AWS AI/ML 서비스 비교**
```yaml
Amazon Rekognition:
  ✅ 이미지/비디오 분석 전용
  ✅ 콘텐츠 모더레이션 기능 내장
  ✅ 완전관리형 (No 개발/배포)
  ✅ API 호출만으로 즉시 사용
  ✅ 신뢰도 점수 제공

Amazon Comprehend:
  ✅ 텍스트 분석 전용
  ❌ 이미지 처리 불가능
  ✅ 감정 분석, 엔티티 추출

Amazon SageMaker:
  ✅ 커스텀 ML 모델 플랫폼
  ⚠️ 개발/훈련/배포 필요
  ⚠️ ML 전문 지식 요구

AWS Fargate:
  ✅ 서버리스 컨테이너 실행
  ⚠️ 애플리케이션 개발 필요
```

#### **콘텐츠 모더레이션 워크플로우**
```yaml
Rekognition 콘텐츠 모더레이션:
  1. 이미지 업로드
  2. Rekognition API 호출
  3. 부적절한 콘텐츠 감지
  4. 신뢰도 점수 확인
  5. 높은 신뢰도: 자동 차단
  6. 낮은 신뢰도: 사람 검토 대기열
```

#### **정답 시나리오 (B번)**
```yaml
1. 사용자 이미지 업로드
   ↓
2. Amazon Rekognition DetectModerationLabels API
   - 폭력성, 성인 콘텐츠, 혐오 심볼 등 감지
   - 신뢰도 점수 (0-100) 반환
   ↓
3. 자동 판단
   - 높은 신뢰도 (예: >90%): 자동 차단
   - 낮은 신뢰도 (예: 50-90%): 사람 검토
   ↓
4. 사람 검토 (필요시)
   - 애매한 케이스만 수동 검토
   - 최종 승인/거부 결정

장점:
  ✅ 즉시 배포 가능 (No 개발)
  ✅ AWS 관리형 서비스
  ✅ 높은 정확도
  ✅ 비용 효율적 (사용량 기반)
```
