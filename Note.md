## Security
### Security Group
Security Group 연결 가능 = VPC 내부 리소스
```
EC2, RDS, ALB/NLB, EFS, Lambda(VPC)
네트워크 인터페이스(ENI)가 있는 서비스
```
Security Group 연결 불가능 = VPC 외부 관리형 서비스
```
API Gateway, CloudFront, S3, DynamoDB
대안: Resource Policy, WAF, IAM Policy
```
<br>

### IAM
**서비스별 특화 방식**
- **EC2**: Instance Profile (IAM Role 연결)
- **Lambda**: Execution Role
- **EKS**: IRSA (IAM Role for Service Accounts)
- **ECS**: Task Role

