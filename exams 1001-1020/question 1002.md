## Question #1002
An online gaming company is transitioning user data storage to Amazon DynamoDB to support the company's growing user base. The current architecture includes DynamoDB tables that contain user profiles, achievements, and in-game transactions.

The company needs to design a robust, continuously available, and resilient DynamoDB architecture to maintain a seamless gaming experience for users.

Which solution will meet these requirements MOST cost-effectively?

A. Create DynamoDB tables in a single AWS Region. Use on-demand capacity mode. Use global tables to replicate data across multiple Regions.

B. Use DynamoDB Accelerator (DAX) to cache frequently accessed data. Deploy tables in a single AWS Region and enable auto scaling. Configure Cross-Region Replication manually to additional Regions.

C. Create DynamoDB tables in multiple AWS Regions. Use on-demand capacity mode. Use DynamoDB Streams for Cross-Region Replication between Regions.

D. Use DynamoDB global tables for automatic multi-Region replication. Deploy tables in multiple AWS Regions. Use provisioned capacity mode. Enable auto scaling.

## Question #1002 분석

### 문제 상황
- **온라인 게임** 사용자 데이터 → DynamoDB 전환
- **증가하는 사용자 기반**
- **견고하고 지속적으로 사용 가능한** 아키텍처 필요
- **비용 효율성** 중요

### 핵심 요구사항
- **고가용성 및 복원력**
- **끊김 없는 게임 경험**
- **비용 효율성**
- **확장성**

### 선택지 분석

**A. 단일 리전 + On-demand + Global Tables**
- 모순: 단일 리전인데 Global Tables? ❌
- Global Tables는 다중 리전 전제 ❌

**B. DAX + 단일 리전 + 수동 복제**
- 단일 리전 → 장애시 전체 다운 ❌
- 수동 복제 → 관리 부담 + 복잡성 ❌

**C. 다중 리전 + On-demand + DynamoDB Streams**
- DynamoDB Streams: 수동 복제 구현 → 복잡 ❌
- Global Tables 대신 수동 구현 → 비효율 ❌

**D. Global Tables + 다중 리전 + Provisioned + Auto Scaling** ⭐
- Global Tables: 자동 다중 리전 복제 ✅
- 고가용성 보장 ✅
- Provisioned + Auto Scaling: 비용 최적화 ✅
- 완전 관리형 ✅

### 비용 효율성 비교

**On-demand vs Provisioned + Auto Scaling**
```yaml
On-demand:
- 예측 불가능한 트래픽에 적합
- 요청당 과금
- 게임은 예측 가능한 패턴 → 비효율

Provisioned + Auto Scaling:
- 예측 가능한 워크로드에 비용 효율적
- 기본 용량 + 필요시 확장
- 게임 트래픽 패턴에 최적
```

### DynamoDB Global Tables

```yaml
장점:
- 완전 관리형 다중 리전 복제
- 자동 장애 조치
- 읽기/쓰기 성능 향상
- 지역별 낮은 지연 시간
```
