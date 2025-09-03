## Question #9
A company is running an SMB file server in its data center. 
The file server stores large files that are accessed frequently for the first few days after the files are created. 
After 7 days the files are rarely accessed.
The total data size is increasing and is close to the company's total storage capacity. 
A solutions architect must increase the company's available storage space without losing low-latency access to the most recently accessed files. 
The solutions architect must also provide file lifecycle management to avoid future storage issues.

Which solution will meet these requirements?

A. Use AWS DataSync to copy data that is older than 7 days from the SMB file server to AWS.

B. Create an Amazon S3 File Gateway to extend the company's storage space. Create an S3 Lifecycle policy to transition the data to S3 Glacier Deep Archive after 7 days.

C. Create an Amazon FSx for Windows File Server file system to extend the company's storage space.

D. Install a utility on each user's computer to access Amazon S3. Create an S3 Lifecycle policy to transition the data to S3 Glacier Flexible Retrieval after 7 days.



### **✅ 요구사항:**
* SMB 파일 서버가 온프레미스 데이터센터에서 실행 중
* 대용량 파일들이 **생성 후 며칠간 자주 접근**됨
* **7일 후에는 거의 접근되지 않음**
* 총 데이터 크기가 증가하여 스토리지 용량 한계에 도달
* **최근 접근 파일에 대한 저지연 접근 유지** 필요
* **파일 수명 주기 관리**로 향후 스토리지 문제 방지

### **✅ 선택지 분석**

**A. AWS DataSync로 7일 이상 된 데이터를 AWS로 복사**
* **일방향 복사**만 수행 - 원본 파일은 온프레미스에 남아있음
* 스토리지 공간 확장 효과 없음 ❌
* 사용자는 여전히 SMB 인터페이스로만 접근
* 근본적인 용량 문제 해결 안됨

**B. Amazon S3 File Gateway + S3 Lifecycle 정책** ⭐
* **하이브리드 스토리지**: 온프레미스 캐시 + 클라우드 백엔드
* **SMB 인터페이스 유지**: 사용자 경험 변화 없음
* **자동 계층화**: 자주 접근하는 파일은 로컬 캐시에 유지
* **수명 주기 관리**: 7일 후 자동으로 Glacier Deep Archive로 이동

**C. Amazon FSx for Windows File Server로 스토리지 공간 확장**
* 클라우드 파일 시스템이지만 **수명 주기 관리 없음** ❌
* 모든 데이터가 고비용 스토리지에 계속 보관
* 향후 스토리지 비용 증가 문제 해결 안됨
* 네트워크 지연으로 온프레미스 대비 성능 저하

**D. 각 사용자 컴퓨터에 S3 접근 유틸리티 설치**
* **사용자 경험 완전 변경** - SMB에서 S3 접근으로 전환 ❌
* 기존 애플리케이션과 호환성 문제
* 사용자 교육 및 관리 복잡성 증가
* **저지연 접근 보장 어려움**

### **✅ 개념 정리**

### **1. Amazon S3 File Gateway (B번 솔루션)**

**1.1 핵심 특징**
* **하이브리드 클라우드 스토리지 게이트웨이**
* 온프레미스에 가상머신으로 배포
* **NFS/SMB 인터페이스**를 통해 S3 버킷에 투명하게 접근
* **로컬 캐시**로 자주 접근하는 데이터의 저지연 보장

**1.2 S3 File Gateway 아키텍처**
```
사용자/애플리케이션 → SMB 프로토콜 → S3 File Gateway (온프레미스)
                                        ↓ 인터넷/Direct Connect
                                    Amazon S3 Bucket
                                        ↓ Lifecycle Policy
                                    S3 Glacier Deep Archive
```

**1.3 작동 방식**
```python
# 파일 쓰기 과정
1. 사용자가 SMB 공유에 파일 저장: \\file-gateway\share\video.mp4
2. File Gateway가 파일을 로컬 캐시에 저장 (즉시 접근 가능)
3. 백그라운드로 S3 버킷에 비동기 업로드
4. 로컬 캐시 공간 부족 시 오래된 파일부터 캐시에서 제거

# 파일 읽기 과정  
1. 사용자가 파일 요청: \\file-gateway\share\old-video.mp4
2. 로컬 캐시에 있으면 즉시 반환 (저지연)
3. 캐시에 없으면 S3에서 다운로드 후 캐시에 저장하며 반환
```

**1.4 캐시 관리 메커니즘**
```
로컬 캐시 (SSD/HDD):
├─ 최근 접근 파일들 (hot data) - 즉시 접근 가능
├─ 자주 접근하는 파일들 - 캐시에 유지
└─ LRU 알고리즘으로 오래된 파일 제거

S3 Standard (백엔드):
├─ 모든 파일의 원본 저장
└─ 캐시 미스 시 여기서 다운로드

S3 Glacier Deep Archive:
└─ 7일 후 Lifecycle Policy로 자동 이동
```

### **2. S3 Lifecycle 정책 구성**

**2.1 Lifecycle 규칙 설정**
```json
{
  "Rules": [
    {
      "ID": "FileServerArchivePolicy",
      "Status": "Enabled",
      "Filter": {
        "Prefix": "file-server-data/"
      },
      "Transitions": [
        {
          "Days": 7,
          "StorageClass": "DEEP_ARCHIVE"
        }
      ]
    }
  ]
}
```

**2.2 스토리지 클래스별 특성**

| 스토리지 클래스 | 접근 지연 시간 | 비용 (GB/월) | 사용 사례 |
|----------------|---------------|-------------|----------|
| **S3 Standard** | 밀리초 | $0.023 | 자주 접근하는 데이터 |
| **S3 IA** | 밀리초 | $0.0125 | 가끔 접근하는 데이터 |
| **S3 Glacier Flexible** | 1-5분 | $0.004 | 월 1-2회 접근 |
| **S3 Glacier Deep Archive** | 12시간 | $0.00099 | 거의 접근 안 함 |

**2.3 비용 효율성**
```
기존 온프레미스 스토리지:
- 100TB 전체를 고성능 스토리지로 유지
- 월 비용: ~$10,000-15,000 (하드웨어 상각 + 전력 + 관리)

S3 File Gateway 적용 후:
- 로컬 캐시 10TB: ~$1,000/월 (SSD)  
- S3 Standard 10TB (최근 7일): ~$230/월
- Glacier Deep Archive 90TB: ~$90/월
- 총 월 비용: ~$1,320/월 (약 85% 절약!)
```

### **3. 잘못된 선택지들 심화 분석**

**3.1 A번: DataSync의 한계**

**DataSync 실제 용도:**
```
✅ 좋은 사용 사례:
- 일회성 마이그레이션
- 정기적 백업 동기화  
- 재해복구 데이터 복제

❌ 이 문제에서 부적절한 이유:
- 원본 파일이 온프레미스에 그대로 남음
- 스토리지 공간 확장 효과 없음
- 사용자는 여전히 온프레미스 파일에만 접근
```


**3.2 C번: FSx의 한계**

**FSx for Windows File Server 특성:**
```
✅ 장점:
- 완전 관리형 Windows 파일 시스템
- SMB 프로토콜 지원
- 온프레미스와 높은 호환성

❌ 이 문제에서 부적절한 이유:
- 모든 데이터가 고성능(고비용) 스토리지에 계속 보관
- 자동 계층화 기능 없음
- 향후 스토리지 비용 폭증
- 수명 주기 관리 불가능
```

**비용 비교:**
```
FSx for Windows (100TB):
- 월 비용: ~$30,000-50,000
- 모든 데이터가 SSD 성능 수준 유지 (불필요)

vs S3 File Gateway (동일 100TB):
- 월 비용: ~$1,320
- 접근 패턴에 따른 자동 최적화
```

**3.3 D번: 사용자 유틸리티의 문제점**

```
문제점들:
❌ 기존 SMB 기반 애플리케이션 호환성 깨짐
❌ 사용자 교육 비용 및 시간
❌ 각 컴퓨터별 개별 관리 복잡성
❌ 네트워크 지연으로 인한 성능 저하
❌ 파일 잠금, 권한 관리 등 고급 기능 손실
```
