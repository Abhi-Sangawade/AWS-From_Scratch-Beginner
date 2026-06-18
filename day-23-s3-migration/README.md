---

# 📅 Day 23: Data Migration Between S3 Buckets Using AWS CLI

---

## 🧠 Task

* Create a new private S3 bucket: `datacenter-sync-30149`
* Migrate all data from existing bucket `datacenter-s3-10751`
* Ensure both buckets have identical data
* Use AWS CLI for all operations

---

## 🎯 Objective

* Understand S3 bucket creation using CLI
* Learn data migration using `aws s3 cp` and `aws s3 sync`
* Ensure data consistency after migration

---

## ☁️ AWS Details

* Service: Amazon S3
* Region: us-east-1
* Source Bucket: `datacenter-s3-10751`
* Destination Bucket: `datacenter-sync-30149`

---

## 🚀 Steps to Execute

---

### 🔹 Step 1: Create S3 Bucket

```bash id="d23-1"
aws s3api create-bucket \
--bucket datacenter-sync-30149 \
--region us-east-1
```

---

### 🔹 Step 2: Verify Bucket Creation

```bash id="d23-2"
aws s3 ls
```

---

### 🔹 Step 3: Migrate Data (Recommended: sync)

```bash id="d23-3"
aws s3 sync s3://datacenter-s3-10751 s3://datacenter-sync-30149
```

👉 Why `sync`?

* Copies only missing/new files
* Efficient for large datasets
* Maintains structure

---

### 🔹 Alternative: Copy Entire Data

```bash id="d23-4"
aws s3 cp s3://datacenter-s3-10751 s3://datacenter-sync-30149 --recursive
```

---

## 🔍 Verification Steps

### ✅ 1. List Objects in Both Buckets

```bash id="d23-5"
aws s3 ls s3://datacenter-s3-10751 --recursive
aws s3 ls s3://datacenter-sync-30149 --recursive
```

---

### ✅ 2. Compare Object Count

```bash id="d23-6"
aws s3 ls s3://datacenter-s3-10751 --recursive | wc -l
aws s3 ls s3://datacenter-sync-30149 --recursive | wc -l
```

👉 Counts should match

---

### ✅ 3. Re-run Sync (Idempotent Check)

```bash id="d23-7"
aws s3 sync s3://datacenter-s3-10751 s3://datacenter-sync-30149
```

👉 If no output → data is already in sync ✅

---

## 💡 Key Learning

* `aws s3 sync` is preferred for migrations
* S3 operations are region-aware
* Data consistency must be verified, not assumed
* CLI is powerful for automation

---

## ⚠️ Challenges Faced

* Confusion between `cp` vs `sync`
* Ensuring complete data transfer
* Verifying data consistency

---

## 🔧 Fix / Learning

* Used `sync` instead of `cp` for efficient migration
* Verified using object count comparison
* Re-ran sync to confirm no pending changes

---

## 🧩 Summary

Successfully created a new S3 bucket `datacenter-sync-30149` and migrated all data from `datacenter-s3-10751` using AWS CLI, ensuring complete data consistency through verification steps.

---
