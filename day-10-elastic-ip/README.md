# 📅 Day 10: Attach Elastic IP to EC2 Instance

---

## 🧠 Task

Attach the Elastic IP `devops-ec2-eip` to the EC2 instance `devops-ec2` in the **us-east-1** region.

---

## 🎯 Objective

* Understand Elastic IP (EIP) usage in AWS
* Associate a static public IP with an EC2 instance
* Practice AWS networking operations using Console and CLI

---

## ☁️ AWS Details

* Service: Amazon EC2
* Region: us-east-1
* Instance Name: `devops-ec2`
* Elastic IP (Logical Name): `devops-ec2-eip`

> Note: Elastic IPs may not always have a **Name tag** in AWS.

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

### 3. Go to Elastic IPs

* Click **Elastic IPs** under Network & Security

---

### 4. Select Elastic IP

* Identify the available Elastic IP
* Select it

---

### 5. Associate Elastic IP

* Click **Actions → Associate Elastic IP address**
* Resource Type → Instance
* Instance → `devops-ec2`
* Private IP → Default
* Click **Associate**

---

## 💻 AWS CLI Method (Corrected Real-World Approach)

### ✅ Step 1: Get Instance ID

```bash id="d1"
aws ec2 describe-instances --filters "Name=tag:Name,Values=devops-ec2" --query "Reservations[].Instances[].InstanceId" --output text
```

---

### ✅ Step 2: Get Elastic IP Allocation ID

```bash id="d2"
aws ec2 describe-addresses --query "Addresses[*].AllocationId" --output text
```

👉 If multiple EIPs exist:

```bash id="d2a"
aws ec2 describe-addresses --query "Addresses[*].[PublicIp,AllocationId]" --output table
```

---

### ⚠️ Important Note

❌ Do NOT use:

```bash id="wrong"
--filters "Name=tag:Name,Values=devops-ec2-eip"
```

👉 Because Elastic IP may not have a **Name tag**

---

### ✅ Step 3: Attach Elastic IP

```bash id="d3"
aws ec2 associate-address --instance-id <instance-id> --allocation-id <allocation-id>
```

---

## 🔍 Verification

```bash id="d4"
aws ec2 describe-addresses --allocation-ids <allocation-id>
```

👉 Confirm:

* `InstanceId` is present
* EIP is attached

---

## 💡 Key Learning

* Elastic IP provides a **static public IP** for EC2
* Allocation ID is used to identify EIP in CLI
* Not all AWS resources are tagged
* Always verify resources instead of assuming

---

## ⚠️ Challenges Faced

* No output when filtering EIP by Name tag
* Understanding that Elastic IPs may not have tags
* Identifying correct Allocation ID

---

## 🧩 Summary

Successfully attached an Elastic IP to EC2 instance `devops-ec2` by identifying the correct Allocation ID and avoiding incorrect tag-based filtering.

---

---

# 🚀 Pro Tip

👉 Interview question:
**Why use Elastic IP instead of default public IP?**

👉 Answer:
“Elastic IP remains constant even after instance stop/start, unlike default public IP which changes.”

---
