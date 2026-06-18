# 📅 Day 11: Attach Elastic Network Interface (ENI) to EC2 Instance

---

## 🧠 Task

Attach the Elastic Network Interface `xfusion-eni` to the EC2 instance `xfusion-ec2` in the **us-east-1** region.

---

## 🎯 Objective

* Understand Elastic Network Interfaces (ENI) in AWS
* Attach an existing network interface to an EC2 instance
* Ensure correct attachment state and instance readiness

---

## ☁️ AWS Details

* Service: Amazon EC2
* Region: us-east-1
* Instance Name: `xfusion-ec2`
* Network Interface Name: `xfusion-eni`

> Note: Ensure the EC2 instance is fully initialized before attaching ENI.

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

### 3. Go to Network Interfaces

* Click **Network Interfaces** under Network & Security

---

### 4. Select ENI

* Find: `xfusion-eni`
* Select the network interface

---

### 5. Attach ENI

* Click **Actions → Attach**
* Instance → `xfusion-ec2`
* Device Index → `1` (default for secondary interface)
* Click **Attach**

---

## 💻 AWS CLI Method

### ✅ Step 1: Get Instance ID

```bash id="e1"
aws ec2 describe-instances --filters "Name=tag:Name,Values=xfusion-ec2" --query "Reservations[].Instances[].InstanceId" --output text
```

---

### ✅ Step 2: Get Network Interface ID

```bash id="e2"
aws ec2 describe-network-interfaces --query "NetworkInterfaces[*].[NetworkInterfaceId,Description]" --output table
```

👉 Identify the ENI (xfusion-eni) from description

---

### ⚠️ Important Note

❌ Do NOT rely only on Name tag:

* ENIs may not always have tags
* Use description or ID

---

### ✅ Step 3: Attach ENI

```bash id="e3"
aws ec2 attach-network-interface \
  --network-interface-id <eni-id> \
  --instance-id <instance-id> \
  --device-index 1
```

---

## 🔍 Verification

```bash id="e4"
aws ec2 describe-network-interfaces --network-interface-ids <eni-id>
```

👉 Confirm:

* `Status` → `in-use`
* `Attachment` → InstanceId present

---

## 💡 Key Learning

* ENI acts as a virtual network card for EC2
* Multiple ENIs can be attached to a single instance
* Device index defines primary (0) and secondary (1+) interfaces
* Always verify attachment status

---

## ⚠️ Challenges Faced

* Identifying correct ENI without relying on tags
* Understanding device index usage
* Ensuring instance is fully initialized

---

## 🧩 Summary

Successfully attached an Elastic Network Interface to EC2 instance `xfusion-ec2`, ensuring proper network configuration and validating attachment status.

---
