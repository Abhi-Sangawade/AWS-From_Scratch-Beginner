---

# 📅 Day 16: Create IAM User

---

## 🧠 Task

Create an IAM user named `iamuser_kirsty` in AWS.

---

## 🎯 Objective

* Understand AWS Identity and Access Management (IAM)
* Create and manage IAM users
* Learn basics of access control in AWS

---

## ☁️ AWS Details

* Service: AWS IAM
* Region: Global (IAM is not region-specific)
* User Name: `iamuser_kirsty`

---

## 🚀 Steps to Execute (AWS Console)

### 1. Login to AWS Console

* Use provided credentials

---

### 2. Navigate to IAM

* Search for **IAM**
* Open IAM Dashboard

---

### 3. Create User

* Click **Users → Create user**
* User name → `iamuser_kirsty`

---

### 4. Set Permissions

* Choose **Attach policies directly** (or skip for now if not required)

---

### 5. Create User

* Review details
* Click **Create user**

---

## 💻 AWS CLI Method

### ✅ Create IAM User

```bash id="i1"
aws iam create-user --user-name iamuser_kirsty
```

---

## 🔍 Verification

```bash id="i2"
aws iam get-user --user-name iamuser_kirsty
```

👉 Expected:

* User details displayed successfully

---

## 💡 Key Learning

* IAM is used to manage users, roles, and permissions in AWS
* IAM is a **global service** (not tied to a region)
* Users should follow least privilege principle

---

## ⚠️ Challenges Faced

* Understanding IAM is global (not region-based)
* Confusion around permissions and policies

---

## 🧩 Summary

Successfully created an IAM user `iamuser_kirsty`, taking the first step towards managing access and security in AWS.

---

---

# 🚀 Pro Tip

👉 Interview question:
**What is IAM in AWS?**

👉 Answer:
“IAM is a service that helps securely control access to AWS resources using users, roles, and policies.”

---
