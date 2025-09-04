## Question #17
A company is implementing a new business application. 
The application runs on two Amazon EC2 instances and uses an Amazon S3 bucket for document storage. 
A solutions architect needs to ensure that the EC2 instances can access the S3 bucket.

What should the solutions architect do to meet this requirement?

A. Create an IAM role that grants access to the S3 bucket. Attach the role to the EC2 instances.

B. Create an IAM policy that grants access to the S3 bucket. Attach the policy to the EC2 instances.

C. Create an IAM group that grants access to the S3 bucket. Attach the group to the EC2 instances.

D. Create an IAM user that grants access to the S3 bucket. Attach the user account to the EC2 instances.

## Question #17 분석

### ✅ 요구사항
- EC2 인스턴스에서 S3 버킷 접근 필요
- 안전하고 적절한 AWS 인증/권한 부여 방법
- 애플리케이션용 권한 관리

### ✅ 선택지 분석

**A. IAM 역할 생성 + EC2 인스턴스 연결** ⭐
- **EC2 인스턴스 전용 권한 부여 방식**
- 임시 자격 증명 자동 교체
- 하드코딩된 키 불필요
- AWS 모범 사례

**B. IAM 정책을 EC2에 직접 연결**
- **정책은 직접 연결 불가** ❌
- 정책은 역할/사용자/그룹에 연결
- 구문적으로 불가능

**C. IAM 그룹을 EC2에 연결**
- **그룹은 사용자 관리 목적** ❌
- EC2 인스턴스는 그룹 멤버가 될 수 없음
- 서비스용이 아닌 사용자용 개념

**D. IAM 사용자를 EC2에 연결**
- **보안상 부적절** ❌
- 정적 Access Key 필요
- 키 관리 복잡성
- 키 노출 위험

### 📋 AWS 권한 부여 개념 정리

### **IAM 역할 (Roles)**
```yaml
용도: AWS 서비스 간 권한 부여
특징:
  - 임시 자격 증명 제공
  - 자동 키 교체 (15분~12시간)
  - 서비스 대 서비스 인증
  - 하드코딩 불필요

EC2 사용 시:
  - Instance Profile 통해 연결
  - STS AssumeRole 자동 처리
  - 애플리케이션에서 SDK 자동 인식
```

### **IAM 정책 (Policies)**
```yaml
용도: 권한 정의 문서
특징:
  - JSON 형태의 권한 명세
  - 단독으로는 효력 없음
  - 반드시 역할/사용자/그룹에 연결

연결 대상:
  - IAM 역할 ✅
  - IAM 사용자 ✅
  - IAM 그룹 ✅
  - EC2 인스턴스 ❌ (직접 불가)
```

### **IAM 그룹 (Groups)**
```yaml
용도: 사용자 그룹화 및 권한 관리
특징:
  - 여러 사용자에게 동일 권한 부여
  - 정책 연결 가능
  - 사용자 관리 편의성

제한사항:
  - 사용자만 멤버 가능
  - EC2 인스턴스 멤버십 불가 ❌
  - 서비스 계정 개념 아님
```

### **IAM 사용자 (Users)**
```yaml
용도: 개별 사용자/애플리케이션 계정
특징:
  - 영구적 자격 증명 (Access Key)
  - 콘솔/API 접근 가능
  - 정적 키 관리 필요

EC2 사용 시 문제점:
  - Access Key 하드코딩 필요 ❌
  - 키 교체 수동 관리 ❌
  - 키 노출 시 보안 위험 ❌
```

### 🔄 EC2 + S3 권한 부여 모범 사례

### **IAM 역할 기반 접근 (권장)**
```yaml
1단계: IAM 역할 생성
   역할 이름: EC2-S3-Access-Role
   신뢰 정책: EC2 서비스 허용
   
2단계: 권한 정책 연결
   정책: S3FullAccess 또는 커스텀 정책
   리소스: 특정 S3 버킷만 지정
   
3단계: EC2 인스턴스 연결
   Instance Profile 생성
   EC2 Launch 시 역할 지정
   
4단계: 애플리케이션 코드
   AWS SDK가 자동으로 임시 자격증명 사용
   하드코딩된 키 불필요
```

### **권한 정책 예시**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject",
        "s3:DeleteObject"
      ],
      "Resource": "arn:aws:s3:::company-documents/*"
    },
    {
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::company-documents"
    }
  ]
}
```

### **보안 비교**
```yaml
IAM 역할 방식:
  ✅ 임시 자격 증명 (자동 교체)
  ✅ 키 하드코딩 불필요
  ✅ 최소 권한 원칙 적용 용이
  ✅ AWS 모범 사례

IAM 사용자 방식:
  ❌ 정적 Access Key (수동 교체)
  ❌ 키 하드코딩 또는 환경변수 필요
  ❌ 키 노출 위험
  ❌ 레거시 방식
```
