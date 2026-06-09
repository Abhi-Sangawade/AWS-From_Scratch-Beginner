# 📅 Day 4: Enable Versioning for S3 Bucket

---

## 🧠 Task

Enable versioning for the S3 bucket `abhsiehk-s3-0618` to ensure data protection and recovery.

---

## 🎯 Objective

* Enable **versioning** on an existing S3 bucket
* Protect data from accidental deletion or overwrites
* Ensure recovery of previous object versions

---

## ☁️ AWS Details

* Service: Amazon S3
* Region: us-east-1
* Bucket Name: `nautilus-s3-10273`

---

## 🚀 Steps to Execute (AWS Console)

### 1. Login to AWS Console

* Use provided credentials
* Select region: **us-east-1**

### 2. Navigate to S3

* Search for **S3**
* Open dashboard

### 3. Select Bucket

* Click: `nautilus-s3-10273`

### 4. Enable Versioning

* Go to **Properties**
* Find **Bucket Versioning**
* Click **Edit → Enable → Save**

---

## 💻 AWS CLI Method (Corrected)

### ✅ Single-line command (recommended)

```bash
aws s3api put-bucket-versioning --bucket nautilus-s3-10273 --versioning-configuration Status=Enabled
```

### ✅ Multi-line command (proper format)

```bash
aws s3api put-bucket-versioning \
  --bucket nautilus-s3-10273 \
  --versioning-configuration Status=Enabled
```

> Note: When using `\`, it must be at the end of the line with the next argument on a new line.

---

## 🔍 Verification

```bash
aws s3api get-bucket-versioning --bucket nautilus-s3-10273
```

Expected Output:

```json
{
  "Status": "Enabled"
}
```

---

## 💡 Key Learning

* S3 versioning keeps multiple versions of objects
* Helps recover deleted or overwritten files
* Critical for backup and data protection

---

## ⚠️ Challenges Faced

* Incorrect CLI formatting using `\`
* Understanding proper command structure

---

## 🧩 Summary

Enabled versioning on an S3 bucket using both console and CLI, ensuring improved data durability and recovery.

---
