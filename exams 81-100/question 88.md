## Question #88
A survey company has gathered data for several years from areas in the United States. 
The company hosts the data in an Amazon S3 bucket that is 3 TB in size and growing. 
The company has started to share the data with a European marketing firm that has S3 buckets. 
The company wants to ensure that its data transfer costs remain as low as possible.

Which solution will meet these requirements?

A. Configure the Requester Pays feature on the company's S3 bucket.

B. Configure S3 Cross-Region Replication from the company's S3 bucket to one of the marketing firm's S3 buckets.

C. Configure cross-account access for the marketing firm so that the marketing firm has access to the company's S3 bucket.

D. Configure the company's S3 bucket to use S3 Intelligent-Tiering. Sync the S3 bucket to one of the marketing firm's S3 buckets.

## Question #88 분석

### ✅ 요구사항
- **3TB+ S3 데이터** 유럽 마케팅 회사와 공유
- **데이터 전송 비용 최소화** 우선
- **지속적 데이터 공유** 필요

### ✅ 선택지 분석

**A. Requester Pays 설정** ⭐
- **비용 전가**: 유럽 회사가 데이터 전송 비용 부담 ✅
- **회사 비용 0**: 데이터 전송 비용 완전 제거 ✅
- **즉시 적용**: 설정만으로 비용 절감 ✅

**B. Cross-Region Replication**
- **중복 비용**: 초기 3TB 복제 + 지속적 동기화 비용 ❌
- **스토리지 비용**: 유럽 리전 추가 스토리지 비용 ❌
- **높은 총비용**: 가장 비싼 옵션 ❌

**C. Cross-Account Access 설정**
- **동일 비용**: 회사가 여전히 모든 전송 비용 부담 ❌
- **비용 절감 없음**: 요구사항 미충족 ❌

**D. Intelligent-Tiering + Sync**
- **스토리지 최적화**: 전송 비용과 무관 ❌
- **Sync 비용**: 추가 전송 비용 발생 ❌
- **목적 불일치**: 문제 해결 안됨 ❌

### 📋 Requester Pays 작동 방식

```yaml
설정 후:
- 데이터 소유자: 스토리지 비용만 부담
- 데이터 요청자: 모든 전송 비용 부담
- 회사 절감: 100% 데이터 전송 비용 제거

비용 예시 (월 100GB 전송):
- Before: 회사 $9 부담
- After: 유럽 회사 $9 부담, 회사 $0
```

### 💰 비용 비교

```yaml
A안 (Requester Pays): $0 (회사 기준)
B안 (Replication): $300+ (초기) + $50/월
C안 (Cross-Account): $90/월 (동일)
D안 (Sync): $100+/월 (추가 비용)
```

**정답: A**

**핵심 이유**: Requester Pays 설정으로 데이터 사용자가 전송 비용을 부담하게 하여 회사의 데이터 전송 비용을 완전히 제거할 수 있습니다.