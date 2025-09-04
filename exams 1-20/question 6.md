## Question #6
A company uses NFS to store large video files in on-premises network attached storage. 
Each video file ranges in size from 1 MB to 500 GB. 
The total storage is 70 TB and is no longer growing. 
The company decides to migrate the video files to Amazon S3. 
The company must migrate the video files as soon as possible while using the least possible network bandwidth.
Which solution will meet these requirements?

A. Create an S3 bucket. Create an IAM role that has permissions to write to the S3 bucket. Use the AWS CLI to copy all files locally to the S3 bucket. <br>
B. Create an AWS Snowball Edge job. Receive a Snowball Edge device on premises. Use the Snowball Edge client to transfer data to the device. Return the device so that AWS can import the data into Amazon S3. <br>
C. Deploy an S3 File Gateway on premises. Create a public service endpoint to connect to the S3 File Gateway. Create an S3 bucket. Create a new NFS file share on the S3 File Gateway. Point the new file share to the S3 bucket. Transfer the data from the existing NFS file share to the S3 File Gateway. <br>
D. Set up an AWS Direct Connect connection between the on-premises network and AWS. Deploy an S3 File Gateway on premises. Create a public virtual interface (VIF) to connect to the S3 File Gateway. Create an S3 bucket. Create a new NFS file share on the S3 File Gateway. Point the new file share to the S3 bucket. Transfer the data from the existing NFS file share to the S3 File Gateway. <br>

### ✅ 요구사항
- 온프레미스 NFS에 저장된 대용량 비디오 파일들
- 파일 크기: 1MB ~ 500GB (다양한 크기)
- 총 데이터 용량: 70TB (더 이상 증가하지 않음)
- S3로 마이그레이션 필요
- 가능한 한 빨리 + 최소 네트워크 대역폭 사용

### ✅ 선택지 분석
A. AWS CLI를 사용하여 로컬에서 S3 버킷으로 직접 복사

- 70TB를 인터넷으로 업로드 - 매우 오랜 시간 소요
- 높은 네트워크 대역폭 사용
- 업로드 실패 시 재시도 복잡성
- 요구사항의 "최소 네트워크 대역폭" 위배

**B. AWS Snowball Edge 작업 생성 후 물리적 데이터 전송** ⭐
- 70TB → Snowball Edge 용량 (80TB) 내에 수용 가능
- 물리적 디바이스로 오프라인 전송
- 네트워크 대역폭 거의 미사용 (초기 설정만)
- 가장 빠른 대용량 데이터 전송 방법

C. S3 File Gateway를 온프레미스에 배포하여 퍼블릭 엔드포인트로 연결

- 70TB를 인터넷을 통해 전송 - 느리고 비효율적
- 높은 네트워크 대역폭 소모
- File Gateway의 장점 활용 못함 (점진적 마이그레이션이 아님)

D. Direct Connect + S3 File Gateway 조합

- Direct Connect 연결 구축에 시간 소요 (수주~수개월)
- "가능한 한 빨리" 요구사항에 맞지 않음
- 높은 초기 설정 비용 및 복잡성
- 일회성 마이그레이션에는 과도한 인프라

### ✅ 개념 정리
### 1. AWS 데이터 전송 서비스 비교
#### 1.1 AWS Snowball Edge

- 핵심 특징
    - 물리적 데이터 전송 디바이스
    - 용량: 80TB (사용 가능: ~72TB)
    - 오프라인 데이터 전송으로 네트워크 대역폭 절약
    - 대용량 일회성 마이그레이션에 최적
- 작동 방식
    ```
    1. AWS 콘솔에서 Snowball Edge 작업 생성
    2. 암호화된 디바이스 온프레미스 배송 (2-3일)
    3. 온프레미스에서 데이터 복사 (로컬 네트워크 속도)
    4. 디바이스 AWS로 반송 (2-3일)
    5. AWS 데이터센터에서 S3로 자동 업로드
    ```

### 2. S3 File Gateway 분석
#### 2.1 S3 File Gateway란?

- 하이브리드 클라우드 스토리지 서비스
- 온프레미스에서 NFS/SMB 인터페이스를 통해 S3 접근
- 로컬 캐시를 통한 성능 최적화
- 점진적/지속적 데이터 동기화에 적합

#### 2.2 File Gateway의 적절한 사용 사례
```
✅ 좋은 사용 사례:
- 지속적인 파일 공유 및 동기화
- 온프레미스 애플리케이션의 클라우드 확장
- 백업 및 아카이브 자동화

❌ 부적절한 사용 사례:
- 대용량 일회성 마이그레이션
- 네트워크 대역폭이 제한된 환경
- 빠른 완료가 필요한 프로젝트
```
