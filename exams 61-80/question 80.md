## Question #80
A company recently signed a contract with an AWS Managed Service Provider (MSP) Partner for help with an application migration initiative. 
A solutions architect needs ta share an Amazon Machine Image (AMI) from an existing AWS account with the MSP Partner's AWS account. 
The AMI is backed by Amazon Elastic Block Store (Amazon EBS) and uses an AWS Key Management Service (AWS KMS) customer managed key to encrypt EBS volume snapshots.

What is the MOST secure way for the solutions architect to share the AMI with the MSP Partner's AWS account?

A. Make the encrypted AMI and snapshots publicly available. Modify the key policy to allow the MSP Partner's AWS account to use the key.

B. Modify the launchPermission property of the AMI. Share the AMI with the MSP Partner's AWS account only. Modify the key policy to allow the MSP Partner's AWS account to use the key.

C. Modify the launchPermission property of the AMI. Share the AMI with the MSP Partner's AWS account only. Modify the key policy to trust a new KMS key that is owned by the MSP Partner for encryption.

D. Export the AMI from the source account to an Amazon S3 bucket in the MSP Partner's AWS account, Encrypt the S3 bucket with a new KMS key that is owned by the MSP Partner. Copy and launch the AMI in the MSP Partner's AWS account.

## Question #80 분석

### ✅ 요구사항
- **AWS MSP 파트너**와 AMI 공유 필요
- **EBS 기반 AMI** (스냅샷 포함)
- **고객 관리형 KMS 키**로 암호화된 EBS 볼륨
- **최고 보안 수준**으로 공유

### ✅ 핵심 보안 고려사항
```yaml
AMI 공유 보안 원칙:
✅ 최소 권한 원칙 (특정 계정만)
✅ 암호화 키 접근 제어
✅ 공개 노출 방지
✅ 감사 추적 가능

KMS 키 관리:
✅ 키 정책 세밀 제어
✅ 교차 계정 접근 최소화
✅ 키 소유권 명확성
```

### ✅ 선택지 분석

**A. AMI/스냅샷 공개 + 키 정책 수정**
- **공개 노출**: AMI와 스냅샷을 모든 계정에 공개 ❌
- **보안 위험**: 전 세계 누구나 접근 가능 ❌
- **최소 권한 위반**: 불특정 다수에게 권한 부여 ❌
- **심각한 보안 결함**: 가장 위험한 접근법 ❌

**B. launchPermission + 특정 계정 + 키 정책 수정** ⭐
- **특정 계정만**: MSP 파트너 계정만 AMI 접근 허용 ✅
- **launchPermission**: AWS 표준 AMI 공유 방식 ✅
- **키 정책 수정**: 파트너 계정에 키 사용 권한 부여 ✅
- **최소 권한**: 필요한 계정에만 최소 권한 ✅
- **표준 보안 방식**: AWS 권장 방법 ✅

**C. launchPermission + 새 KMS 키 신뢰**
- **AMI 공유**: 올바른 방식 ✅
- **새 키 신뢰**: 복잡하고 불필요한 설정 ❌
- **키 관리 복잡성**: 여러 키 간 신뢰 관계 설정 ❌
- **보안 복잡성**: 불필요한 키 의존성 생성 ❌

**D. S3 Export + 새 키로 재암호화**
- **AMI Export**: 복잡하고 시간 소모적 ❌
- **S3 중간 단계**: 불필요한 추가 단계 ❌
- **재암호화**: 시간과 비용 소모 ❌
- **복잡성**: 여러 단계의 수동 프로세스 ❌
- **비효율성**: 간단한 공유를 복잡하게 만듦 ❌

### 📋 핵심 개념 정리

#### **AMI 공유 메커니즘**
```yaml
launchPermission 방식:
- AWS 표준 AMI 공유 방법
- 특정 계정 ID 지정 가능
- 퍼블릭/프라이빗 공유 선택
- 즉시 적용, 즉시 취소 가능

공유 범위:
✅ Private: 특정 계정만
❌ Public: 모든 AWS 계정

보안 수준:
✅ 계정 기반 접근 제어
✅ AMI 메타데이터 보호
✅ 스냅샷 자동 연결
```

#### **KMS 교차 계정 공유**
```yaml
KMS 키 정책 수정:
- Resource-based policy
- 특정 계정/사용자 권한 부여
- 세밀한 작업 권한 제어

필요한 KMS 권한:
- kms:Decrypt: 스냅샷 복호화
- kms:DescribeKey: 키 정보 조회
- kms:CreateGrant: 인스턴스 실행 시 필요
```

#### **정답 시나리오 (B번)**
```yaml
1. AMI launchPermission 설정
aws ec2 modify-image-attribute \
    --image-id ami-12345678 \
    --launch-permission \
    "Add=[{UserId=MSP-ACCOUNT-ID}]"

2. KMS 키 정책 수정
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowMSPPartnerAccess",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::MSP-ACCOUNT-ID:root"
      },
      "Action": [
        "kms:Decrypt",
        "kms:DescribeKey",
        "kms:CreateGrant"
      ],
      "Resource": "*"
    }
  ]
}

3. MSP 파트너 측 인스턴스 실행
aws ec2 run-instances \
    --image-id ami-12345678 \
    --instance-type t3.medium \
    --key-name my-key

4. 권한 확인 및 테스트
- AMI 가시성 확인
- 인스턴스 실행 테스트
- EBS 볼륨 마운트 확인
```

#### **보안 모범 사례**
```yaml
최소 권한 적용:
✅ 특정 MSP 계정만 접근
✅ 필요한 KMS 작업만 허용
✅ 시간 제한 고려 (필요 시)
✅ 정기적 권한 검토

감사 및 모니터링:
✅ CloudTrail로 AMI 사용 추적
✅ KMS 키 사용 로깅
✅ 정기적 접근 검토
✅ 프로젝트 완료 후 권한 취소
```

#### **다른 옵션들의 문제점**

**A안 (공개 공유)의 위험성:**
```yaml
보안 위험:
❌ 전 세계 누구나 AMI 검색 가능
❌ 악의적 사용자의 접근 위험
❌ 기업 지적재산 노출
❌ 규정 준수 위반 가능성

비즈니스 영향:
❌ 데이터 유출 위험
❌ 컴플라이언스 위반
❌ 법적 책임 발생 가능
❌ 브랜드 신뢰도 손상
```

**C안 (키 신뢰)의 복잡성:**
```yaml
기술적 복잡성:
❌ 여러 KMS 키 간 의존성
❌ 키 정책 복잡도 증가
❌ 장애 지점 추가
❌ 문제 해결 어려움

보안 복잡성:
❌ 키 관리 책임 분산
❌ 권한 추적 어려움
❌ 의도치 않은 권한 승격
❌ 감사 추적 복잡
```

**D안 (Export)의 비효율성:**
```yaml
운영 비효율:
❌ 시간 소모적 (시간 단위)
❌ 추가 스토리지 비용
❌ 네트워크 전송 비용
❌ 수동 프로세스 다단계

기술적 제약:
❌ 대용량 AMI 처리 어려움
❌ 네트워크 장애 위험
❌ 데이터 무결성 검증 필요
❌ 버전 관리 복잡
```

