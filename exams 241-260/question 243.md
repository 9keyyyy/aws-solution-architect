## Question #243
A medical research lab produces data that is related to a new study. 
The lab wants to make the data available with minimum latency to clinics across the country for their on-premises, file-based applications. 
The data files are stored in an Amazon S3 bucket that has read-only permissions for each clinic.

What should a solutions architect recommend to meet these requirements?

A. Deploy an AWS Storage Gateway file gateway as a virtual machine (VM) on premises at each clinic

B. Migrate the files to each clinic’s on-premises applications by using AWS DataSync for processing.

C. Deploy an AWS Storage Gateway volume gateway as a virtual machine (VM) on premises at each clinic.

D. Attach an Amazon Elastic File System (Amazon EFS) file system to each clinic’s on-premises servers.

## Question #243 분석

### 요구사항
- **의료 연구 데이터** S3 저장
- **전국 클리닉**에 최소 지연시간으로 제공
- **온프레미스 파일 기반** 애플리케이션
- **읽기 전용** 권한

### 선택지 분석

**A. Storage Gateway File Gateway** ⭐
- **파일 기반 접근**: 온프레미스 애플리케이션이 NFS로 직접 접근 ✅
- **지연시간 최소화**: 로컬 캐시로 자주 사용하는 파일 저장 ✅
- **S3 연동**: 백그라운드에서 S3와 자동 동기화 ✅
- **읽기 최적화**: 캐시 히트 시 극도로 빠른 접근 ✅

**B. DataSync를 통한 파일 마이그레이션**
- **일회성 전송**: 지속적인 동기화에 적합하지 않음 ❌
- **수동 관리**: 새 데이터 업데이트 시 수동 재전송 필요 ❌
- **확장성 부족**: 연구소에서 지속적 데이터 업데이트 시 비효율 ❌

**C. Storage Gateway Volume Gateway**
- **블록 스토리지**: 파일 기반 애플리케이션에 부적합 ❌
- **복잡성**: iSCSI 설정으로 파일 접근 방식과 불일치 ❌

**D. Amazon EFS를 온프레미스 서버에 연결**
- **네트워크 의존**: 인터넷을 통한 EFS 접근으로 지연시간 증가 ❌
- **데이터 위치**: S3의 기존 데이터를 EFS로 이전해야 함 ❌

### File Gateway 작동 원리

```yaml
아키텍처:
연구소: S3 버킷 (원본 데이터)
각 클리닉: File Gateway VM + 로컬 캐시

데이터 흐름:
1. 연구소가 S3에 새 데이터 업로드
2. File Gateway가 변경사항 감지
3. 클리닉 애플리케이션이 NFS로 파일 요청
4. 캐시 히트: 즉시 응답 (최소 지연시간)
5. 캐시 미스: S3에서 다운로드 후 캐시 저장
```

