# 💾 AWS EBS Volume Scenario-Based Interview Questions & Answers

---

# Scenario 01: EC2 Disk is Full

## Problem Statement

A production Linux server is running out of disk space.

```bash
df -h
```

Output:

```text
/dev/xvda1   100%
```

Users are unable to upload files and applications are failing.

---

## Root Cause

The EBS volume attached to the EC2 instance is too small.

---

## Solution

1. Modify EBS Volume

```text
EC2 → Volumes → Modify Volume
```

Example:

```text
2 GiB → 20 GiB
```

2. Extend filesystem:

```bash
sudo growpart /dev/xvda 1

sudo xfs_growfs /
```

---

## Interview Question

How do you increase storage without downtime?

### Answer

Modify the EBS volume size and extend the filesystem from the operating system.

---

# Scenario 02: Accidentally Deleted Important Files

## Problem Statement

A developer accidentally deletes application files.

```bash
rm -rf /app/*
```

Production application stops working.

---

## Root Cause

Critical files removed.

---

## Solution

Restore from EBS Snapshot.

Steps:

1. Create volume from snapshot
2. Attach volume
3. Recover files

---

## Interview Question

How do you recover data from EBS?

### Answer

Use EBS Snapshots to create a new volume and restore data.

---

# Scenario 03: Volume Attached but Not Visible

## Problem Statement

New EBS volume attached successfully.

AWS Console shows:

```text
In-use
```

But Linux does not show the disk.

---

## Investigation

Check:

```bash
lsblk
```

or

```bash
fdisk -l
```

---

## Root Cause

Volume attached but not mounted.

---

## Solution

```bash
sudo mkfs -t xfs /dev/xvdf

sudo mkdir /data

sudo mount /dev/xvdf /data
```

---

## Interview Question

Why is an EBS volume not visible after attachment?

### Answer

The volume must be partitioned, formatted, and mounted inside the operating system.

---

# Scenario 04: EC2 Instance Accidentally Terminated

## Problem Statement

Production EC2 instance deleted accidentally.

Application unavailable.

---

## Investigation

Root EBS volume was deleted with instance.

---

## Solution

Recover from:

```text
EBS Snapshot
```

Create:

```text
New Volume
New EC2 Instance
```

Attach and restore.

---

## Interview Question

Can EBS help in disaster recovery?

### Answer

Yes. EBS Snapshots provide backup and recovery capabilities.

---

# Scenario 05: Unable to Attach EBS Volume

## Problem Statement

Volume exists.

Instance exists.

Attachment fails.

---

## Investigation

Instance:

```text
us-east-1a
```

Volume:

```text
us-east-1b
```

---

## Root Cause

EBS is Availability Zone specific.

---

## Solution

Create volume in same AZ or create snapshot and restore volume in target AZ.

---

## Interview Question

Can an EBS volume be attached across Availability Zones?

### Answer

No.

EBS volumes can only be attached to EC2 instances in the same Availability Zone.

---

# Scenario 06: High Database Latency

## Problem Statement

Database response time increased significantly.

---

## Investigation

Database using:

```text
gp2
```

Volume with low IOPS.

---

## Solution

Upgrade volume:

```text
gp2 → gp3
```

or

```text
io2
```

for critical databases.

---

## Interview Question

Which EBS volume type is best for databases?

### Answer

io1/io2 for high-performance databases.

gp3 for most production workloads.

---

# Scenario 07: EBS Volume Deleted Accidentally

## Problem Statement

Administrator deletes an EBS volume.

Important data lost.

---

## Investigation

Snapshots exist.

---

## Solution

Create new volume from snapshot.

```text
Snapshots → Create Volume
```

Attach to EC2.

---

## Interview Question

What is the importance of EBS Snapshots?

### Answer

Snapshots provide backup, recovery, migration, and disaster recovery capabilities.

---

# Scenario 08: Root Volume Deleted After Instance Termination

## Problem Statement

EC2 instance terminated.

Root volume disappeared.

---

## Investigation

Root volume setting:

```text
Delete on Termination = Enabled
```

---

## Root Cause

Default behavior of root EBS volume.

---

## Solution

Disable:

```text
Delete on Termination
```

for critical workloads.

---

## Interview Question

What happens to the root EBS volume when an instance is terminated?

### Answer

By default, the root EBS volume is deleted unless "Delete on Termination" is disabled.

---

# Scenario 09: Multiple Servers Need Shared Storage

## Problem Statement

Two EC2 instances need access to the same storage.

---

## Investigation

Trying to attach one EBS volume to two servers.

---

## Root Cause

Standard EBS supports single attachment.

---

## Solution

Use:

```text
Amazon EFS
```

or

```text
EBS Multi-Attach (io1/io2 only)
```

---

## Interview Question

Can one EBS volume be attached to multiple EC2 instances?

### Answer

Normally No.

Only io1/io2 Multi-Attach supports multiple attachments.

---

# Scenario 10: Backup Strategy for Production Server

## Problem Statement

Management requires daily backups.

---

## Solution

Use:

```text
Amazon Data Lifecycle Manager (DLM)
```

or

```text
AWS Backup
```

to automate EBS snapshots.

---

## Interview Question

How do you automate EBS backups?

### Answer

Using AWS Backup, Amazon DLM, or scheduled Lambda functions.

---

# 🎤 Frequently Asked EBS Interview Questions

### What is Amazon EBS?

Block-level storage service for EC2 instances.

---

### Difference Between EBS and Instance Store?

| EBS              | Instance Store              |
| ---------------- | --------------------------- |
| Persistent       | Temporary                   |
| Can Snapshot     | Cannot Snapshot             |
| Survives Reboot  | Data Lost on Stop/Terminate |
| Network Attached | Physically Attached         |

---

### What is an EBS Snapshot?

Point-in-time backup of an EBS volume stored in Amazon S3.

---

### Can EBS Volume Size Be Increased?

Yes.

AWS supports online volume expansion.

---

### Which EBS Volume Type is Recommended?

For most workloads:

```text
gp3
```

---

### Is EBS Regional or AZ Specific?

EBS Volumes are Availability Zone specific.

---

### Can We Change Volume Type?

Yes.

Example:

```text
gp2 → gp3
gp3 → io2
```

without recreating the volume.

---

# 💡 Key Takeaway

Most production EBS issues involve:

* Volume Full
* Wrong Availability Zone
* Missing Snapshots
* Performance Bottlenecks
* Accidental Deletion

Understanding these scenarios is critical for AWS Cloud and DevOps interviews.

