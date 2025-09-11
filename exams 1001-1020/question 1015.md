## Question #1015
A company's image-hosting website gives users around the world the ability to upload, view, and download images from their mobile devices. 
The company currently hosts the static website in an Amazon S3 bucket.

Because of the website's growing popularity, the website's performance has decreased. Users have reported latency issues when they upload and download images.

The company must improve the performance of the website.

Which solution will meet these requirements with the LEAST implementation effort?

A. Configure an Amazon CloudFront distribution for the S3 bucket to improve the download performance. Enable S3 Transfer Acceleration to improve the upload performance.

B. Configure Amazon EC2 instances of the right sizes in multiple AWS Regions. Migrate the application to the EC2 instances. Use an Application Load Balancer to distribute the website traffic equally among the EC2 instances. Configure AWS Global Accelerator to address global demand with low latency.

C. Configure an Amazon CloudFront distribution that uses the S3 bucket as an origin to improve the download performance. Configure the application to use CloudFront to upload images to improve the upload performance. Create S3 buckets in multiple AWS Regions. Configure replication rules for the buckets to replicate users' data based on the users' location. Redirect downloads to the S3 bucket that is closest to each user's location.

D. Configure AWS Global Accelerator for the S3 bucket to improve network performance. Create an endpoint for the application to use Global Accelerator instead of the S3 bucket.

## Question #1015 분석

### 문제 상황
- **글로벌 이미지 호스팅 웹사이트** (S3 정적 웹사이트)
- **업로드/다운로드 지연 문제**
- **최소 구현 노력**으로 성능 개선 필요

### 핵심 요구사항
- **업로드 성능** 향상
- **다운로드 성능** 향상  
- **최소 구현 노력**

### 성능 개선 기술

**다운로드 성능**: CloudFront (캐싱 + 글로벌 엣지)
**업로드 성능**: S3 Transfer Acceleration (글로벌 엣지 활용)

### 선택지 분석

**A. CloudFront + S3 Transfer Acceleration** ⭐
```yaml
다운로드: CloudFront로 캐싱 ✅
업로드: Transfer Acceleration ✅
구현: 간단 (설정만) ✅
비용: 합리적 ✅
```

**B. EC2 + ALB + Global Accelerator**
```yaml
성능: ✅ 개선 가능
구현: ❌ 대규모 마이그레이션 필요
노력: ❌ 애플리케이션 재구성
비용: ❌ 높은 운영 비용
```

**C. CloudFront + 다중 리전 S3 + 복제**
```yaml
성능: ✅ 매우 좋음
구현: ❌ 복잡한 아키텍처
관리: ❌ 다중 버킷 관리
노력: ❌ 최소 노력 위배
```

**D. Global Accelerator for S3**
```yaml
기능: ❌ Global Accelerator는 S3와 직접 연동 안됨
구현: ❌ 기술적으로 불가능
```

### S3 Transfer Acceleration

```yaml
작동 방식:
일반 업로드: 사용자 → S3 (직접)
가속 업로드: 사용자 → CloudFront Edge → S3

장점:
- 50-500% 업로드 속도 향상
- 설정만으로 활성화
- 추가 코드 변경 최소
```

### 구현 복잡도 비교

```yaml
Option A:
1. CloudFront 배포 생성 (클릭 몇 번)
2. S3 Transfer Acceleration 활성화 (클릭 1번)
3. DNS 변경
→ 최소 노력 ✅

Option B:
1. EC2 인스턴스 설계/배포
2. 애플리케이션 마이그레이션
3. ALB 구성
4. Global Accelerator 설정
→ 대규모 작업 ❌

Option C:
1. 다중 리전 S3 버킷 생성
2. 복제 규칙 설정
3. 지리적 라우팅 로직 구현
4. CloudFront 복잡 설정
→ 복잡한 아키텍처 ❌
```
