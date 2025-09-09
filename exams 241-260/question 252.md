## Question #252
A solutions architect needs to design a system to store client case files. 
The files are core company assets and are important. 
The number of files will grow over time.

The files must be simultaneously accessible from multiple application servers that run on Amazon EC2 instances. 
The solution must have built-in redundancy.

Which solution meets these requirements?

A. Amazon Elastic File System (Amazon EFS)

B. Amazon Elastic Block Store (Amazon EBS)

C. Amazon S3 Glacier Deep Archive

D. AWS Backup

## Question #252 분석

### 요구사항
- **클라이언트 케이스 파일** 저장
- **핵심 회사 자산** (중요도 높음)
- **파일 수 지속 증가**
- **여러 EC2에서 동시 접근** 필요
- **내장 이중화** 필요

### 선택지 분석

**A. Amazon Elastic File System (EFS)** ⭐
- **동시 접근**: 여러 EC2에서 NFS를 통해 동시 마운트 가능 ✅
- **확장성**: 파일 수 증가에 따라 자동 확장 ✅
- **내장 이중화**: 여러 AZ에 자동 복제 및 이중화 ✅
- **고가용성**: 99.999999999% 내구성 보장 ✅

**B. Amazon Elastic Block Store (EBS)**
- **단일 연결**: 한 번에 하나의 EC2만 연결 가능 ❌
- **동시 접근 불가**: Multi-Attach는 제한적이고 복잡 ❌
- **공유 스토리지 부적합**: 여러 서버 간 파일 공유 불가 ❌

**C. Amazon S3 Glacier Deep Archive**
- **아카이브 전용**: 장기 보관용, 활성 접근에 부적합 ❌
- **복구 시간**: 12-48시간 소요로 즉시 접근 불가 ❌
- **애플리케이션 통합**: EC2에서 직접 파일 시스템으로 사용 불가 ❌

**D. AWS Backup**
- **백업 서비스**: 데이터 저장소가 아닌 백업 솔루션 ❌
- **주 스토리지 아님**: 실제 파일 저장 및 접근용이 아님 ❌

### EFS 특징

```yaml
파일 시스템 특성:
- NFS 4.0/4.1 프로토콜
- POSIX 호환
- 수천 개 EC2 동시 연결 가능
- 자동 확장 (페타바이트까지)

이중화 및 가용성:
- 여러 AZ에 자동 복제
- 지역 내 모든 AZ에서 접근 가능
- 99.999999999% 내구성
- 자동 백업 지원
```
