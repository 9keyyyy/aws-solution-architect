## Question #248
A company runs analytics software on Amazon EC2 instances. 
The software accepts job requests from users to process data that has been uploaded to Amazon S3. 
Users report that some submitted data is not being processed Amazon CloudWatch reveals that the EC2 instances have a consistent CPU utilization at or near 100%. 
The company wants to improve system performance and scale the system based on user load.

What should a solutions architect do to meet these requirements?

A. Create a copy of the instance. Place all instances behind an Application Load Balancer.

B. Create an S3 VPC endpoint for Amazon S3. Update the software to reference the endpoint.

C. Stop the EC2 instances. Modify the instance type to one with a more powerful CPU and more memory. Restart the instances.

D. Route incoming requests to Amazon Simple Queue Service (Amazon SQS). Configure an EC2 Auto Scaling group based on queue size. Update the software to read from the queue.

## Question #248 분석

### 요구사항
- **분석 소프트웨어** EC2에서 실행
- **작업 요청** 처리 (S3 데이터)
- **CPU 100%** 지속으로 일부 데이터 미처리
- **성능 개선** 및 **사용자 부하 기반 확장**

### 선택지 분석

**A. 인스턴스 복사 + ALB**
- **부분적 해결**: 로드 분산은 되지만 작업 분배 메커니즘 없음 ❌
- **작업 중복**: 여러 인스턴스가 같은 작업을 처리할 위험 ❌
- **확장성 부족**: 수동 확장으로 사용자 부하 대응 어려움 ❌

**B. S3 VPC Endpoint**
- **네트워크 최적화**: 약간의 성능 향상은 있지만 근본 해결 안됨 ❌
- **CPU 병목 미해결**: 100% CPU 사용률 문제 지속 ❌

**C. 더 강력한 인스턴스로 업그레이드**
- **수직 확장**: 일시적 성능 향상은 가능 ⚡
- **확장성 한계**: 사용자 부하 증가 시 동일 문제 재발 ❌
- **단일 장애점**: 여전히 하나의 인스턴스에 의존 ❌

**D. SQS + Auto Scaling + 큐 기반 처리** ⭐
- **작업 큐잉**: 모든 요청을 SQS에 안전하게 저장 ✅
- **자동 확장**: 큐 크기 기반으로 EC2 인스턴스 자동 조정 ✅
- **작업 분배**: 여러 인스턴스가 큐에서 작업을 가져와 병렬 처리 ✅
- **데이터 손실 방지**: SQS의 메시지 지속성으로 처리 보장 ✅

### SQS 기반 아키텍처

```yaml
개선된 흐름:
1. 사용자 요청 → SQS 큐 (즉시 응답)
2. Auto Scaling Group이 큐 크기 모니터링
3. 큐가 길어지면 EC2 인스턴스 자동 추가
4. 각 인스턴스가 큐에서 작업을 폴링하여 처리
5. 작업 완료 후 큐에서 메시지 삭제

장점:
- 피크 시간: 자동으로 인스턴스 증가
- 한가한 시간: 자동으로 인스턴스 감소
- 작업 손실 없음: SQS 메시지 지속성
```
