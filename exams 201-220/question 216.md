## Question #216
A company has a serverless website with millions of objects in an Amazon S3 bucket. 
The company uses the S3 bucket as the origin for an Amazon CloudFront distribution. 
The company did not set encryption on the S3 bucket before the objects were loaded. 
A solutions architect needs to enable encryption for all existing objects and for all objects that are added to the S3 bucket in the future.

Which solution will meet these requirements with the LEAST amount of effort?

A. Create a new S3 bucket. Turn on the default encryption settings for the new S3 bucket. Download all existing objects to temporary local storage. Upload the objects to the new S3 bucket.

B. Turn on the default encryption settings for the S3 bucket. Use the S3 Inventory feature to create a .csv file that lists the unencrypted objects. Run an S3 Batch Operations job that uses the copy command to encrypt those objects.

C. Create a new encryption key by using AWS Key Management Service (AWS KMS). Change the settings on the S3 bucket to use server-side encryption with AWS KMS managed encryption keys (SSE-KMS). Turn on versioning for the S3 bucket.

D. Navigate to Amazon S3 in the AWS Management Console. Browse the S3 bucket’s objects. Sort by the encryption field. Select each unencrypted object. Use the Modify button to apply default encryption settings to every unencrypted object in the S3 bucket.

## Question #216 분석

### 요구사항
- **수백만 개 객체** S3 버킷
- **CloudFront 오리진** 사용 중
- **기존 객체 + 신규 객체** 모두 암호화
- **최소 노력**

### 선택지 분석

**A. 새 버킷 + 다운로드/업로드**
- **엄청난 작업량**: 수백만 객체 다운로드/업로드 ❌
- **네트워크 비용**: 대용량 데이터 전송 비용 ❌
- **시간 소모**: 매우 오랜 시간 필요 ❌

**B. 기본 암호화 설정 + S3 Inventory + Batch Operations** ⭐
- **신규 객체**: 기본 암호화로 자동 처리 ✅
- **기존 객체**: Inventory로 목록 생성 후 Batch Operations로 자동 암호화 ✅
- **최소 노력**: 설정 후 완전 자동화 ✅
- **효율성**: AWS 내부 처리로 빠르고 저렴 ✅

**C. KMS 키 + SSE-KMS + 버전 관리**
- **신규 객체만**: 기존 객체는 여전히 암호화되지 않음 ❌
- **문제 미해결**: 기존 객체 암호화 방법 없음 ❌

**D. 콘솔에서 수동 선택 및 수정**
- **수백만 개 수동 작업**: 실현 불가능한 작업량 ❌
- **콘솔 제한**: 대량 객체 처리에 부적합 ❌

### B안 상세 과정

```yaml
1. 기본 암호화 활성화
   - 신규 객체 자동 암호화 보장

2. S3 Inventory 설정
   - 암호화되지 않은 객체 목록 생성
   - CSV 파일로 출력

3. S3 Batch Operations
   - Inventory CSV를 입력으로 사용
   - Copy 명령으로 기존 객체 재암호화
   - 완전 자동 처리
```

### S3 Batch Operations 장점

```yaml
대량 처리 최적화:
- 수억 개 객체 처리 가능
- AWS 내부 네트워크 사용
- 병렬 처리로 빠른 완료
- 실패 재시도 자동 처리

비용 효율성:
- 데이터 전송 없음
- 서버 비용 없음
- 요청 비용만 발생
```
