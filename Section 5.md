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
