---

# 📅 Day 17: Create IAM Group

---

## 🧠 Task

Create an IAM group named `iamgroup_siva` in AWS.

---

## 🎯 Objective

* Understand IAM group creation
* Learn how to organize users using groups
* Improve access management using IAM

---

## ☁️ AWS Details

* Service: AWS IAM
* Region: Global (IAM is not region-specific)
* Group Name: `iamgroup_siva`

---

## 🚀 Steps to Execute (AWS Console)

### 1. Login to AWS Console

* Use provided credentials

---

### 2. Navigate to IAM

* Search for **IAM**
* Open IAM Dashboard

---

### 3. Create Group

* Click **User groups → Create group**
* Group name → `iamgroup_siva`

---

### 4. Attach Permissions (Optional)

* Attach policies if required
* Skip if not specified

---

### 5. Create Group

* Review details
* Click **Create group**

---

## 💻 AWS CLI Method

### ✅ Create IAM Group

```bash id="g1"
aws iam create-group --group-name iamgroup_siva
```

---

## 🔍 Verification

```bash id="g2"
aws iam get-group --group-name iamgroup_siva
```

👉 Expected:

* Group details displayed successfully

---

## 💡 Key Learning

* IAM groups help manage multiple users efficiently
* Permissions can be assigned at group level
* IAM simplifies access control at scale

---

## ⚠️ Challenges Faced

* Understanding when to use groups vs individual user permissions
* IAM being a global service

---

## 🧩 Summary

Successfully created IAM group `iamgroup_siva`, improving user organization and access management in AWS.

---

---

# 🚀 Pro Tip

👉 Interview question:
**Why use IAM groups?**

👉 Answer:
“To assign permissions to multiple users at once instead of individually, improving scalability and management.”

---
