### Section 3. Getting Started with AWS

**AWS Region 선택에 영향을 주는 요인?**

- Compliance (규정 준수) - 데이터 보관
- Proximity - reduced latency
- Available services - 모든 리전에 새로운 서비스/피처 제공 X
- Pricing - 리전마다 가격이 다름

**AWS Availability Zones**

- 각 리전은 여러 가용한 zone들을 가지고 있음 (3-6개)
- 각 AZ에는 1개 이상의 데이터 센터가 있음
- AZ는 서로 분리되어있고, 재해로부터 영향을 덜 받도록 설계
- 각 AZ는 초저지연 네트워킹으로 연결 + 모두 연결되어 하나의 Region

**AWS Points of Presence**

- 90개 도시에서 400개 이상의 Points Of Presence를 운영
- 최저 지연 시간으로 사용자에게 전달

**Tour of the AWS Console**

- AWS has Global Services:<br>
• Identity and Access Management (IAM) <br>
• Route 53 (DNS service)<br>
• CloudFront (Content Delivery Network)<br>
• WAF (Web Application Firewall)<br>
- Most AWS services are Region-scoped:<br>
• Amazon EC2 (Infrastructure as a Service)<br>
• Elastic Beanstalk (Platform as a Service)<br>
• Lambda (Function as a Service)<br>
• Rekognition (Software as a Service)<br>
- Region Table: https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services
