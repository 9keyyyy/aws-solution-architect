## Question #221
A company runs an application on a group of Amazon Linux EC2 instances. 
For compliance reasons, the company must retain all application log files for 7 years. 
The log files will be analyzed by a reporting tool that must be able to access all the files concurrently.

Which storage solution meets these requirements MOST cost-effectively?

A. Amazon Elastic Block Store (Amazon EBS)

B. Amazon Elastic File System (Amazon EFS)

C. Amazon EC2 instance store

D. Amazon S3

## Question #221 분석

### 요구사항
- **Amazon Linux EC2** 애플리케이션 로그
- **7년간 보존** (규정 준수)
- **동시 접근**: 리포팅 툴이 모든 파일에 동시 접근
- **최고 비용 효율성**

### 선택지 분석

**A. Amazon EBS**
- **EC2 종속성**: 인스턴스별 개별 스토리지 ❌
- **동시 접근 제한**: 하나의 인스턴스만 연결 가능 ❌
- **확장성 부족**: 여러 인스턴스 간 공유 불가 ❌

**B. Amazon EFS**
- **동시 접근**: 여러 EC2에서 동시 마운트 가능 ✅
- **7년 보존**: 장기 저장 지원 ✅
- **비용**: NFS 프로토콜 오버헤드로 상대적 고비용 ❌

**C. Amazon EC2 instance store**
- **휘발성**: 인스턴스 종료 시 데이터 손실 ❌
- **7년 보존 불가능**: 영구 저장소 아님 ❌
- **규정 준수 위험**: 데이터 손실 위험 ❌

**D. Amazon S3** ⭐
- **동시 접근**: 무제한 동시 접근 지원 ✅
- **7년 보존**: 무제한 저장 기간 ✅
- **비용 효율**: 장기 저장에 최적화된 가격 ✅
- **내구성**: 99.999999999% 데이터 내구성 ✅

### 비용 비교 (7년 보존)

```yaml
EFS vs S3 (1TB 로그 기준):
EFS Standard: $0.30/GB/월 × 1000GB × 84개월 = $25,200
S3 Standard: $0.023/GB/월 × 1000GB × 84개월 = $1,932
S3 IA: $0.0125/GB/월 × 1000GB × 84개월 = $1,050

S3가 90% 이상 저렴
```

### 동시 접근 특성

```yaml
S3 동시 접근:
- HTTP/HTTPS API 기반
- 무제한 동시 요청
- 글로벌 액세스 가능
- 리포팅 툴 직접 연동

EFS 동시 접근:
- NFS 마운트 필요
- 동시 연결 수 제한
- VPC 내부만 접근
- 추가 네트워크 설정
```
