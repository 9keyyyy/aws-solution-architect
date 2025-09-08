## Question #215
A company has 700 TB of backup data stored in network attached storage (NAS) in its data center. 
This backup data need to be accessible for infrequent regulatory requests and must be retained 7 years. 
The company has decided to migrate this backup data from its data center to AWS. 
The migration must be complete within 1 month. The company has 500 Mbps of dedicated bandwidth on its public internet connection available for data transfer.

What should a solutions architect do to migrate and store the data at the LOWEST cost?

A. Order AWS Snowball devices to transfer the data. Use a lifecycle policy to transition the files to Amazon S3 Glacier Deep Archive.

B. Deploy a VPN connection between the data center and Amazon VPC. Use the AWS CLI to copy the data from on premises to Amazon S3 Glacier.

C. Provision a 500 Mbps AWS Direct Connect connection and transfer the data to Amazon S3. Use a lifecycle policy to transition the files to Amazon S3 Glacier Deep Archive.

D. Use AWS DataSync to transfer the data and deploy a DataSync agent on premises. Use the DataSync task to copy files from the on-premises NAS storage to Amazon S3 Glacier.

## Question #215 분석

### 요구사항
- **700TB 백업 데이터** 마이그레이션
- **1개월 내** 완료 필요
- **500Mbps** 인터넷 대역폭
- **7년 보관**, 드문 접근
- **최저 비용**

### 전송 시간 계산
```yaml
인터넷 전송 (500Mbps):
700TB = 5,600,000 Gbits
500Mbps = 0.5 Gbps
전송 시간 = 5,600,000 ÷ 0.5 = 11,200,000초
= 약 130일 (4개월 이상)

결론: 인터넷 전송으로는 1개월 내 불가능
```

### 선택지 분석

**A. AWS Snowball + Glacier Deep Archive** ⭐
- **전송 시간**: Snowball로 1-2주 내 가능 ✅
- **최저 비용**: Deep Archive 가장 저렴한 장기 보관 ✅
- **드문 접근**: 12-48시간 복구 시간 적합 ✅
- **1개월 요구사항**: 충분히 달성 가능 ✅

**B. VPN + S3 Glacier**
- **전송 시간**: 여전히 130일 필요 ❌
- **1개월 불가능**: 시간 요구사항 미충족 ❌

**C. Direct Connect + S3 Glacier**
- **전송 시간**: 여전히 130일 (500Mbps 동일) ❌
- **높은 비용**: Direct Connect 초기 비용 + 월 비용 ❌
- **1개월 불가능**: 시간 요구사항 미충족 ❌

**D. DataSync + S3 Glacier**
- **Glacier 직접 전송 불가**: DataSync는 Glacier 직접 지원 안함 ❌
- **전송 시간**: 여전히 인터넷 대역폭 제한 ❌

### Snowball vs 인터넷 비교

```yaml
AWS Snowball (100TB 디바이스):
- 필요 대수: 7개
- 배송 시간: 각 1주
- 총 시간: 2-3주
- 비용: ~$1,400

인터넷 전송:
- 시간: 130일
- 대역폭 비용: 월 수천 달러
- 1개월 내 불가능
```

### 저장 비용 비교

```yaml
S3 Glacier Deep Archive: $0.00099/GB/월
S3 Glacier: $0.004/GB/월
S3 Standard: $0.023/GB/월

700TB × 7년 Deep Archive: 약 $5,900
700TB × 7년 Glacier: 약 $23,500

Deep Archive가 75% 저렴
```