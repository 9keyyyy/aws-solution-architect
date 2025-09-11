## Question #1009
A company runs an environment where data is stored in an Amazon S3 bucket. The objects are accessed frequently throughout the day. The company has strict data encryption requirements for data that is stored in the S3 bucket. The company currently uses AWS Key Management Service (AWS KMS) for encryption.

The company wants to optimize costs associated with encrypting S3 objects without making additional calls to AWS KMS.

Which solution will meet these requirements?

A. Use server-side encryption with Amazon S3 managed keys (SSE-S3).

B. Use an S3 Bucket Key for server-side encryption with AWS KMS keys (SSE-KMS) on the new objects.

C. Use client-side encryption with AWS KMS customer managed keys.

D. Use server-side encryption with customer-provided keys (SSE-C) stored in AWS KMS.

## Question #1009 분석

### 문제 상황
- S3 객체에 **빈번한 접근**
- **엄격한 암호화 요구사항**
- 현재 **AWS KMS** 사용 중
- **KMS 추가 호출 없이** 비용 최적화

### 핵심 요구사항
- **암호화 유지**
- **KMS 호출 최소화**
- **비용 최적화**

### KMS 비용 구조
- **API 호출당 과금** ($0.03/10,000 요청)
- 빈번한 접근 → 높은 KMS 비용

### 선택지 분석

**A. SSE-S3**
- KMS 사용 안함 → 비용 절약 ✅
- 하지만 "엄격한 암호화 요구사항" 충족 안할 수도 ❌

**B. S3 Bucket Key + SSE-KMS** ⭐
- KMS 사용 유지 ✅
- Bucket Key로 KMS 호출 대폭 감소 ✅
- 동일한 보안 수준 ✅

**C. Client-side 암호화**
- 애플리케이션 변경 필요 ❌
- KMS 호출 여전히 발생 ❌

**D. SSE-C**
- 고객이 키 관리 → 복잡성 증가 ❌
- KMS 저장은 의미 없음 ❌

### S3 Bucket Key 원리

```yaml
기존 SSE-KMS:
각 객체마다 KMS 호출 → 높은 비용

S3 Bucket Key:
버킷 레벨에서 한 번만 KMS 호출 → 비용 99% 절약
동일한 암호화 강도 유지
```

