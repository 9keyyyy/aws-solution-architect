## Question #1000
A company currently stores 5 TB of data in on-premises block storage systems. The company's current storage solution provides limited space for additional data. The company runs applications on premises that must be able to retrieve frequently accessed data with low latency. The company requires a cloud-based storage solution.

Which solution will meet these requirements with the MOST operational efficiency?

A. Use Amazon S3 File Gateway. Integrate S3 File Gateway with the on-premises applications to store and directly retrieve files by using the SMB file system.

B. Use an AWS Storage Gateway Volume Gateway with cached volumes as iSCSI targets.

C. Use an AWS Storage Gateway Volume Gateway with stored volumes as iSCSI targets.

D. Use an AWS Storage Gateway Tape Gateway. Integrate Tape Gateway with the on-premises applications to store virtual tapes in Amazon S3.

## Question #1000 분석

### 문제 상황
- **5TB 블록 스토리지** 온프레미스
- **제한된 추가 공간**
- **자주 접근하는 데이터** 낮은 지연 시간 필요
- **클라우드 기반 솔루션** 필요

### 핵심 요구사항
- **블록 스토리지** 호환성
- **자주 접근하는 데이터** 빠른 접근
- **최대 운영 효율성**

### Storage Gateway 타입별 특징

| 타입 | 인터페이스 | 용도 | 캐싱 |
|------|------------|------|------|
| **File Gateway** | NFS/SMB | 파일 저장 | 로컬 |
| **Volume Gateway (Cached)** | iSCSI | 블록 저장 | 로컬 |
| **Volume Gateway (Stored)** | iSCSI | 블록 저장 | 클라우드 |
| **Tape Gateway** | VTL | 백업/아카이브 | - |

### 선택지 분석

**A. S3 File Gateway + SMB**
- 파일 시스템 (NFS/SMB) ≠ 블록 스토리지 ❌
- 현재 블록 스토리지 애플리케이션과 호환성 문제 ❌

**B. Volume Gateway (Cached Volumes)** ⭐
- iSCSI 블록 인터페이스 ✅
- 자주 접근 데이터 로컬 캐싱 ✅
- 낮은 지연 시간 ✅
- 무제한 클라우드 확장 ✅

**C. Volume Gateway (Stored Volumes)**
- 주 데이터가 온프레미스 → 확장성 제한 ❌
- 여전히 로컬 스토리지 용량 문제 ❌

**D. Tape Gateway**
- 백업/아카이브용 → 애플리케이션 직접 접근 부적합 ❌

### Cached vs Stored Volumes

**Cached Volumes**
```yaml
특징:
- 주 데이터: S3 (무제한)
- 캐시: 온프레미스 (자주 접근)
- 확장성: 무제한

장점:
- 공간 제한 해결
- 빠른 접근 + 무제한 확장
```

**Stored Volumes**
```yaml
특징:
- 주 데이터: 온프레미스
- 백업: S3
- 확장성: 로컬 스토리지에 의존

단점:
- 여전히 공간 제한 문제
```
