# 📅 Day 8: Enable Stop Protection for EC2 Instance

---

## 🧠 Task

Enable **stop protection** for the EC2 instance `xfusion-ec2` in the **us-east-1** region.

---

## 🎯 Objective

* Understand EC2 instance protection features
* Prevent accidental stopping of critical instances
* Practice modifying instance settings using AWS Console and CLI

---

## ☁️ AWS Details

* Service: Amazon EC2
* Region: us-east-1
* Instance Name: `xfusion-ec2`
* Feature: Stop Protection

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
* Select instance: `xfusion-ec2`

---

### 4. Enable Stop Protection

* Click **Actions → Instance Settings → Change Stop Protection**
* Select **Enable**
* Save changes

---

## 💻 AWS CLI Method (Final Correct Version)

### ✅ Step 1: Get Instance ID

```bash id="fix1"
aws ec2 describe-instances --filters "Name=tag:Name,Values=xfusion-ec2" --query "Reservations[].Instances[].InstanceId" --output text
```

👉 Output example:

```bash
i-0a43082cbd147d22a
```

---

### ✅ Step 2: Enable Stop Protection (Correct Syntax)

```bash id="fix2"
aws ec2 modify-instance-attribute --instance-id <instance-id> --disable-api-stop Value=false
```

---

### 🔍 Why `Value=false`?

* `disable-api-stop = false` → Stop protection is **enabled**
* This is a **structured parameter**, so `Value=` is mandatory

---

### ❌ Common Mistake

```bash id="wrong1"
aws ec2 modify-instance-attribute --instance-id <instance-id> --disable-api-stop false
```

👉 This fails because:

* AWS expects `Key=Value` format
* Not plain `false`

---

## 🔍 Verification

```bash id="fix3"
aws ec2 describe-instance-attribute --instance-id <instance-id> --attribute disableApiStop
```

Expected Output:

```json id="fix4"
{
  "DisableApiStop": {
    "Value": false
  }
}
```

---

## 💡 Key Learning

* Stop protection prevents accidental stopping of EC2 instances
* Critical for production environments
* AWS CLI requires correct parameter formats
* Structured parameters must follow `Key=Value` syntax

---

## ⚠️ Challenges Faced

* Incorrect use of `\` in CLI commands
* AWS CLI parsing errors
* Misunderstanding of structured parameters (`Value=false`)

---

## 🧩 Summary

Enabled stop protection on EC2 instance `xfusion-ec2` by correctly formatting AWS CLI commands and understanding structured parameter syntax.

---
