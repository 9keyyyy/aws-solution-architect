## Question #53
A company needs to store its accounting records in Amazon S3. 
The records must be immediately accessible for 1 year and then must be archived for an additional 9 years. 
No one at the company, including administrative users and root users, can be able to delete the records during the entire 10-year period. 
The records must be stored with maximum resiliency.

Which solution will meet these requirements?

A. Store the records in S3 Glacier for the entire 10-year period. Use an access control policy to deny deletion of the records for a period of 10 years.

B. Store the records by using S3 Intelligent-Tiering. Use an IAM policy to deny deletion of the records. After 10 years, change the IAM policy to allow deletion.

C. Use an S3 Lifecycle policy to transition the records from S3 Standard to S3 Glacier Deep Archive after 1 year. Use S3 Object Lock in compliance mode for a period of 10 years.

D. Use an S3 Lifecycle policy to transition the records from S3 Standard to S3 One Zone-Infrequent Access (S3 One Zone-IA) after 1 year. Use S3 Object Lock in governance mode for a period of 10 years.

## Question #53 분석

### ✅ 요구사항
- **1년간 즉시 접근** 가능
- **추가 9년간 아카이브** (총 10년)
- **10년간 삭제 불가능** (관리자/루트 사용자 포함)
- **최대 복원력** (Maximum Resiliency)


### ✅ 선택지 분석

**A. S3 Glacier (10년) + Access Control Policy**
- **즉시 접근 불가**: 1년간 즉시 접근 요구사항 미충족 ❌
- **Access Control Policy**: 루트 사용자 우회 가능 ❌

**B. S3 Intelligent-Tiering + IAM Policy**
- **IAM Policy**: 루트 사용자가 정책 변경 가능 ❌
- **삭제 방지 불완전**: 관리자 권한으로 우회 가능 ❌

**C. S3 Standard → Glacier Deep Archive + Object Lock Compliance** ✅
- **1년 즉시 접근**: S3 Standard로 충족 ✅
- **9년 아카이브**: Glacier Deep Archive로 비용 최적화 ✅
- **삭제 불가능**: Compliance Mode로 완전 보호 ✅
- **최대 복원력**: Multi-AZ, 11 9s 내구성 ✅

**D. S3 Standard → One Zone-IA + Object Lock Governance**
- **단일 AZ**: 복원력 부족 (Maximum Resiliency 미충족) ❌
- **Governance Mode**: 관리자 우회 가능 ❌


### 📋 핵심 개념 정리

#### **S3 스토리지 클래스**
```yaml
S3 Standard: 
  - 즉시 접근, 높은 가용성
  - 11 9s 내구성, Multi-AZ

S3 Glacier Deep Archive:
  - 장기 아카이브용 (최저 비용)
  - 복원 시간: 12시간
  - 11 9s 내구성, Multi-AZ

S3 One Zone-IA:
  - 단일 가용 영역 ❌ (복원력 부족)
```

#### **S3 Object Lock 모드**
```yaml
Compliance Mode:
  ✅ 루트 사용자도 삭제 불가능
  ✅ 보존 기간 단축 불가능
  ✅ 규정 준수 요구사항 충족

Governance Mode:
  ❌ 특별 권한으로 우회 가능
  ❌ 관리자가 삭제 가능
```

### 📋 솔루션 아키텍처 (C번)

#### **Lifecycle 정책**
```yaml
0-1년: S3 Standard
  ✅ 즉시 접근 가능
  ✅ 높은 가용성

1-10년: S3 Glacier Deep Archive  
  ✅ 장기 보관 최적화
  ✅ 비용 효율적
  ✅ 높은 내구성
```

#### **Object Lock Compliance Mode**
```yaml
10년간 보호:
  ✅ 루트 사용자도 삭제 불가
  ✅ 보존 기간 변경 불가
  ✅ 규정 준수 보장
  ✅ 법적 요구사항 충족
```
