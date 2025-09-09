## Question #231
An application runs on an Amazon EC2 instance that has an Elastic IP address in VPC A. 
The application requires access to a database in VPC B. Both VPCs are in the same AWS account.

Which solution will provide the required access MOST securely?

A. Create a DB instance security group that allows all traffic from the public IP address of the application server in VPC A.

B. Configure a VPC peering connection between VPC A and VPC B.

C. Make the DB instance publicly accessible. Assign a public IP address to the DB instance.

D. Launch an EC2 instance with an Elastic IP address into VPC

B. Proxy all requests through the new EC2 instance.

## Question #231 분석

### 요구사항
- **VPC A의 EC2** (Elastic IP)에서 **VPC B의 데이터베이스** 접근
- **동일 AWS 계정** 내 VPC 간 통신
- **최고 보안 수준** 접근

### 선택지 분석

**A. DB 보안 그룹에 퍼블릭 IP 허용**
- **보안 위험**: 퍼블릭 IP는 변경 가능하고 스푸핑 위험 ❌
- **과도한 노출**: 인터넷을 통한 DB 접근 경로 생성 ❌
- **제어 부족**: 네트워크 레벨 격리 없음 ❌

**B. VPC Peering 연결 구성** ⭐
- **프라이빗 통신**: VPC 간 직접 프라이빗 네트워크 연결 ✅
- **인터넷 우회**: 트래픽이 AWS 백본 네트워크만 사용 ✅
- **세밀한 제어**: 라우팅 테이블과 보안 그룹으로 정확한 제어 ✅
- **최고 보안**: 완전히 격리된 프라이빗 통신 ✅

**C. DB 퍼블릭 접근 + 퍼블릭 IP**
- **심각한 보안 위험**: DB를 인터넷에 노출 ❌
- **공격 벡터 증가**: 전 세계에서 접근 가능 ❌
- **보안 모범 사례 위반**: DB는 절대 퍼블릭하면 안됨 ❌

**D. VPC B에 프록시 EC2 배치**
- **불필요한 복잡성**: 추가 인스턴스 관리 부담 ❌
- **단일 장애점**: 프록시 인스턴스 장애 위험 ❌
- **성능 저하**: 불필요한 홉 추가 ❌

### VPC Peering 보안 장점

```yaml
네트워크 격리:
- 완전한 프라이빗 연결
- AWS 백본 네트워크만 사용
- 인터넷 완전 우회

정확한 제어:
- 라우팅 테이블로 트래픽 경로 제어
- 보안 그룹으로 포트/프로토콜 제한
- NACLs로 추가 보안 계층

감사 및 모니터링:
- VPC Flow Logs로 트래픽 추적
- CloudTrail로 설정 변경 기록
- CloudWatch로 연결 상태 모니터링
```
