---

# 📅 Day 15: Create Volume Snapshot

---

## 🧠 Task

Create a snapshot of the EBS volume `xfusion-vol` in the **us-east-1** region.

* Snapshot Name: `xfusion-vol-ss`
* Description: `xfusion Snapshot`
* Ensure snapshot status is **completed** before submission

---

## 🎯 Objective

* Understand EBS snapshot functionality
* Create backups of volumes
* Verify snapshot completion status

---

## ☁️ AWS Details

* Service: Amazon EC2 (EBS Snapshots)
* Region: us-east-1
* Volume Name: `xfusion-vol`
* Snapshot Name: `xfusion-vol-ss`

---

## 🚀 Steps to Execute (AWS Console)

### 1. Login to AWS Console

* Use provided credentials
* Select region: **us-east-1**

---

### 2. Navigate to EC2

* Search for **EC2**
* Open EC2 Dashboard

---

### 3. Go to Volumes

* Click **Volumes** under Elastic Block Store

---

### 4. Select Volume

* Find: `xfusion-vol`
* Select the volume

---

### 5. Create Snapshot

* Click **Actions → Create Snapshot**
* Description → `xfusion Snapshot`
* Add Tag:

  * Key → `Name`
  * Value → `xfusion-vol-ss`
* Click **Create Snapshot**

---

### 6. Wait for Completion

* Go to **Snapshots** section
* Check status → `completed`

---

## 💻 AWS CLI Method

### ✅ Step 1: Get Volume ID

```bash id="s1"
aws ec2 describe-volumes --query "Volumes[*].[VolumeId,Size]" --output table
```

👉 Identify the volume corresponding to `xfusion-vol`

---

### ⚠️ Important Note

❌ Do NOT rely only on Name tag:

* Volume may not always have a Name tag
* Verify using VolumeId

---

### ✅ Step 2: Create Snapshot

```bash id="s2"
aws ec2 create-snapshot \
  --volume-id <volume-id> \
  --description "xfusion Snapshot" \
  --tag-specifications 'ResourceType=snapshot,Tags=[{Key=Name,Value=xfusion-vol-ss}]'
```

---

### ✅ Step 3: Verify Snapshot Status

```bash id="s3"
aws ec2 describe-snapshots --owner-ids self --query "Snapshots[*].[SnapshotId,State,Description]" --output table
```

👉 Ensure:

* Description → `xfusion Snapshot`
* State → `completed`

---

## 🔍 Important Notes

* Snapshot creation may take time depending on volume size
* Snapshots are incremental backups
* Always verify status before submission

---

## 💡 Key Learning

* EBS snapshots are used for backup and recovery
* Stored in Amazon S3 (managed by AWS)
* Can be used to recreate volumes
* Important for disaster recovery

---

## ⚠️ Challenges Faced

* Identifying correct volume
* Waiting for snapshot to reach `completed` state
* Understanding snapshot behavior

---

## 🧩 Summary

Successfully created a snapshot `xfusion-vol-ss` from volume `xfusion-vol`, ensuring backup creation and verifying completion status.

---

---

# 🚀 Pro Tip

👉 Interview question:
**Are EBS snapshots full or incremental?**

👉 Answer:
“They are incremental—only changes since the last snapshot are stored.”

---
