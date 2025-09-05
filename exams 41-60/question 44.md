## Question #44
A company has an Amazon S3 bucket that contains critical data. 
The company must protect the data from accidental deletion.

Which combination of steps should a solutions architect take to meet these requirements? (Choose two.)

A. Enable versioning on the S3 bucket.

B. Enable MFA Delete on the S3 bucket.

C. Create a bucket policy on the S3 bucket.

D. Enable default encryption on the S3 bucket.

E. Create a lifecycle policy for the objects in the S3 bucket.

## Question #44 분석

### ✅ 요구사항
- S3 버킷의 **중요한 데이터** 보호
- **실수로 인한 삭제** 방지
- **2개 조합** 선택

### ✅ 선택지 분석

**A. 버전 관리 활성화** ⭐
- **삭제 보호**: 삭제 시 실제 데이터 보존, 삭제 마커만 생성 ✅
- **즉시 복구**: 이전 버전으로 언제든 복원 가능 ✅
- **실수 방지 핵심**: 실수 삭제의 1차 방어선 ✅

**B. MFA Delete 활성화** ⭐
- **다단계 인증**: 객체 버전 영구 삭제 시 MFA 토큰 필요 ✅
- **강화된 보안**: 무단 삭제 및 악의적 삭제 방지 ✅
- **버전 관리 연계**: A번과 함께 2중 보호 체계 ✅

**C. 버킷 정책 생성**
- **복잡한 설정**: 조건부 로직으로 관리 복잡 
- **제한적 효과**: 정책 오류 시 의도치 않은 결과 
- **불완전한 보호**: 버전 관리만큼 확실하지 않음

**D. 기본 암호화 활성화**
- **다른 보안 영역**: 저장 중 데이터 암호화 
- **삭제 방지 무관**: 실수 삭제 문제 해결 안함 

**E. 라이프사이클 정책**
- **자동 삭제 설정**: 오히려 데이터 삭제 위험 증가 
- **목적 상반**: 보존이 아닌 정리가 목적 

### 📋 A + B 조합의 보호 메커니즘

### **버전 관리 (A번)**
```yaml
실수 삭제 시:
  - 실제 데이터: 그대로 보존
  - 생성되는 것: 삭제 마커만
  - 복구 방법: 삭제 마커 제거로 즉시 복원
  - 사용자 관점: 파일이 "삭제"된 것처럼 보임
```

### **MFA Delete (B번)**
```yaml
보호 강화:
  - 버전 영구 삭제: MFA 토큰 필요
  - 버전 관리 해제: MFA 토큰 필요
  - 효과: 2단계 인증으로 실수/악의적 삭제 차단
```

### 🔄 실제 보호 시나리오

### **일반 사용자 실수**
```yaml
1. 파일 삭제 명령 실행
2. 버전 관리: 삭제 마커 생성, 원본 보존
3. 복구: 관리자가 삭제 마커 제거
4. 결과: 즉시 파일 복원 완료
```

### **악의적 삭제 시도**
```yaml
1. 사용자가 버전까지 영구 삭제 시도
2. MFA Delete: 물리적 토큰 요구
3. 토큰 없이는 삭제 불가능
4. 보안 로그에 시도 기록
```
