## Question #72
A company runs a photo processing application that needs to frequently upload and download pictures from Amazon S3 buckets that are located in the same AWS Region. 
A solutions architect has noticed an increased cost in data transfer fees and needs to implement a solution to reduce these costs.

How can the solutions architect meet this requirement?

A. Deploy Amazon API Gateway into a public subnet and adjust the route table to route S3 calls through it.

B. Deploy a NAT gateway into a public subnet and attach an endpoint policy that allows access to the S3 buckets.

C. Deploy the application into a public subnet and allow it to route through an internet gateway to access the S3 buckets.

D. Deploy an S3 VPC gateway endpoint into the VPC and attach an endpoint policy that allows access to the S3 buckets.

## Question #72 분석

### ✅ 요구사항
- **포토 프로세싱 애플리케이션** 운영
- **S3 버킷**에 빈번한 업로드/다운로드
- **동일 리전** 내 S3 버킷 사용
- **데이터 전송 비용 증가** 문제
- **비용 절감** 솔루션 필요

### ✅ 선택지 분석

**A. API Gateway를 퍼블릭 서브넷에 배포**
- **API Gateway 목적**: RESTful API 관리 서비스 ❌
- **S3 접근 최적화**: API Gateway는 S3 직접 최적화 아님 ❌
- **비용 추가**: API Gateway 사용료 추가 발생 ❌
- **복잡성 증가**: 불필요한 중간 계층 추가 ❌
- **잘못된 접근**: 데이터 전송 비용 해결 불가 ❌

**B. NAT Gateway + 엔드포인트 정책**
- **NAT Gateway 목적**: 프라이빗 서브넷의 아웃바운드 인터넷 접근 ❌
- **비용 증가**: NAT Gateway 비용이 더 비쌈 ❌
- **데이터 전송 비용**: 여전히 인터넷을 통한 S3 접근 ❌
- **문제 해결 실패**: 근본 원인 해결 안됨 ❌

**C. 퍼블릭 서브넷 + 인터넷 게이트웨이**
- **인터넷 경유**: S3 접근이 인터넷을 통해 진행 ❌
- **데이터 전송 비용**: 비용 문제 해결 안됨 ❌
- **보안 위험**: 불필요한 퍼블릭 노출 ❌
- **현재 상황과 동일**: 문제 상황 지속 ❌

**D. S3 VPC Gateway Endpoint 배포** ⭐
- **VPC 내부 통신**: 인터넷 우회, AWS 백본 네트워크 사용 ✅
- **데이터 전송 비용 제거**: 동일 리전 VPC Endpoint는 무료 ✅
- **성능 향상**: 더 빠른 네트워크 경로 ✅
- **보안 강화**: 트래픽이 인터넷에 노출되지 않음 ✅
- **정책 제어**: 세밀한 S3 접근 제어 가능 ✅

### 📋 핵심 개념 정리

#### **S3 접근 경로 비교**
```yaml
인터넷 경유 (기존):
Application → Internet Gateway → Internet → S3
- 비용: 데이터 전송 요금 발생
- 경로: 인터넷을 통한 긴 경로
- 보안: 인터넷 노출
- 성능: 상대적으로 느림

VPC Gateway Endpoint (최적):
Application → VPC Gateway Endpoint → S3
- 비용: 무료 (동일 리전)
- 경로: AWS 백본 네트워크 직접
- 보안: VPC 내부 통신
- 성능: 최적화된 경로
```

#### **VPC Gateway Endpoint 특징**
```yaml
S3 VPC Gateway Endpoint:
  ✅ 완전 무료 서비스
  ✅ 동일 리전 내 무제한 사용
  ✅ 인터넷 우회로 보안 강화
  ✅ 더 빠른 성능
  ✅ 라우팅 테이블 자동 업데이트

설정 요구사항:
  - VPC 내 프라이빗 서브넷
  - 라우팅 테이블 연결
  - 엔드포인트 정책 설정
  - S3 버킷 정책 연동
```

#### **데이터 전송 비용 구조**
```yaml
AWS 데이터 전송 요금:
인터넷 → AWS: 무료
AWS → 인터넷: 유료 ($0.09/GB)
동일 AZ 내: 무료
다른 AZ 간: $0.01/GB
다른 리전 간: $0.02/GB

VPC Endpoint 사용 시:
✅ S3 접근: 완전 무료
✅ 동일 리전: 전송 비용 없음
✅ VPC 내부: 인터넷 요금 회피
✅ 대용량 처리: 비용 절감 극대화
```

#### **포토 프로세싱 애플리케이션 시나리오**
```yaml
현재 상황 (비용 발생):
1. 애플리케이션이 S3에 사진 업로드
2. 인터넷 게이트웨이 → 인터넷 → S3
3. 대용량 이미지 전송으로 높은 요금

최적화 후 (비용 절감):
1. S3 VPC Gateway Endpoint 설치
2. 애플리케이션 → VPC Endpoint → S3
3. AWS 백본 네트워크로 직접 전송
4. 데이터 전송 비용 완전 제거
```

#### **정답 시나리오 (D번)**
```yaml
1. S3 VPC Gateway Endpoint 생성
   - VPC 선택
   - 서비스: com.amazonaws.region.s3
   - 라우팅 테이블 연결

2. 엔드포인트 정책 설정
   {
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": "*",
         "Action": [
           "s3:GetObject",
           "s3:PutObject",
           "s3:DeleteObject"
         ],
         "Resource": [
           "arn:aws:s3:::my-photo-bucket/*"
         ]
       }
     ]
   }

3. 라우팅 테이블 자동 업데이트
   - S3 트래픽이 자동으로 VPC Endpoint로 라우팅
   - 인터넷 게이트웨이 우회

4. 결과
   ✅ 데이터 전송 비용 100% 절감
   ✅ 성능 향상 (더 빠른 경로)
   ✅ 보안 강화 (인터넷 노출 없음)
   ✅ 설정 간단 (추가 비용 없음)
```
