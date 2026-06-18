---

# 📅 Day 12: Attach Volume to EC2 Instance

---

## 🧠 Task

Attach the EBS volume `devops-volume` to the EC2 instance `devops-ec2` in the **us-east-1** region.
Ensure the device name is set to `/dev/sdb`.

---

## 🎯 Objective

* Understand how to attach EBS volumes to EC2
* Practice working with block storage in AWS
* Learn device naming conventions

---

## ☁️ AWS Details

* Service: Amazon EC2 (EBS)
* Region: us-east-1
* Instance Name: `devops-ec2`
* Volume Name: `devops-volume`
* Device Name: `/dev/sdb`

---

## 🚀 Steps to Execute (AWS Console)

### 1. Login to AWS Console

* Use provided credentials
* Select region: **us-east-1**

---

### 2. Navigate to EC2

* Search for **EC2**
* Open EC2 Dashboard

---

### 3. Go to Volumes

* Click **Volumes** under Elastic Block Store

---

### 4. Select Volume

* Find: `devops-volume`
* Select the volume

---

### 5. Attach Volume

* Click **Actions → Attach Volume**
* Instance → `devops-ec2`
* Device Name → `/dev/sdb`
* Click **Attach**

---

## 💻 AWS CLI Method

### ✅ Step 1: Get Instance ID

```bash id="v1"
aws ec2 describe-instances --filters "Name=tag:Name,Values=devops-ec2" --query "Reservations[].Instances[].InstanceId" --output text
```

---

### ✅ Step 2: Get Volume ID

```bash id="v2"
aws ec2 describe-volumes --query "Volumes[*].[VolumeId,Size]" --output table
```

👉 Identify the volume corresponding to `devops-volume`

---

### ⚠️ Important Note

❌ Do NOT rely only on Name tag:

* Volume may not always have a Name tag
* Use VolumeId carefully

---

### ✅ Step 3: Attach Volume

```bash id="v3"
aws ec2 attach-volume \
  --volume-id <volume-id> \
  --instance-id <instance-id> \
  --device /dev/sdb
```

---

## 🔍 Verification

```bash id="v4"
aws ec2 describe-volumes --volume-ids <volume-id>
```

👉 Confirm:

* `State` → `in-use`
* `Attachments` → InstanceId present

---

## 💡 Key Learning

* EBS volumes provide persistent storage for EC2
* Volumes must be in the same Availability Zone as the instance
* Device names define how volumes are mounted in the OS
* Always verify attachment status

---

## ⚠️ Challenges Faced

* Identifying correct Volume ID
* Understanding device naming conventions
* Ensuring correct AZ alignment

---

## 🧩 Summary

Successfully attached an EBS volume to EC2 instance `devops-ec2` using device name `/dev/sdb`, ensuring proper storage configuration and verification.

---

---

# 🚀 Pro Tip

👉 Interview question:
**What happens if volume and instance are in different AZs?**

👉 Answer:
“Attachment will fail because EBS volumes must be in the same Availability Zone as the EC2 instance.”

---
