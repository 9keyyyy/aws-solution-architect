## Question #242
A company hosts its web application on AWS using seven Amazon EC2 instances. 
The company requires that the IP addresses of all healthy EC2 instances be returned in response to DNS queries.

Which policy should be used to meet this requirement?

A. Simple routing policy

B. Latency routing policy

C. Multivalue routing policy

D. Geolocation routing policy

## Question #242 분석

### 요구사항
- **7개 EC2 인스턴스** 웹 애플리케이션 호스팅
- **모든 건강한 인스턴스 IP**를 DNS 쿼리 응답에 포함
- **Route 53 라우팅 정책** 선택

### 선택지 분석

**A. Simple 라우팅 정책**
- **단순 반환**: 여러 IP를 반환하지만 헬스체크 없음 ❌
- **건강 상태 무시**: 비정상 인스턴스도 포함하여 반환 ❌
- **요구사항 미충족**: "건강한 인스턴스만" 조건 불충족 ❌

**B. Latency 라우팅 정책**
- **지연시간 기반**: 가장 낮은 지연시간 하나만 반환 ❌
- **단일 응답**: 여러 IP 동시 반환 안함 ❌
- **요구사항 불일치**: "모든 IP" 조건 불충족 ❌

**C. Multivalue 라우팅 정책** ⭐
- **다중 값 반환**: 여러 IP 주소를 동시에 반환 ✅
- **헬스체크 지원**: 건강한 인스턴스만 응답에 포함 ✅
- **요구사항 완벽 충족**: 모든 건강한 EC2 IP 반환 ✅
- **로드 분산 효과**: 클라이언트가 여러 IP 중 선택 ✅

**D. Geolocation 라우팅 정책**
- **지리적 기반**: 사용자 위치에 따라 특정 IP만 반환 ❌
- **단일 응답**: 여러 IP 동시 반환 안함 ❌
- **요구사항 불일치**: "모든 IP" 조건 불충족 ❌

### Multivalue 라우팅 정책

```yaml
작동 방식:
- 최대 8개 IP 주소 동시 반환
- 각 레코드별 헬스체크 설정 가능
- 비정상 인스턴스 자동 제외
- 클라이언트가 여러 옵션 중 선택

예시 응답:
DNS Query: example.com
Response: 
- 1.2.3.4 (healthy)
- 1.2.3.5 (healthy)
- 1.2.3.6 (healthy)
- 1.2.3.7 (healthy)
```
