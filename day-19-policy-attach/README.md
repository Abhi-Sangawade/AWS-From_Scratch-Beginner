---

# 📅 Day 19: Attach IAM Policy to IAM User

---

## 🧠 Task

Attach the IAM policy `iampolicy_siva` to the IAM user `iamuser_siva`.

---

## 🎯 Objective

* Understand how to attach policies to IAM users
* Learn how permissions are granted in AWS
* Practice IAM access control

---

## ☁️ AWS Details

* Service: AWS IAM
* Region: Global (IAM is not region-specific)
* User Name: `iamuser_siva`
* Policy Name: `iampolicy_siva`

---

## 🚀 Steps to Execute (AWS Console)

### 1. Login to AWS Console

* Use provided credentials

---

### 2. Navigate to IAM

* Search for **IAM**
* Open IAM Dashboard

---

### 3. Select User

* Click **Users**
* Select user: `iamuser_siva`

---

### 4. Attach Policy

* Go to **Permissions** tab
* Click **Add permissions**
* Choose **Attach policies directly**
* Select policy → `iampolicy_siva`
* Click **Add permissions**

---

## 💻 AWS CLI Method

### ✅ Step 1: Attach Policy to User

```bash id="u1"
aws iam attach-user-policy \
  --user-name iamuser_siva \
  --policy-arn arn:aws:iam::<account-id>:policy/iampolicy_siva
```

👉 Replace `<account-id>` with your AWS account ID

---

## 🔍 Verification

```bash id="u2"
aws iam list-attached-user-policies --user-name iamuser_siva
```

👉 Ensure:

* `iampolicy_siva` is listed

---

## 💡 Key Learning

* IAM policies define permissions
* Policies must be attached to users/groups/roles to take effect
* ARN (Amazon Resource Name) uniquely identifies resources
* IAM follows least privilege principle

---

## ⚠️ Challenges Faced

* Finding correct policy ARN
* Understanding how policies are linked to users
* Difference between inline and managed policies

---

## 🧩 Summary

Successfully attached IAM policy `iampolicy_siva` to user `iamuser_siva`, enabling controlled access to AWS resources.

---

---

# 🚀 Pro Tip

👉 Interview question:
**How are permissions assigned in AWS IAM?**

👉 Answer:
“Permissions are assigned by attaching policies to users, groups, or roles.”

---
