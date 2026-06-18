---

# 📅 Day 31: Configuring a Private RDS Instance for Application Development

---

## 🧠 Task

* Create a private RDS instance → `nautilus-rds`
* Engine → MySQL (version 8.4.x)
* Instance type → `db.t3.micro` (Free Tier)
* Enable storage autoscaling (max 50 GB)
* Ensure instance is in **Available** state

---

## 🎯 Objective

* Understand Amazon RDS provisioning
* Configure a private database instance
* Learn Free Tier optimization
* Enable autoscaling for storage

---

## ☁️ AWS Details

* Service: Amazon RDS
* Region: us-east-1
* DB Instance: `nautilus-rds`
* Engine: MySQL 8.4.x
* Instance Type: db.t3.micro
* Storage: General Purpose (Free Tier)
* Autoscaling: Enabled (50 GB max)
* Access: Private (No public access)

---

# 🚀 Steps to Execute (AWS Console)

---

## 🔹 Step 1: Navigate to RDS

1. Open AWS Console
2. Search → **RDS**
3. Click **Create database**

---

## 🔹 Step 2: Choose Database Creation Method

* Select → **Standard create**

---

## 🔹 Step 3: Engine Configuration

* Engine type → **MySQL**
* Version → **8.4.x**

---

## 🔹 Step 4: Templates

* Select → **Free tier**

---

## 🔹 Step 5: Settings

* DB Instance Identifier → `nautilus-rds`
* Master username → `admin` (or default)
* Password → Set securely

---

## 🔹 Step 6: Instance Configuration

* DB Instance Class → `db.t3.micro`

---

## 🔹 Step 7: Storage Configuration

* Storage type → General Purpose (gp2/gp3)
* Enable **Storage autoscaling**
* Max storage threshold → **50 GB**

---

## 🔹 Step 8: Connectivity (IMPORTANT)

* VPC → Default or specified VPC
* Subnet group → Default
* Public access → ❌ **No (Private Instance)**
* Security Group → Default or custom

---

## 🔹 Step 9: Additional Configuration

* Leave as default

---

## 🔹 Step 10: Create Database

* Click **Create database**

---

# 🔍 Verification

---

## ✅ 1. Check Instance Status

* Go to **RDS → Databases**
* Status should be:

```text
Available
```

---

## ✅ 2. Verify Connectivity Type

* Public access → **No** ✅

---

## 💡 Key Learning

* RDS simplifies database management
* Private instances enhance security
* Free Tier helps in cost optimization
* Storage autoscaling prevents storage issues

---

## ⚠️ Challenges Faced

* Selecting correct engine version
* Understanding public vs private access
* Configuring autoscaling properly

---

## 🔧 Fix / Learning

* Chose Free Tier template for cost control
* Disabled public access for security
* Enabled autoscaling for flexibility

---

## 🧩 Summary

Successfully provisioned a private RDS instance `nautilus-rds` using MySQL 8.4.x on Free Tier (`db.t3.micro`), with storage autoscaling enabled and instance verified in available state.

---
