## Question #1
A company collects data for temperature, humidity, and atmospheric pressure in cities across multiple continents. The average volume of data that the company collects from each site daily is 500 GB. Each site has a high-speed Internet connection.
The company wants to aggregate the data from all these global sites as quickly as possible in a single Amazon S3 bucket. The solution must minimize operational complexity.
Which solution meets these requirements?

A. Turn on S3 Transfer Acceleration on the destination S3 bucket. Use multipart uploads to directly upload site data to the destination S3 bucket.
<br>
B. Upload the data from each site to an S3 bucket in the closest Region. Use S3 Cross-Region Replication to copy objects to the destination S3 bucket. Then remove the data from the origin S3 bucket.
<br>
C. Schedule AWS Snowball Edge Storage Optimized device jobs daily to transfer data from each site to the closest Region. Use S3 Cross-Region Replication to copy objects to the destination S3 bucket.
<br>
D. Upload the data from each site to an Amazon EC2 instance in the closest Region. Store the data in an Amazon Elastic Block Store (Amazon EBS) volume. At regular intervals, take an EBS snapshot and copy it to the Region that contains the destination S3 bucket. Restore the EBS volume in that Region.

### ✅ 요구사항:
- 전세계 여러 사이트에서 매일 500GB 수집
- 모든 사이트의 데이터를 단일 s3 버킷에 최대한 빠르게 집계
- 운영 복잡성 최소화
- 각 사이트는 고속 인터넷 연결 보유

### ✅ 선택지 분석
**A. S3 Transfer Acceleration + 멀티파트 업로드** [정답]
- Transfer Acceleration으로 전세계에서 S3로의 업로드 속도 최적화
- 멀티파트 업로드로 대용량 파일 전송 효율성 향상
- 직접 목적지 버킷에 업로드하여 단순함 + 추가 인프라/복잡한 설정 불필요

B. 지역별 S3 버킷 + Cross-Region Replication
- 지역별로 가까운 곳에 먼저 업로드하여 초기 업로드 속도 향상
- 업로드 -> 복제 프로세스로 전체 시간은 증가함
- 중간 버킷 관리 필요하여 운영 복잡성 증가
- 원본 데이터 삭제 과정 필요 + 스토리지 비용 중복 발생

C. Snowball Edge + Cross-Region Replication
- 매우 대용량 데이터에 적합(50TB-100TB)
- 500GB는 Snowball을 사용할만큼 큰 용량은 아님
- 물리적 배송으로 인한 지연 시간 발생
- 매일 배송은 비현실적 + 복잡 + 높은 운영 복잡성

D. EC2 + EBS 스냅샷
- 매우 복잡한 아키텍처
- EC2 인스턴스, EBS 볼륨 관리 필요
- 스냅샷 생성 및 복사 과정 느림 + S3 사용 대비 불필요하게 복잡

### ✅ 개념 정리
1. **Cross-Region Replication (CRR)**
    - 정의: 한 AWS 리전의 S3 버킷에서 다른 리전의 S3 버킷으로 객체를 자동으로 복사하는 기능
    - 작동 방식:
      - 원본 버킷에 파일이 업로드되면 자동으로 목적지 버킷에 복사
      - 실시간에 가까운 복제 (보통 15분 이내)
      - 한 번 설정하면 자동으로 동작
    - 사용 목적:
      - 재해 복구 (백업)
      - 지역별 데이터 접근 속도 향상
      - 규정 준수 (특정 지역에 데이터 보관 요구사항)
2. **S3 Transfer Acceleration**
    ```
    일반적인 업로드:
    한국 사이트 ────────────────────→ 미국 S3 버킷
          (느린 대륙간 직접 연결)
    
    Transfer Acceleration:
    한국 사이트 → 서울 엣지 로케이션 ⚡ 고속 백본 네트워크 → 미국 S3 버킷
        (빠름)         (AWS 내부 초고속 네트워크)
    ```
    핵심 아이디어:
    - 사용자는 가장 가까운 엣지 로케이션에 업로드 (빠름)
    - 엣지에서 S3까지는 AWS의 최적화된 내부 네트워크 사용 (매우 빠름)
    
    엣지 로케이션이란?
    - CloudFront CDN의 전 세계 캐시 서버들
    - 전 세계 400+ 개 지점에 분산
