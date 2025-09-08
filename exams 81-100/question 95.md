## Question #95
An application allows users at a company's headquarters to access product data. 
The product data is stored in an Amazon RDS MySQL DB instance. 
The operations team has isolated an application performance slowdown and wants to separate read traffic from write traffic. 
A solutions architect needs to optimize the application's performance quickly.

What should the solutions architect recommend?

A. Change the existing database to a Multi-AZ deployment. Serve the read requests from the primary Availability Zone.

B. Change the existing database to a Multi-AZ deployment. Serve the read requests from the secondary Availability Zone.

C. Create read replicas for the database. Configure the read replicas with half of the compute and storage resources as the source database.

D. Create read replicas for the database. Configure the read replicas with the same compute and storage resources as the source database.

## Question #95 분석

### ✅ 요구사항
- **성능 저하** 문제 발생
- **읽기/쓰기 트래픽 분리** 필요
- **빠른 성능 최적화** 요구

### ✅ 선택지 분석

**A. Multi-AZ + Primary에서 읽기**
- **분리 효과 없음**: 여전히 Primary에 모든 부하 ❌
- **성능 개선 없음**: 문제 해결 안됨 ❌

**B. Multi-AZ + Secondary에서 읽기**
- **기술적 불가능**: Multi-AZ Secondary는 읽기 불가 ❌
- **장애조치 전용**: Standby는 접근 차단 ❌

**C. Read Replica + 절반 리소스**
- **읽기 분리**: 트래픽 분산 효과 ✅
- **리소스 부족**: 절반 스펙으로 성능 제한 위험 ❌
- **병목 가능성**: 읽기 부하 집중 시 성능 저하 ❌

**D. Read Replica + 동일 리소스** ⭐
- **완전한 읽기 분리**: Primary는 쓰기 전용 ✅
- **충분한 성능**: 동일 스펙으로 안정적 처리 ✅
- **빠른 구현**: Read Replica 생성만으로 즉시 효과 ✅

### 📋 Multi-AZ vs Read Replica

```yaml
Multi-AZ:
목적: 가용성 (장애조치)
읽기: Primary만 가능
성능: 개선 없음

Read Replica:
목적: 읽기 성능 향상
읽기: Primary + Replica 분산
성능: 읽기 워크로드 분산으로 개선
```

### 💡 리소스 크기 고려

```yaml
C안 (절반 리소스):
위험: 읽기 부하 집중 시 병목
결과: 부분적 성능 개선

D안 (동일 리소스):
안정: 충분한 처리 능력 보장
결과: 완전한 성능 개선
```
