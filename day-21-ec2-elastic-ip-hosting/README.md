---

# 📅 Day 21: Setting Up an EC2 Instance with an Elastic IP for Application Hosting

---

## 🧠 Task

Create an EC2 instance `datacenter-ec2` using a Linux AMI (e.g., Ubuntu), instance type `t2.micro`, and associate an Elastic IP `datacenter-eip` in the **us-east-1** region.

---

## 🎯 Objective

* Launch an EC2 instance for application hosting
* Understand Elastic IP usage for stable public access
* Practice end-to-end AWS provisioning

---

## ☁️ AWS Details

* Service: Amazon EC2
* Region: us-east-1
* Instance Name: `datacenter-ec2`
* Instance Type: `t2.micro`
* AMI: Linux (Ubuntu or similar)
* Elastic IP Name (Tag): `datacenter-eip`

---

## 🚀 Steps to Execute (AWS Console)

### 1. Login to AWS Console

* Use provided credentials
* Select region: **us-east-1**

---

### 2. Launch EC2 Instance

* Navigate to **EC2 → Instances → Launch Instance**
* Name → `datacenter-ec2`
* AMI → Select Ubuntu/Linux
* Instance Type → `t2.micro`
* Key Pair → Select/Create key pair
* Network Settings → Default VPC
* Click **Launch Instance**

---

### 3. Allocate Elastic IP

* Go to **Elastic IPs**
* Click **Allocate Elastic IP address**
* Click **Allocate**

---

### 4. Tag Elastic IP (Important ✅)

* Select allocated IP
* Add tag:

  * Key → `Name`
  * Value → `datacenter-eip`

> ⚠️ Without this tag, the task may fail even if EIP is attached.

---

### 5. Associate Elastic IP

* Select Elastic IP
* Click **Actions → Associate Elastic IP address**
* Resource Type → Instance
* Instance → `datacenter-ec2`
* Click **Associate**

---

## 💻 AWS CLI Method

### ✅ Step 1: Launch EC2 Instance

```bash id="d1"
aws ec2 run-instances \
  --image-id <ami-id> \
  --instance-type t2.micro \
  --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=datacenter-ec2}]'
```

---

### ✅ Step 2: Allocate Elastic IP

```bash id="d2"
aws ec2 allocate-address --domain vpc
```

---

### ✅ Step 3: Tag Elastic IP (Critical Step ⚠️)

```bash id="d2a"
aws ec2 create-tags \
  --resources <allocation-id> \
  --tags Key=Name,Value=datacenter-eip
```

---

### ✅ Step 4: Get Instance ID

```bash id="d3"
aws ec2 describe-instances \
--filters "Name=tag:Name,Values=datacenter-ec2" \
--query "Reservations[].Instances[].InstanceId" \
--output text
```

---

### ✅ Step 5: Associate Elastic IP

```bash id="d4"
aws ec2 associate-address \
  --instance-id <instance-id> \
  --allocation-id <allocation-id>
```

---

## 🔍 Verification

```bash id="d5"
aws ec2 describe-instances \
--query "Reservations[].Instances[].{State:State.Name,PublicIP:PublicIpAddress}" \
--output table
```

👉 Confirm:

* Instance is **running**
* Public IP is present

---

## 💡 Key Learning

* EC2 is used to host applications in AWS
* Elastic IP provides a **static public IP**
* Tagging is critical for identification
* Always verify resources before using IDs

---

## ⚠️ Challenges Faced

* Elastic IP was attached but not correctly named
* Allocation ID error during association
* Initial assumption that resources already exist

---

## 🔧 Fix / Learning

### 🔹 Issue 1: Missing Name Tag

* Elastic IP was not tagged correctly
* Fixed by adding:

```bash
aws ec2 create-tags --resources <allocation-id> --tags Key=Name,Value=datacenter-eip
```

---

### 🔹 Issue 2: Invalid Allocation ID Error

```bash
InvalidAllocationID.NotFound
```

👉 Cause:

* Wrong region OR EIP didn’t exist

👉 Fix:

```bash
aws ec2 describe-addresses
aws ec2 allocate-address --domain vpc
```

---

### 🔹 Final Fix Flow

```bash
aws ec2 allocate-address --domain vpc
aws ec2 create-tags --resources <allocation-id> --tags Key=Name,Value=datacenter-eip
aws ec2 associate-address --instance-id <instance-id> --allocation-id <allocation-id>
```

---

## 🧩 Summary

Successfully launched EC2 instance `datacenter-ec2`, allocated and attached an Elastic IP, resolved allocation ID errors, and ensured correct tagging (`datacenter-eip`) for successful validation.

---
