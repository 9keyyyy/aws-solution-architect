## Question #100
A company's containerized application runs on an Amazon EC2 instance. 
The application needs to download security certificates before it can communicate with other business applications. 
The company wants a highly secure solution to encrypt and decrypt the certificates in near real time. 
The solution also needs to store data in highly available storage after the data is encrypted.

Which solution will meet these requirements with the LEAST operational overhead?

A. Create AWS Secrets Manager secrets for encrypted certificates. Manually update the certificates as needed. Control access to the data by using fine-grained IAM access.

B. Create an AWS Lambda function that uses the Python cryptography library to receive and perform encryption operations. Store the function in an Amazon S3 bucket.

C. Create an AWS Key Management Service (AWS KMS) customer managed key. Allow the EC2 role to use the KMS key for encryption operations. Store the encrypted data on Amazon S3.

D. Create an AWS Key Management Service (AWS KMS) customer managed key. Allow the EC2 role to use the KMS key for encryption operations. Store the encrypted data on Amazon Elastic Block Store (Amazon EBS) volumes.

## Question #100 분석

### 요구사항
- **컨테이너 애플리케이션**이 보안 인증서 다운로드
- **실시간에 가까운** 암호화/복호화
- **고가용성 스토리지**에 암호화된 데이터 저장
- **최소 운영 오버헤드**

### 선택지 분석

**A. AWS Secrets Manager**
- **수동 업데이트**: 인증서 갱신 시 수동 작업 필요 ❌
- **운영 부담**: 정기적 수동 관리 ❌
- **용도 불일치**: 동적 암호화보다는 자격증명 저장용 ❌

**B. Lambda + Python cryptography**
- **복잡성**: 커스텀 암호화 로직 개발 필요 ❌
- **S3 저장 오해**: Lambda 함수를 S3에 저장 불가 ❌
- **운영 부담**: 암호화 라이브러리 관리 필요 ❌

**C. KMS + S3** ⭐
- **KMS 암호화**: 하드웨어 기반 실시간 암호화 ✅
- **S3 고가용성**: 99% 내구성 ✅
- **완전 관리형**: AWS 관리 서비스만 사용 ✅
- **최소 오버헤드**: 설정 후 자동 운영 ✅

**D. KMS + EBS**
- **KMS 암호화**: 실시간 암호화 지원 ✅
- **EBS 제한**: 단일 AZ, 인스턴스 종속 ❌
- **가용성 부족**: S3 대비 낮은 가용성 ❌

### 고가용성 비교

```yaml
S3:
✅ 멀티 AZ 자동 복제
✅ 99.999999999% 내구성
✅ 인스턴스 독립적 접근
✅ 글로벌 액세스

EBS:
❌ 단일 AZ 제한
❌ 인스턴스 종속성
❌ 가용성 상대적 제한
```

### KMS 통합

```yaml
KMS + S3:
- 자동 서버측 암호화
- 투명한 암복호화
- IAM 기반 접근 제어
- 실시간 처리 성능

구현:
aws s3api put-object \
    --bucket certificates \
    --key cert.pem \
    --body cert.pem \
    --server-side-encryption aws:kms \
    --ssekms-key-id arn:aws:kms:region:account:key/key-id
```
