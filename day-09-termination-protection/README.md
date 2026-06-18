---

# 🚀 Day 9 – Enable Termination Protection for EC2 Instance

## 📌 Task Overview

As part of the migration process, the Nautilus DevOps team created an EC2 instance but forgot to enable **termination protection** — a critical safeguard to prevent accidental deletion.

Your task is to enable termination protection for the existing EC2 instance.

---

## 🧾 Task Details

* **Instance Name:** `datacenter-ec2`
* **Region:** `us-east-1`
* **Requirement:** Enable termination protection

---

## ⚠️ Why Termination Protection?

Termination protection ensures that an EC2 instance **cannot be accidentally deleted**, even if someone tries to terminate it manually or via automation.

---

## 🛠️ Solution Approaches

### 🔹 Method 1: Using AWS Management Console

1. Log in to the AWS Console
2. Navigate to **EC2 Dashboard**
3. Click on **Instances**
4. Select instance: `datacenter-ec2`
5. Go to **Actions → Instance Settings → Change Termination Protection**
6. Enable: ✅ **Enable termination protection**
7. Click **Save**

---

### 🔹 Method 2: Using AWS CLI

#### Step 1: Get Instance ID

```bash
aws ec2 describe-instances \
  --region us-east-1 \
  --filters "Name=tag:Name,Values=datacenter-ec2" \
  --query "Reservations[].Instances[].InstanceId" \
  --output text
```

#### Step 2: Enable Termination Protection

```bash
aws ec2 modify-instance-attribute \
  --instance-id <INSTANCE_ID> \
  --disable-api-termination \
  --region us-east-1
```

---

## ✅ Verification

Run the following command to confirm:

```bash
aws ec2 describe-instance-attribute \
  --instance-id <INSTANCE_ID> \
  --attribute disableApiTermination \
  --region us-east-1
```

### ✔ Expected Output:

```json
{
  "DisableApiTermination": {
    "Value": true
  }
}
```

---

## 📍 Important Notes

* Ensure all actions are performed in **us-east-1 region**
* Do NOT create a new instance
* Modify only the existing instance: `datacenter-ec2`

---

## 🧠 Key Learning

* Importance of **termination protection** in production environments
* Preventing accidental infrastructure loss
* Using AWS CLI for infrastructure management

---

## 🏁 Conclusion

Successfully enabled termination protection for the EC2 instance, ensuring safer infrastructure management and preventing accidental deletions.

---

## 🔖 One-Line Summary

👉 Enabled termination protection on EC2 instance `datacenter-ec2` in `us-east-1` to prevent accidental deletion.

---
