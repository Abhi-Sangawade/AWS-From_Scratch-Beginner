---

# 📅 Day 20: Create IAM Role for EC2 with Policy Attachment

---

## 🧠 Task

Create an IAM role `iamrole_rose` for EC2 and attach the policy `iampolicy_rose`.

---

## 🎯 Objective

* Understand IAM roles and their use cases
* Create roles for AWS services (EC2)
* Attach policies to roles for permission management

---

## ☁️ AWS Details

* Service: AWS IAM
* Region: Global (IAM is not region-specific)
* Role Name: `iamrole_rose`
* Trusted Entity: AWS Service (EC2)
* Policy Name: `iampolicy_rose`

---

## 🚀 Steps to Execute (AWS Console)

### 1. Login to AWS Console

* Use provided credentials

---

### 2. Navigate to IAM

* Search for **IAM**
* Open IAM Dashboard

---

### 3. Create Role

* Click **Roles → Create role**
* Trusted entity type → **AWS service**
* Use case → **EC2**

---

### 4. Attach Policy

* Select policy → `iampolicy_rose`

---

### 5. Name Role

* Role name → `iamrole_rose`
* Click **Create role**

---

## 💻 AWS CLI Method

### ✅ Step 1: Create Trust Policy (trust.json)

```json id="r1"
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

---

### ✅ Step 2: Create IAM Role

```bash id="r2"
aws iam create-role \
  --role-name iamrole_rose \
  --assume-role-policy-document file://trust.json
```

---

### ✅ Step 3: Attach Policy

```bash id="r3"
aws iam attach-role-policy \
  --role-name iamrole_rose \
  --policy-arn arn:aws:iam::<account-id>:policy/iampolicy_rose
```

👉 Replace `<account-id>` with your AWS account ID

---

## 🔍 Verification

```bash id="r4"
aws iam list-attached-role-policies --role-name iamrole_rose
```

👉 Ensure:

* `iampolicy_rose` is attached

---

## 💡 Key Learning

* IAM roles are used by AWS services (like EC2) to access resources
* No need to store credentials inside instances
* Trust policy defines **who can assume the role**
* Roles improve security and scalability

---

## ⚠️ Challenges Faced

* Understanding trust policy structure
* Difference between users and roles
* Attaching correct policy ARN

---

## 🧩 Summary

Successfully created IAM role `iamrole_rose` for EC2 and attached policy `iampolicy_rose`, enabling secure access without using credentials.

---

---

# 🚀 Pro Tip

👉 Interview question:
**Why use IAM roles instead of access keys?**

👉 Answer:
“Roles provide temporary credentials and are more secure than hardcoding access keys.”

---
