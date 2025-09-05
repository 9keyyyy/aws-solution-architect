## Question #43
A company has an on-premises application that generates a large amount of time-sensitive data that is backed up to Amazon S3. 
The application has grown and there are user complaints about internet bandwidth limitations. 
A solutions architect needs to design a long-term solution that allows for both timely backups to Amazon S3 and with minimal impact on internet connectivity for internal users.

Which solution meets these requirements?

A. Establish AWS VPN connections and proxy all traffic through a VPC gateway endpoint.

B. Establish a new AWS Direct Connect connection and direct backup traffic through this new connection.

C. Order daily AWS Snowball devices. Load the data onto the Snowball devices and return the devices to AWS each day.

D. Submit a support ticket through the AWS Management Console. Request the removal of S3 service limits from the account.

## Question #43 분석

### ✅ 요구사항
- **온프레미스 애플리케이션**에서 **대용량 시간 민감 데이터** 생성
- **S3 백업** 필요
- **인터넷 대역폭 제한**으로 사용자 불만
- **장기적 솔루션** 필요
- **적시 백업** + **내부 사용자 인터넷 영향 최소화**

### ✅ 선택지 분석

**A. AWS VPN + VPC Gateway Endpoint**
- **VPN 한계**: 여전히 인터넷 기반 연결 
- **대역폭 제한**: 인터넷 대역폭 공유 문제 미해결 
- **VPN 성능**: 암호화 오버헤드로 속도 저하 
- 근본적 문제 해결 안됨

**B. AWS Direct Connect** ⭐
- **전용 네트워크**: 인터넷과 분리된 전용 회선 ✅
- **높은 대역폭**: 1Gbps-100Gbps 전용 대역폭 ✅
- **일관된 성능**: 네트워크 지연시간 예측 가능 ✅
- **인터넷 영향 없음**: 백업 트래픽 완전 분리 ✅
- **장기적 솔루션**: 확장 가능한 영구 해결책 ✅

**C. 일일 Snowball 배송**
- **물리적 배송 지연**: 일일 배송으로 적시성 부족 
- **운영 복잡성**: 매일 디바이스 관리 
- **높은 비용**: 일일 배송비 + 디바이스 비용 
- **확장성 제한**: 디바이스 용량 한계 

**D. S3 서비스 제한 제거 요청**
- **제한 오해**: S3 자체 제한이 문제가 아님 
- **대역폭 문제 미해결**: 인터넷 연결 병목 그대로 
- **잘못된 접근**: 문제 원인 오진단 

### 📋 Direct Connect의 네트워크 분리

#### **현재 문제 상황**
```yaml
온프레미스 네트워크:
  애플리케이션 백업 + 사용자 인터넷 = 동일 인터넷 회선 공유

대역폭 경합:
  인터넷 회선 (예: 1Gbps)
  ├── 백업 트래픽: 800Mbps (80% 점유)
  └── 사용자 트래픽: 200Mbps (느린 인터넷)

결과:
  ❌ 사용자 불만 (느린 웹 브라우징, 이메일)
  ❌ 백업 성능 저하 (대용량 데이터 처리 지연)
  ❌ 비즈니스 영향 (업무 효율성 감소)
```

#### **Direct Connect 해결책**
```yaml
네트워크 분리:
  온프레미스
  ├── 인터넷 회선: 사용자 트래픽 전용 (1Gbps)
  └── Direct Connect: 백업 트래픽 전용 (1Gbps-100Gbps)

트래픽 분리:
  사용자: 인터넷 → 웹사이트, 이메일, SaaS
  백업: Direct Connect → AWS S3

결과:
  ✅ 사용자 인터넷 속도 복원 (전용 대역폭)
  ✅ 백업 성능 대폭 향상 (전용 고속 회선)
  ✅ 네트워크 경합 완전 해소
```

### 🚀 Direct Connect 성능 및 안정성

#### **대역폭 옵션**
```yaml
전용 연결:
  - 1Gbps: $0.30 per hour + 포트 비용
  - 10Gbps: $2.25 per hour + 포트 비용
  - 100Gbps: $24.50 per hour + 포트 비용

호스팅 연결 (서브 1Gbps):
  - 50Mbps, 100Mbps, 200Mbps, 300Mbps, 400Mbps, 500Mbps
  - 더 저렴하지만 공유 연결

선택 기준:
  - 백업 데이터 볼륨
  - 성장 예상률
  - 예산 고려사항
```

#### **성능 특성**
```yaml
지연시간:
  - 인터넷: 15-100ms (가변적)
  - Direct Connect: 1-10ms (일관된 낮은 지연)

처리량:
  - 인터넷: 베스트 에포트 (변동적)
  - Direct Connect: 보장된 대역폭 (일관된 성능)

안정성:
  - 인터넷: 공용 네트워크 영향
  - Direct Connect: 전용 회선 (안정적)
```
