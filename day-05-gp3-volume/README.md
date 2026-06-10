# 📅 Day 5: Create GP3 EBS Volume

---

# 🧠 Task

Create an AWS EBS volume with the following specifications:

* Name: `devops-volume`
* Volume Type: `gp3`
* Size: `2 GiB`
* Region: `us-east-1`

---

# 🎯 Objective

* Understand Amazon EBS volumes
* Learn different EBS volume types
* Create and manage storage volumes in AWS
* Understand performance characteristics of EBS storage
* Practice AWS Console and CLI operations

---

# ☁️ AWS Details

| Property    | Value         |
| ----------- | ------------- |
| Service     | Amazon EBS    |
| Region      | us-east-1     |
| Volume Name | devops-volume |
| Volume Type | gp3           |
| Size        | 2 GiB         |

---

# 🧠 What is Amazon EBS?

Amazon Elastic Block Store (EBS) is a block-level storage service designed for use with Amazon EC2 instances.

EBS volumes:

* Act like virtual hard disks
* Can be attached to EC2 instances
* Store operating systems, applications, and data
* Support snapshots for backup and recovery

---

# 📦 EBS Volume Types

## 1. gp3 (General Purpose SSD)

### Features

* Recommended for most workloads
* Independent performance and storage scaling
* Low latency SSD storage
* Cost-effective

### Use Cases

* Web Servers
* Application Servers
* Development Environments
* Small Databases

---

## 2. gp2 (General Purpose SSD)

### Features

* Previous generation SSD
* Performance tied to volume size

### Use Cases

* Legacy workloads

### Note

AWS recommends gp3 instead of gp2.

---

## 3. io1 / io2 (Provisioned IOPS SSD)

### Features

* High-performance SSD storage
* Provisioned IOPS
* Low latency

### Use Cases

* Oracle Databases
* SQL Server
* High Transaction Systems

---

## 4. st1 (Throughput Optimized HDD)

### Features

* Low-cost HDD storage
* High throughput

### Use Cases

* Big Data
* Log Processing
* Data Warehousing

---

## 5. sc1 (Cold HDD)

### Features

* Lowest cost storage
* Infrequently accessed data

### Use Cases

* Archives
* Backups

---

# 🚀 Steps to Create Volume (AWS Console)

## Step 1: Login to AWS Console

* Open AWS Management Console
* Select Region → `us-east-1`

---

## Step 2: Open EC2 Dashboard

```text
AWS Console → EC2
```

---

## Step 3: Navigate to Volumes

```text
Elastic Block Store → Volumes
```

Click:

```text
Create Volume
```

---

## Step 4: Configure Volume

| Setting           | Value      |
| ----------------- | ---------- |
| Volume Type       | gp3        |
| Size              | 2 GiB      |
| Availability Zone | us-east-1a |

---

## Step 5: Add Tags

| Key  | Value         |
| ---- | ------------- |
| Name | devops-volume |

---

## Step 6: Create Volume

Click:

```text
Create Volume
```

---

# 💻 AWS CLI Commands

## Create Volume

```bash
aws ec2 create-volume \
  --availability-zone us-east-1a \
  --size 2 \
  --volume-type gp3 \
  --tag-specifications 'ResourceType=volume,Tags=[{Key=Name,Value=devops-volume}]'
```

---

## Verify Volume

```bash
aws ec2 describe-volumes \
  --filters "Name=tag:Name,Values=devops-volume"
```

---

# 🎤 Interview Questions & Answers

### 1. What is Amazon EBS?

**Answer:**

Amazon Elastic Block Store (EBS) is a block storage service used with EC2 instances to store operating systems, applications, and persistent data.

---

### 2. What is the difference between EBS and S3?

| EBS                    | S3                      |
| ---------------------- | ----------------------- |
| Block Storage          | Object Storage          |
| Attached to EC2        | Accessed via API        |
| Low Latency            | Higher Latency          |
| Single Instance Access | Multiple Access Methods |

---

### 3. Can an EBS Volume Exist Without an EC2 Instance?

**Answer:**

Yes. An EBS volume can exist independently and be attached later to an EC2 instance.

---

### 4. What is the difference between gp2 and gp3?

| gp2                         | gp3                             |
| --------------------------- | ------------------------------- |
| Performance depends on size | Performance independent of size |
| Older generation            | New generation                  |
| Less flexible               | More flexible                   |
| Better cost optimization    | Recommended by AWS              |

---

### 5. Is EBS Regional or Availability Zone Specific?

**Answer:**

EBS volumes are Availability Zone specific.

Example:

* Volume in `us-east-1a`
* Can only be attached to EC2 instances in `us-east-1a`

---

### 6. What is an EBS Snapshot?

**Answer:**

An EBS Snapshot is a point-in-time backup of an EBS volume stored in Amazon S3.

---

### 7. Can You Increase EBS Volume Size?

**Answer:**

Yes. AWS supports online volume expansion without data loss.

---

### 8. What Happens When an EC2 Instance is Terminated?

**Answer:**

It depends on the **Delete on Termination** setting.

* Enabled → Volume deleted automatically
* Disabled → Volume remains available

---

# 💾 EBS Scenario-Based Questions

## Scenario 1: EBS Volume Full

### Problem

Production server disk usage reaches 100%.

```bash
df -h
```

Output:

```text
/dev/xvda1 100%
```

### Solution

1. Modify EBS Volume Size
2. Extend Partition
3. Extend Filesystem

```bash
sudo growpart /dev/xvda 1

sudo xfs_growfs /
```

### Interview Question

How do you increase EBS storage without downtime?

### Answer

Modify the EBS volume size and extend the filesystem from the operating system.

---

## Scenario 2: Unable to Attach Volume

### Problem

Volume exists but attachment fails.

### Investigation

EC2:

```text
us-east-1a
```

Volume:

```text
us-east-1b
```

### Root Cause

EBS volumes are Availability Zone specific.

### Solution

Create a new volume in the same Availability Zone or restore from snapshot.

---

## Scenario 3: Accidentally Deleted Data

### Problem

Important application files were deleted.

### Solution

Restore data using an EBS Snapshot.

### Interview Question

How do you recover deleted files from EBS?

### Answer

Restore from a snapshot by creating a new volume and attaching it to an EC2 instance.

---

## Scenario 4: EC2 Instance Accidentally Deleted

### Problem

Production EC2 instance was terminated.

### Solution

Create a new EC2 instance and restore data using EBS snapshots.

---

## Scenario 5: Volume Attached but Not Visible

### Problem

AWS shows:

```text
In-use
```

but Linux does not show the disk.

### Solution

Format and mount the volume.

```bash
sudo mkfs -t xfs /dev/xvdf

sudo mkdir /data

sudo mount /dev/xvdf /data
```

---

# 💡 Key Learnings

* Amazon EBS provides persistent block storage.
* gp3 is the recommended volume type for most workloads.
* EBS volumes are Availability Zone specific.
* Snapshots are used for backup and recovery.
* EBS can exist independently of EC2 instances.

---

# ⚠️ Common Mistakes

* Creating volumes in the wrong Availability Zone
* Trying to attach volumes across AZs
* Confusing EBS with S3
* Not taking snapshots before modifications
* Deleting volumes accidentally

---

# 🧩 Key Takeaway

Amazon EBS is the primary block storage service for EC2 instances, and gp3 is the preferred volume type due to its flexibility, performance, and cost efficiency.

---

# ✅ Outcome

Successfully created a gp3 EBS volume named `devops-volume`, learned EBS storage concepts, explored volume types, practiced AWS CLI commands, and understood real-world production troubleshooting scenarios.
