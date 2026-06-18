# Day 50: Expanding EC2 Instance Storage for Development Needs

---

## 📌 Task

The Nautilus DevOps Team identified that the EC2 instance used for development activities is running low on storage. The attached EBS volume currently has a size of **8 GiB**, and it needs to be expanded to **12 GiB** without disrupting existing workloads.

Requirements:

* Identify the EBS volume attached to `nautilus-ec2`
* Increase volume size from **8 GiB → 12 GiB**
* Reflect changes inside the instance filesystem
* Ensure root (`/`) partition uses expanded storage
* Access EC2 using:

```text
/root/nautilus-keypair.pem
```

---

## 🎯 Objective

This task demonstrates:

* EBS volume management
* Storage expansion in AWS
* Linux filesystem resizing
* EC2 administration
* Extending storage without downtime

---

## ☁️ AWS Services Used

| Service         | Purpose                  |
| --------------- | ------------------------ |
| EC2             | Compute instance         |
| EBS             | Persistent block storage |
| SSH             | Instance access          |
| Linux Utilities | Filesystem resize        |

---

## 🏗 Architecture

```text
EC2 Instance
(nautilus-ec2)
        ↓
Attached EBS Volume
(8 GiB)
        ↓
Modify Volume
(12 GiB)
        ↓
SSH into EC2
        ↓
Extend Partition
        ↓
Resize Filesystem
        ↓
Root Partition Updated
(/ = 12 GiB)
```

---

# 🚀 Step 1: Identify Attached Volume

Navigate:

```text
AWS Console
→ EC2
→ Instances
→ nautilus-ec2
```

Open:

```text
Storage Tab
```

Identify attached volume:

Example:

| Volume ID    | Existing Size |
| ------------ | ------------- |
| vol-xxxxxxxx | 8 GiB         |

---

# 🚀 Step 2: Modify EBS Volume Size

Click attached volume ID.

Navigate:

```text
Actions
→ Modify Volume
```

Update:

| Parameter | Value  |
| --------- | ------ |
| Size      | 12 GiB |

Click:

```text
Modify
```

Confirm:

```text
Modify Volume
```

Wait until:

```text
Volume Modification State → Optimizing/Completed
```

---

# 🚀 Step 3: Connect to EC2 Instance

SSH into EC2:

```bash
ssh -i /root/nautilus-keypair.pem ubuntu@<public-ip>
```

Verify current storage:

```bash
df -h
```

Expected before expansion:

```bash
Filesystem      Size Used Avail Use%
/dev/root       7.8G
```

Check block devices:

```bash
lsblk
```

Example output:

```bash
NAME     MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
xvda     202:0    0  12G  0 disk
└─xvda1  202:1    0   8G  0 part /
```

Notice:

* Disk updated to **12G**
* Partition still shows **8G**

---

# 🚀 Step 4: Extend Root Partition

Run:

```bash
sudo growpart /dev/xvda 1
```

Expected:

```bash
CHANGED:
partition=1
start=2048
old: size=...
new: size=...
```

Verify:

```bash
lsblk
```

Expected:

```bash
NAME     SIZE
xvda      12G
└─xvda1   12G
```

---

# 🚀 Step 5: Resize Filesystem

For Ubuntu (ext4 filesystem):

```bash
sudo resize2fs /dev/xvda1
```

If filesystem is XFS:

```bash
sudo xfs_growfs /
```

---

# 🚀 Step 6: Verify Expanded Storage

Run:

```bash
df -h
```

Expected:

```bash
Filesystem      Size Used Avail Use%
/dev/root        12G
```

Root partition now reflects expanded storage.

---

## ⚠️ Challenges Faced

### 1. Volume modified but storage not increasing

Issue:

```bash
Disk size changed
Filesystem remained same
```

Root cause:

Partition not expanded.

Fix:

```bash
sudo growpart /dev/xvda 1
```

---

### 2. Filesystem size unchanged

Issue:

```bash
Partition expanded
Filesystem unchanged
```

Root cause:

Filesystem resize missing.

Fix:

```bash
sudo resize2fs /dev/xvda1
```

---

### 3. Wrong filesystem command used

Issue:

```bash
Bad magic number in super-block
```

Root cause:

Using `resize2fs` on XFS.

Fix:

```bash
sudo xfs_growfs /
```

---

### 4. growpart command not available

Error:

```bash
growpart: command not found
```

Fix:

Install package:

```bash
sudo apt update
sudo apt install cloud-guest-utils -y
```

---

### 5. SSH connection failure

Error:

```bash
Permission denied (publickey)
```

Root cause:

Wrong key permissions.

Fix:

```bash
chmod 400 /root/nautilus-keypair.pem
```

---

## 🔧 Fix / Learning

* EBS modifications do not automatically resize partitions
* Partition expansion and filesystem resizing are separate operations
* `lsblk` helps verify block devices
* `df -h` verifies usable filesystem size
* Storage expansion can happen without downtime

---

## 💡 Key Learning

Expanding cloud storage isn't just increasing the EBS size — Linux partitions and filesystems must also be resized to make the extra space usable.

Cloud infrastructure often requires changes at both the AWS layer and the operating system layer.

---

## ✅ Final Result

Successfully completed:

✅ Identified EBS volume attached to `nautilus-ec2`
✅ Increased volume size from **8 GiB → 12 GiB**
✅ Expanded partition using `growpart`
✅ Resized filesystem
✅ Verified root partition reflects **12 GiB**

Verification:

```bash
df -h
```

Expected output:

```bash
Filesystem      Size Used Avail Use%
/dev/root        12G
```

🚀 Day 50 completed — successfully expanded EC2 storage with zero downtime while learning EBS volume management and Linux filesystem resizing.
