## Question #30
A company that hosts its web application on AWS wants to ensure all Amazon EC2 instances. 
Amazon RDS DB instances. 
and Amazon Redshift clusters are configured with tags. 
The company wants to minimize the effort of configuring and operating this check.

What should a solutions architect do to accomplish this?

A. Use AWS Config rules to define and detect resources that are not properly tagged.

B. Use Cost Explorer to display resources that are not properly tagged. Tag those resources manually.

C. Write API calls to check all resources for proper tag allocation. Periodically run the code on an EC2 instance.

D. Write API calls to check all resources for proper tag allocation. Schedule an AWS Lambda function through Amazon CloudWatch to periodically run the code.

## Question #30 분석

### ✅ 요구사항
- **모든 EC2, RDS, Redshift** 리소스의 태그 구성 확인
- **구성 및 운영 노력 최소화** 필요
- 자동화된 태그 규정 준수 확인

### ✅ 선택지 분석

**A. AWS Config 규칙 사용** ⭐
- **자동화된 규정 준수**: Config가 지속적으로 모니터링 ✅
- **사전 구축된 규칙**: 태그 관련 규칙 기본 제공 ✅
- **최소 운영 노력**: 설정 후 자동 운영 ✅
- **다중 리소스 지원**: EC2, RDS, Redshift 모두 지원 ✅

**B. Cost Explorer 수동 태깅**
- **수동 프로세스**: 지속적인 수동 작업 필요 ❌
- **운영 노력 최대화**: 최소화 요구사항 위배 ❌
- Cost Explorer는 분석 도구, 규정 준수 도구 아님

**C. EC2에서 API 호출**
- **커스텀 개발**: 코드 작성 및 유지보수 필요 ❌
- **EC2 관리**: 인스턴스 운영 오버헤드 ❌
- **높은 운영 부담**: 최소화 요구사항 위배

**D. Lambda + CloudWatch 스케줄링**
- **커스텀 개발**: 코드 작성 필요 ❌
- **서버리스 장점**: EC2보다 나음
- **여전히 운영 부담**: Config 규칙보다 복잡
