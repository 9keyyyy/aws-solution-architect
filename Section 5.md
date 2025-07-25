### Section 5: EC2 기초
**Amazon EC2**

- EC2 is one of the most popular of AWS’ offering
- EC2 = Elastic Compute Cloud = Infrastructure as a Service
- It mainly consists in the capability of :<br>
• Renting virtual machines (EC2)<br>
• Storing data on virtual drives (EBS)<br>
• Distributing load across machines (ELB)<br>
• Scaling the services using an auto-scaling group (ASG)<br>
- Knowing EC2 is fundamental to understand how the Cloud works

**EC2 sizing & configuration options**

- Operating System (OS): Linux, Windows or Mac OS
- How much compute power & cores (CPU)
- How much random-access memory (RAM)
- How much storage space: <br>
• Network-attached (EBS & EFS) <br>
• hardware (EC2 Instance Store)<br>
- Network card: speed of the card, Public IP address
- Firewall rules: security group
- Bootstrap script (configure at first launch): EC2 User Data

**EC2 User Data**

- It is possible to bootstrap our instances using an EC2 User data script
- bootstrapping means launching commands when a machine starts
- That script is only run once at the instance first start
- EC2 user data is used to automate boot tasks such as:<br>
• Installing updates<br>
• Installing software<br>
• Downloading common files from the internet<br>
• Anything you can think of<br>
- The EC2 User Data Script runs with the root user

**EC2 Instance Types – General Purpose**

- Great for a diversity of workloads such as web servers or code repositories
- Balance between:<br>
• Compute<br>
• Memory<br>
• Networking<br>

**EC2 Instance Types – Compute Optimized**

- Great for compute-intensive tasks that require high performance processors:<br>
• Batch processing workloads<br>
• Media transcoding<br>
• High performance web servers<br>
• High performance computing (HPC)<br>
• Scientific modeling & machine learning<br>
• Dedicated gaming servers<br>

**EC2 Instance Types – Memory Optimized**

- Fast performance for workloads that process large data sets in memory
- Use cases:<br>
• High performance, relational/non-relational databases<br>
• Distributed web scale cache stores<br>
• In-memory databases optimized for BI (business intelligence)<br>
• Applications performing real-time processing of big unstructured data<br>

**EC2 Instance Types – Storage Optimized**

- Great for storage-intensive tasks that require high, sequential read and write access to large data sets on local storage
- Use cases:<br>
• High frequency online transaction processing (OLTP) systems<br>
• Relational & NoSQL databases<br>
• Cache for in-memory databases (for example, Redis)<br>
• Data warehousing applications<br>
• Distributed file systems<br>

**Introduction to Security Groups**

- Security Groups are the fundamental of network security in AWS
- They control how traffic is allowed into or out of our EC2 Instances.
- Security groups only contain rules
- Security groups rules can reference by IP or by security group


**Security Groups Good to know**

- Can be attached to multiple instances
- Locked down to a region / VPC combination
- Does live “outside” the EC2 – if traffic is blocked the EC2 instance won’t see it
- It’s good to maintain one separate security group for SSH access
- If your application is not accessible (time out), then it’s a security group issue
- If your application gives a “connection refused“ error, then it’s an application
error or it’s not launched
- All **inbound traffic is blocked by default**
- All **outbound traffic is authorised by default**

**EC2 Instances Purchasing Options**

- On-Demand Instances – short workload, predictable pricing, pay by second
- Reserved (1 & 3 years) <br>
• Reserved Instances – long workloads<br>
• Convertible Reserved Instances – long workloads with flexible instances<br>
- Savings Plans (1 & 3 years) –commitment to an amount of usage, long workload
- Spot Instances – short workloads, cheap, can lose instances (less reliable)
- Dedicated Hosts – book an entire physical server, control instance placement
- Dedicated Instances – no other customers will share your hardware
- Capacity Reservations – reserve capacity in a specific AZ for any duration

**EC2 On Demand**

- Has the highest cost but no upfront payment
- No long-term commitment
- Recommended for short-term and un-interrupted workloads, where you can't predict how the application will behave

**EC2 Reserved Instances**

- Up to 72% discount compared to On-demand
- You reserve a specific instance attributes (Instance Type, Region, Tenancy, OS)
- Reservation Period – 1 year (+discount) or 3 years (+++discount)
- Payment Options – No Upfront (+), Partial Upfront (++), All Upfront (+++)
- Reserved Instance’s Scope – Regional or Zonal (reserve capacity in an AZ)
- Recommended for steady-state usage applications (think database)
- You can buy and sell in the Reserved Instance Marketplace

**EC2 Savings Plans**

- Get a discount based on long-term usage (up to 72% - same as RIs)
- Commit to a certain type of usage ($10/hour for 1 or 3 years)
- Usage beyond EC2 Savings Plans is billed at the On-Demand price

**EC2 Spot Instances**

- Can get a discount of up to 90% compared to On-demand
- Instances that you can “lose” at any point of time if your max price is less than the
current spot price
- The MOST cost-efficient instances in AWS
- Useful for workloads that are resilient to failure<br>
• Batch jobs<br>
• Data analysis<br>
• Image processing<br>
• Any distributed workloads<br>
• Workloads with a flexible start and end time<br>
- Not suitable for critical jobs or databases

**EC2 Dedicated Hosts**

- A physical server with EC2 instance capacity fully dedicated to your use
- Allows you address compliance requirements and use your existing server- bound software licenses (per-socket, per-core, pe—VM software licenses)
- Purchasing Options:<br>
• On-demand – pay per second for active Dedicated Host<br>
• Reserved - 1 or 3 years (No Upfront, Partial Upfront, All Upfront)<br>
- The most expensive option
- Useful for software that have complicated licensing model (BYOL – Bring Your Own License)
- Or for companies that have strong regulatory or compliance needs

**EC2 Dedicated Instances**

- Instances run on hardware that’s dedicated to you
- May share hardware with other instances in same account
- No control over instance placement

### 퀴즈
#### Question 1:
다음 중, 할인 폭이 가장 크나, 데이터베이스 혹은 중요 업무에는 적합하지 않은 EC2 구매 옵션은 무엇인가요?

[답] 스팟 인스턴스

- 스팟 인스턴스는 단기적인 워크로드에 적합
- 가장 저렴한 EC2 구매옵션
- 인스턴스 손실 우려가 있어 신뢰도는 떨어짐

#### Question 2:
EC2 인스턴스 내/외의 트래픽을 제어하기 위해서는 무엇을 사용해야 하나요?

[답] 보안그룹

#### Question 3:
EC2 예약 인스턴스를 예약할 수 있는 기간은 얼마인가요?

[답] 1년, 혹은 3년

#### Question 4:
EC2 인스턴스에 고성능 컴퓨팅(HPC) 애플리케이션을 배포하려 합니다. 다음 중 어떤 EC2 인스턴스 유형을 선택해야 할까요?

[답] 컴퓨팅 최적화

- 컴퓨팅 최적화 EC2 인스턴스는 고성능 프로세서가 필요한 집중 컴퓨팅 워크로드에 적합
- 예) 배치 처리, 미디어 트랜스코딩, 머신러닝, 전용 게이밍 서버

#### Question 5:
1년 간 지속적으로 서버를 운영할 계획인 애플리케이션의 경우에는 다음 중 어떤 EC2 구매 옵션을 사용해야 할까요?

[답] 예약 인스턴스

- 예약 인스턴스는 장기적인 워크로드에 적합
- 1년, 혹은 3년 기간으로 예약 가능

#### Question 6:
일련의 EC2 인스턴스에 호스팅 될 애플리케이션을 실행하려 합니다. 이 애플리케이션에는 소프트웨어 설치가 필요하며, 최초 실행 중에 일부 OS 패키지를 업데이트해야 합니다. EC2 인스턴스를 실행하려는 경우, 이를 위한 최적의 방식은 무엇일까요?

[답] 필수 소프트웨어의 설치 및 OS 업데이트를 수행하는 bash 스크립트를 작성한 후, 이 스크립트를 EC2 인스턴스 실행 시 사용자 데이터에서 사용하기


####  Question 7:
인메모리 데이터베이스를 사용하는 중요 애플리케이션을 위해서는 다음 중 어떤 EC2 인스턴스 유형을 선택해야 할까요?

[답] 메모리 최적화


#### Question 8:
온프레미스에 호스팅된 OLTP 데이터베이스를 갖춘 전자 상거래 애플리케이션이 있습니다. 이 애플리케이션은 인기가 좋아, 데이터베이스가 초당 수천 개의 요청을 지니게 됩니다. 여러분은 데이터베이스를 EC2 인스턴스로 이전하려 합니다. 이렇게 높은 빈도를 보이는 OLTP 데이터베이스를 처리하기 위해서는 어떤 EC2 인스턴스 유형을 선택해야 할까요?

[답] 스토리지 최적화

#### Question 9:
(참/거짓) 보안 그룹은 오직 하나의 EC2 인스턴스에만 연결될 수 있습니다.

[답] 거짓

#### Question 10:
온프레미스 애플리케이션을 AWS로 이전하려 합니다. 여러분의 기업에는 애플리케이션을 전용 서버에서 실행해야 한다는 엄격한 규정이 있습니다. 또한 비용 절감을 위해 전용 서버 바운드 소프트웨어 라이선스를 사용해야 합니다. 이 경우, 다음 중 어떤 EC2 구매 옵션이 적합할까요?

[답] 전용 호스트

- 전용 호스트는 높은 수준의 규정 준수가 필요한 기업, 혹은 복잡한 라이선스 모델을 가진 소프트웨어에 적합
- 가장 비싼 EC2 구매 옵션


#### Question 11:
데이터베이스 기술을 EC2 인스턴스에 배포하려 합니다. 공급 업체 라이선스는 물리적 코어 및 기반 네크워크 소켓 가시성를 기반으로 비용을 책정합니다. 이 경우, 어떤 EC2 구매 옵션을 사용해야 가시성을 확보할 수 있을까요?

[답] 전용 호스트

#### Question 12:
스팟 플릿은 스팟 인스턴스의 집합이며, 선택적으로 ___________ 이다.

[답] 온디맨드 인스턴스

- 스팟 플릿은 스팟 인스턴스의 집합이며, 선택적으로 온디맨드 인스턴스
- 스팟 플릿은 가장 낮은 가격으로 스팟 인스턴스를 자동으로 요청할 수 있게 해줌





