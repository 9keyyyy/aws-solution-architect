## Question #1003
A company runs its media rendering application on premises. The company wants to reduce storage costs and has moved all data to Amazon S3. The on-premises rendering application needs low-latency access to storage.

The company needs to design a storage solution for the application. The storage solution must maintain the desired application performance.

Which storage solution will meet these requirements in the MOST cost-effective way?

A. Use Mountpoint for Amazon S3 to access the data in Amazon S3 for the on-premises application.

B. Configure an Amazon S3 File Gateway to provide storage for the on-premises application.

C. Copy the data from Amazon S3 to Amazon FSx for Windows File Server. Configure an Amazon FSx File Gateway to provide storage for the on-premises application.

D. Configure an on-premises file server. Use the Amazon S3 API to connect to S3 storage. Configure the application to access the storage from the on-premises file server.

## Question #1003 분석

### 문제 상황
- **온프레미스 미디어 렌더링** 애플리케이션
- 모든 데이터를 **S3로 이동** 완료
- **낮은 지연 시간** 필요
- **비용 효율성** 중요

### 핵심 요구사항
- **S3 데이터 접근**
- **온프레미스에서 낮은 지연 시간**
- **비용 효율적**
- **기존 성능 유지**

### 선택지 분석

**A. Mountpoint for Amazon S3**
- S3를 파일 시스템처럼 마운트 ✅
- 낮은 지연 시간 → 제한적 (인터넷 경유) ❌
- 캐싱 없음 → 반복 접근시 비효율 ❌

**B. S3 File Gateway** ⭐
- 온프레미스 캐싱 → 낮은 지연 시간 ✅
- S3 데이터 직접 활용 ✅
- 비용 효율적 ✅
- NFS/SMB 인터페이스 ✅

**C. FSx + FSx File Gateway**
- 데이터를 S3 → FSx로 복사 → 추가 비용 ❌
- 이중 저장 → 비효율적 ❌

**D. 온프레미스 파일 서버 + S3 API**
- 자체 구축 → 관리 부담 ❌
- 캐싱 로직 직접 구현 → 복잡성 ❌

### Storage Gateway 타입

**S3 File Gateway**
```yaml
기능:
- S3를 NFS/SMB로 노출
- 온프레미스 로컬 캐싱
- 자주 사용하는 데이터는 로컬에 유지
- 백그라운드에서 S3와 동기화

장점:
- 낮은 지연 시간 (로컬 캐시)
- S3 비용 그대로 활용
- 관리형 서비스
```
