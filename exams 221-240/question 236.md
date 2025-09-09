## Question #236
A company has a three-tier application for image sharing. 
The application uses an Amazon EC2 instance for the front-end layer, another EC2 instance for the application layer, and a third EC2 instance for a MySQL database.
A solutions architect must design a scalable and highly available solution that requires the least amount of change to the application.

Which solution meets these requirements?

A. Use Amazon S3 to host the front-end layer. Use AWS Lambda functions for the application layer. Move the database to an Amazon DynamoDB table. Use Amazon S3 to store and serve users’ images.

B. Use load-balanced Multi-AZ AWS Elastic Beanstalk environments for the front-end layer and the application layer. Move the database to an Amazon RDS DB instance with multiple read replicas to serve users’ images.

C. Use Amazon S3 to host the front-end layer. Use a fleet of EC2 instances in an Auto Scaling group for the application layer. Move the database to a memory optimized instance type to store and serve users’ images.

D. Use load-balanced Multi-AZ AWS Elastic Beanstalk environments for the front-end layer and the application layer. Move the database to an Amazon RDS Multi-AZ DB instance. Use Amazon S3 to store and serve users’ images.

## Question #236 분석

### 요구사항
- **3계층 이미지 공유 애플리케이션**
- **확장성 및 고가용성** 필요
- **애플리케이션 변경 최소화**

### 선택지 분석

**A. S3 + Lambda + DynamoDB**
- **대규모 변경**: Lambda 서버리스로 완전 재작성 필요 ❌
- **MySQL → DynamoDB**: 데이터베이스 변경으로 쿼리 로직 재작성 ❌
- **변경 최소화 위반**: 요구사항과 정반대 ❌

**B. Elastic Beanstalk + RDS 읽기 전용 복제본**
- **부적절한 읽기 복제본**: 이미지 서빙용으로는 부적합 ❌
- **스토리지 용도 오해**: 데이터베이스로 이미지 저장 비효율 ❌

**C. S3 프론트엔드 + Auto Scaling + 메모리 최적화 DB**
- **인스턴스 변경**: 메모리 최적화로도 이미지 저장 비효율 ❌
- **부분적 해결**: 고가용성 측면에서 DB 단일 장애점 ❌

**D. Elastic Beanstalk + RDS Multi-AZ + S3 이미지 저장** ⭐
- **최소 변경**: 기존 코드를 Beanstalk에 거의 그대로 배포 ✅
- **확장성**: Auto Scaling으로 자동 확장 ✅
- **고가용성**: Multi-AZ 배포로 AZ 장애 대응 ✅
- **적절한 아키텍처**: DB는 메타데이터, S3는 이미지 파일 ✅

### Elastic Beanstalk 장점

```yaml
애플리케이션 변경 최소화:
- 기존 코드 업로드만으로 배포
- EC2 기반이므로 호환성 높음
- 로드 밸런싱 자동 구성
- Multi-AZ 배포 자동 처리

완전 관리형:
- 인프라 자동 프로비저닝
- Auto Scaling 자동 설정
- 모니터링 및 헬스체크
- 플랫폼 업데이트 자동
```
