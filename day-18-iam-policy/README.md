---

# 📅 Day 18: Create Read-Only IAM Policy for EC2 Console Access

---

## 🧠 Task

Create an IAM policy named `iampolicy_kareem` that provides **read-only access** to the EC2 console (instances, AMIs, snapshots).

---

## 🎯 Objective

* Understand IAM policies and permissions
* Create custom policies using JSON
* Grant least-privilege access (read-only)

---

## ☁️ AWS Details

* Service: AWS IAM
* Region: Global (IAM is not region-specific)
* Policy Name: `iampolicy_kareem`
* Access Type: Read-only for EC2

---

## 🚀 Steps to Execute (AWS Console)

### 1. Login to AWS Console

* Use provided credentials

---

### 2. Navigate to IAM

* Search for **IAM**
* Open IAM Dashboard

---

### 3. Create Policy

* Click **Policies → Create policy**
* Choose **JSON** tab

---

### 4. Add Policy Document

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:Describe*"
      ],
      "Resource": "*"
    }
  ]
}
```

---

### 5. Review and Create

* Policy Name → `iampolicy_kareem`
* Click **Create policy**

---

## 💻 AWS CLI Method

### ✅ Create Policy

```bash id="p1"
aws iam create-policy \
  --policy-name iampolicy_kareem \
  --policy-document '{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [
          "ec2:Describe*"
        ],
        "Resource": "*"
      }
    ]
  }'
```

---

## 🔍 Verification

```bash id="p2"
aws iam list-policies --query "Policies[*].PolicyName" --output text
```

👉 Ensure:

* `iampolicy_kareem` is listed

---

## 💡 Key Learning

* IAM policies define **what actions are allowed or denied**
* `ec2:Describe*` provides read-only access
* Policies follow JSON format
* Always follow **least privilege principle**

---

## ⚠️ Challenges Faced

* Writing correct JSON policy syntax
* Understanding EC2 permissions (`Describe*`)
* Difference between managed and custom policies

---

## 🧩 Summary

Successfully created IAM policy `iampolicy_kareem` to provide read-only access to EC2 resources, following best practices for secure access control.

---

---

# 🚀 Pro Tip

👉 Interview question:
**What is the difference between managed and inline policies?**

👉 Answer:

* Managed → reusable across users/groups
* Inline → attached to a single entity only

---
