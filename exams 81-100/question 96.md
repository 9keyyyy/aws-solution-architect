## Question #96
An Amazon EC2 administrator created the following policy associated with an IAM group containing several users:

```
Effect: Allow
Action: ec2:TerminateInstances
Resource: *
Condition: 
  IpAddress:
    aws:SourceIp: "10.100.100.0/24"
```
```
Effect: Deny  
Action: ec2:*
Resource: *
Condition:
  StringNotEquals:
    ec2:Region: "us-east-1"
```
What is the effect of this policy?

A. Users can terminate an EC2 instance in any AWS Region except us-east-1.

B. Users can terminate an EC2 instance with the IP address 10.100.100.1 in the us-east-1 Region.

C. Users can terminate an EC2 instance in the us-east-1 Region when the user's source IP is 10.100.100.254.

D. Users cannot terminate an EC2 instance in the us-east-1 Region when the user's source IP is 10.100.100.254.


## Question #96 분석

### IAM 정책 해석

**Statement 1 (Allow)**
```yaml
Effect: Allow
Action: ec2:TerminateInstances
Resource: *
Condition: 
  IpAddress:
    aws:SourceIp: "10.100.100.0/24"
```
허용 조건: 소스 IP가 10.100.100.0/24 대역에서 EC2 종료 가능

**Statement 2 (Deny)**
```yaml
Effect: Deny  
Action: ec2:*
Resource: *
Condition:
  StringNotEquals:
    ec2:Region: "us-east-1"
```
거부 조건: us-east-1이 아닌 모든 리전에서 EC2 작업 차단

### 정책 효과 분석

**조건 결합**
```yaml
허용: 10.100.100.0/24 대역에서 EC2 종료
제한: us-east-1 리전에서만 가능

결과: 소스 IP가 10.100.100.0/24 대역이고 
      us-east-1 리전에서만 EC2 종료 가능
```

### 선택지 검토

**A. us-east-1 제외한 모든 리전**
- 정책 효과와 반대 ❌

**B. 특정 IP 10.100.100.1만**
- 10.100.100.0/24 전체 대역 허용 (부분적) ❌

**C. 소스 IP 10.100.100.254에서 us-east-1 종료 가능**
- 10.100.100.254는 10.100.100.0/24 대역에 포함 ✅
- us-east-1 리전 조건 충족 ✅
- 정책 효과와 정확히 일치 ✅

**D. 10.100.100.254에서 us-east-1 종료 불가**
- 정책 효과와 반대 ❌

**정답: C**

정책은 10.100.100.0/24 대역(10.100.100.254 포함)에서 us-east-1 리전의 EC2 인스턴스 종료를 허용합니다.