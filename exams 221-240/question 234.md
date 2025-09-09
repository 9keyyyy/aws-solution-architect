## Question #234
A company is building a new web-based customer relationship management application. 
The application will use several Amazon EC2 instances that are backed by Amazon Elastic Block Store (Amazon EBS) volumes behind an Application Load Balancer (ALB). 
The application will also use an Amazon Aurora database. 
All data for the application must be encrypted at rest and in transit.

Which solution will meet these requirements?

A. Use AWS Key Management Service (AWS KMS) certificates on the ALB to encrypt data in transit. Use AWS Certificate Manager (ACM) to encrypt the EBS volumes and Aurora database storage at rest.

B. Use the AWS root account to log in to the AWS Management Console. Upload the company’s encryption certificates. While in the root account, select the option to turn on encryption for all data at rest and in transit for the account.

C. Use AWS Key Management Service (AWS KMS) to encrypt the EBS volumes and Aurora database storage at rest. Attach an AWS Certificate Manager (ACM) certificate to the ALB to encrypt data in transit.

D. Use BitLocker to encrypt all data at rest. Import the company’s TLS certificate keys to AWS Key Management Service (AWS KMS) Attach the KMS keys to the ALB to encrypt data in transit.

## Question #234 분석

### 요구사항
- **웹 기반 CRM 애플리케이션**
- **EC2 + EBS + ALB + Aurora** 아키텍처
- **저장 시 및 전송 중 암호화** 필수

### 선택지 분석

**A. KMS 인증서로 전송 암호화 + ACM으로 저장 암호화**
- **서비스 역할 혼동**: KMS는 암호화 키 관리, 인증서 서비스 아님 ❌
- **기능 오해**: ACM은 SSL/TLS 인증서용, 스토리지 암호화용 아님 ❌

**B. Root 계정으로 전체 암호화 옵션 활성화**
- **존재하지 않는 기능**: AWS에는 계정 전체 암호화 토글 없음 ❌
- **보안 위험**: Root 계정 일상적 사용 부적절 ❌
- **기술적 불가능**: 설명된 기능 자체가 존재하지 않음 ❌

**C. KMS로 저장 암호화 + ACM 인증서로 전송 암호화** ⭐
- **KMS 저장 암호화**: EBS 및 Aurora 암호화에 적합 ✅
- **ACM 전송 암호화**: ALB에 SSL/TLS 인증서 연결 ✅
- **올바른 서비스 사용**: 각 서비스의 목적에 맞는 사용 ✅
- **완전한 암호화**: 저장 시와 전송 중 모두 암호화 ✅

**D. BitLocker + KMS TLS 키**
- **플랫폼 부적합**: BitLocker는 Windows 전용, Linux EC2에 부적절 ❌
- **서비스 오해**: KMS는 TLS 인증서 관리 서비스 아님 ❌
- **복잡성**: 불필요한 외부 도구 사용 ❌

### 올바른 암호화 구성

```yaml
저장 시 암호화 (KMS):
- EBS 볼륨: KMS 키로 암호화
- Aurora 데이터베이스: KMS 키로 암호화
- 자동 백업도 암호화됨

전송 중 암호화 (ACM):
- ALB에 ACM SSL/TLS 인증서 연결
- HTTPS 트래픽 강제
- 클라이언트-ALB 간 암호화 보장
```
