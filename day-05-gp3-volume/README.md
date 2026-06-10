\# 📅 Day 5: Create GP3 EBS Volume



\---



\# 🧠 Task



Create an AWS EBS volume with the following specifications:



\* Name: `devops-volume`

\* Volume Type: `gp3`

\* Size: `2 GiB`

\* Region: `us-east-1`



\---



\# 🎯 Objective



\* Understand Amazon EBS volumes

\* Learn different EBS volume types

\* Create and manage storage volumes in AWS

\* Understand performance characteristics of EBS storage

\* Practice AWS Console and CLI operations



\---



\# ☁️ AWS Details



| Property    | Value         |

| ----------- | ------------- |

| Service     | Amazon EBS    |

| Region      | us-east-1     |

| Volume Name | devops-volume |

| Volume Type | gp3           |

| Size        | 2 GiB         |



\---



\# 🧠 What is Amazon EBS?



Amazon Elastic Block Store (EBS) is a block-level storage service designed for use with Amazon EC2 instances.



EBS volumes:



\* Act like virtual hard disks

\* Can be attached to EC2 instances

\* Store operating systems, applications, and data

\* Support snapshots for backup and recovery



\---



\# 📦 EBS Volume Types



\## 1. gp3 (General Purpose SSD)



\### Features



\* Recommended for most workloads

\* Independent performance and storage scaling

\* Low latency SSD storage

\* Cost-effective



\### Use Cases



\* Web Servers

\* Application Servers

\* Development Environments

\* Small Databases



\---



\## 2. gp2 (General Purpose SSD)



\### Features



\* Previous generation SSD

\* Performance tied to volume size



\### Use Cases



\* Legacy workloads



\### Note



AWS recommends gp3 instead of gp2.



\---



\## 3. io1 / io2 (Provisioned IOPS SSD)



\### Features



\* High-performance SSD storage

\* Provisioned IOPS

\* Low latency



\### Use Cases



\* Oracle Databases

\* SQL Server

\* High Transaction Systems



\---



\## 4. st1 (Throughput Optimized HDD)



\### Features



\* Low-cost HDD storage

\* High throughput



\### Use Cases



\* Big Data

\* Log Processing

\* Data Warehousing



\---



\## 5. sc1 (Cold HDD)



\### Features



\* Lowest cost storage

\* Infrequently accessed data



\### Use Cases



\* Archives

\* Backups



\---



\# 🚀 Steps to Create Volume (AWS Console)



\### Step 1: Login to AWS Console



\* Open AWS Management Console

\* Select Region → `us-east-1`



\---



\### Step 2: Open EC2 Dashboard



```text

AWS Console → EC2

```



\---



\### Step 3: Navigate to Volumes



```text

Elastic Block Store → Volumes

```



Click:



```text

Create Volume

```



\---



\### Step 4: Configure Volume



| Setting           | Value      |

| ----------------- | ---------- |

| Volume Type       | gp3        |

| Size              | 2 GiB      |

| Availability Zone | us-east-1a |



\---



\### Step 5: Add Tags



| Key  | Value         |

| ---- | ------------- |

| Name | devops-volume |



\---



\### Step 6: Create Volume



Click:



```text

Create Volume

```



\---



\# 💻 AWS CLI Commands



\## Create Volume



```bash

aws ec2 create-volume \\

&#x20; --availability-zone us-east-1a \\

&#x20; --size 2 \\

&#x20; --volume-type gp3 \\

&#x20; --tag-specifications 'ResourceType=volume,Tags=\[{Key=Name,Value=devops-volume}]'

```



\---



\## Verify Volume



```bash

aws ec2 describe-volumes \\

&#x20; --filters "Name=tag:Name,Values=devops-volume"

```



\---



\# 🎤 Interview Questions \& Answers



\### 1. What is Amazon EBS?



\*\*Answer:\*\*



Amazon Elastic Block Store (EBS) is a block storage service used with EC2 instances to store operating systems, applications, and persistent data.



\---



\### 2. What is the difference between EBS and S3?



| EBS                    | S3                      |

| ---------------------- | ----------------------- |

| Block Storage          | Object Storage          |

| Attached to EC2        | Accessed via API        |

| Low Latency            | Higher Latency          |

| Single Instance Access | Multiple Access Methods |



\---



\### 3. Can an EBS Volume Exist Without an EC2 Instance?



\*\*Answer:\*\*



Yes.



An EBS volume can exist independently and be attached later to an EC2 instance.



\---



\### 4. What is the difference between gp2 and gp3?



| gp2                            | gp3                             |

| ------------------------------ | ------------------------------- |

| Performance depends on size    | Performance independent of size |

| Older generation               | New generation                  |

| Less flexible                  | More flexible                   |

| More expensive for performance | Better cost optimization        |



\---



\# 💡 Key Learnings



\* Amazon EBS provides persistent block storage.

\* gp3 is the recommended volume type for most workloads.

\* EBS volumes are Availability Zone specific.

\* Snapshots are used for backup and recovery.

\* EBS can exist independently of EC2 instances.



\---



\# ⚠️ Common Mistakes



\* Creating volume in the wrong Availability Zone

\* Attempting to attach a volume across AZs

\* Confusing EBS with S3

\* Forgetting snapshots before modifications

\* Deleting volumes accidentally during instance termination



\---



\# 🧩 Key Takeaway



Amazon EBS is the primary block storage service for EC2 instances, and gp3 is the preferred volume type due to its flexibility, performance, and cost efficiency.



\---



\# ✅ Outcome



Successfully created a gp3 EBS volume named `devops-volume`, understood EBS storage concepts, explored volume types, and learned real-world interview and production use cases.



