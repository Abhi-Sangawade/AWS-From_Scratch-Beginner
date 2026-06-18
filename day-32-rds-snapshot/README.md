---

# 📅 Day 32: Snapshot and Restoration of an RDS Instance

---

## 🧠 Task

* Take snapshot of RDS instance → `datacenter-rds`
* Snapshot name → `datacenter-snapshot`
* Restore snapshot to new instance → `datacenter-snapshot-restore`
* Instance class → `db.t3.micro`
* Ensure restored instance is in **Available** state

---

## 🎯 Objective

* Understand RDS backup strategies
* Learn snapshot creation and restoration
* Validate data recovery process
* Prepare for safe database upgrades

---

## ☁️ AWS Details

* Service: Amazon RDS
* Region: us-east-1
* Source DB: `datacenter-rds`
* Snapshot: `datacenter-snapshot`
* Restored DB: `datacenter-snapshot-restore`
* Instance Type: db.t3.micro

---

# 🚀 Steps to Execute (AWS Console)

---

# 🟢 PART 1: Take RDS Snapshot

---

## 🔹 Step 1: Navigate to RDS

1. Open AWS Console
2. Go to **RDS → Databases**

---

## 🔹 Step 2: Ensure DB is Available

* Select → `datacenter-rds`
* Status must be:

```text
Available
```

---

## 🔹 Step 3: Create Snapshot

1. Select DB → `datacenter-rds`
2. Click **Actions → Take snapshot**
3. Snapshot name → `datacenter-snapshot`
4. Click **Take snapshot**

---

## 🔹 Step 4: Wait for Completion

* Go to **RDS → Snapshots**
* Status should be:

```text
Available
```

---

# 🟡 PART 2: Restore Snapshot

---

## 🔹 Step 5: Restore Snapshot

1. Select snapshot → `datacenter-snapshot`
2. Click **Actions → Restore snapshot**

---

## 🔹 Step 6: Configure Restored Instance

* DB instance identifier → `datacenter-snapshot-restore`
* Instance class → `db.t3.micro`

👉 Keep remaining settings default

---

## 🔹 Step 7: Launch Restored Instance

* Click **Restore DB instance**

---

# 🔍 PART 3: Verification

---

## 🔹 Step 8: Check Restored Instance

* Go to **RDS → Databases**
* Select → `datacenter-snapshot-restore`

Status must be:

```text
Available
```

---

## 🔹 Step 9: Validate Setup

* Ensure instance class → `db.t3.micro`
* Ensure DB is accessible within VPC

---

# 💡 Key Learning

* Snapshots act as backups for RDS
* Restoration helps in testing and disaster recovery
* Snapshots are point-in-time copies
* Restored DB can be used for staging/testing

---

# ⚠️ Challenges Faced

* Waiting for snapshot to complete
* Understanding restore configurations
* Ensuring correct instance type

---

# 🔧 Fix / Learning

* Verified snapshot status before restore
* Selected correct instance class
* Monitored DB status after restore

---

# 🧩 Summary

Successfully created snapshot `datacenter-snapshot` from RDS instance `datacenter-rds` and restored it to a new instance `datacenter-snapshot-restore`, ensuring it is available and configured with `db.t3.micro`.

---
