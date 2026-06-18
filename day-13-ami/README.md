---

# 📅 Day 13: Create AMI from EC2 Instance

---

## 🧠 Task

Create an AMI from the EC2 instance `nautilus-ec2` with the name `nautilus-ec2-ami` in the **us-east-1** region.
Ensure the AMI reaches the **available** state.

---

## 🎯 Objective

* Understand Amazon Machine Images (AMI)
* Create reusable machine templates from EC2 instances
* Learn how to verify AMI status

---

## ☁️ AWS Details

* Service: Amazon EC2 (AMI)
* Region: us-east-1
* Source Instance: `nautilus-ec2`
* AMI Name: `nautilus-ec2-ami`

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

### 3. Select Instance

* Go to **Instances**
* Select instance: `nautilus-ec2`

---

### 4. Create AMI

* Click **Actions → Image and templates → Create Image**
* Image Name → `nautilus-ec2-ami`
* Keep default settings
* Click **Create Image**

---

### 5. Wait for AMI to Become Available

* Go to **AMIs** section
* Check status → `available`

---

## 💻 AWS CLI Method

### ✅ Step 1: Get Instance ID

```bash id="a1"
aws ec2 describe-instances --filters "Name=tag:Name,Values=nautilus-ec2" --query "Reservations[].Instances[].InstanceId" --output text
```

---

### ✅ Step 2: Create AMI

```bash id="a2"
aws ec2 create-image \
  --instance-id <instance-id> \
  --name "nautilus-ec2-ami" \
  --no-reboot
```

---

### ✅ Step 3: Verify AMI Status

```bash id="a3"
aws ec2 describe-images --owners self --query "Images[*].[ImageId,Name,State]" --output table
```

👉 Ensure:

* Name → `nautilus-ec2-ami`
* State → `available`

---

## 🔍 Important Notes

* `--no-reboot` avoids instance restart (faster but slight risk of inconsistency)
* AMI creation may take a few minutes
* Always verify status before submission

---

## 💡 Key Learning

* AMI is a snapshot/template of an EC2 instance
* Used to launch identical instances quickly
* Helps in backup, scaling, and automation

---

## ⚠️ Challenges Faced

* Identifying correct instance ID
* Waiting for AMI to reach `available` state
* Understanding reboot vs no-reboot behavior

---

## 🧩 Summary

Successfully created an AMI `nautilus-ec2-ami` from EC2 instance `nautilus-ec2`, ensuring it reached the available state for reuse and deployment.

---

---

# 🚀 Pro Tip

👉 Interview question:
**Why use AMI?**

👉 Answer:
“To create reusable templates of EC2 instances for scaling, backup, and consistent deployments.”

---
