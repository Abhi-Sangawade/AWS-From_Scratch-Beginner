# 📅 Day 37: Managing EC2 Access with S3 Role-based Permissions (Console Based)

---

## 🧠 Task

* Configure EC2 → `devops-ec2` to access S3
* Create SSH key for secure access
* Create private S3 bucket → `devops-s3-392284424672`
* Create IAM policy and role → `devops-role`
* Attach role to EC2
* Test upload and list operations

---

## 🎯 Objective

* Understand IAM Roles for EC2
* Enable secure access to S3 without access keys
* Configure role-based permissions
* Validate real-world EC2 ↔ S3 integration

---

## ☁️ AWS Details

* Service: EC2 + S3 + IAM
* Region: us-east-1
* EC2 Instance: `devops-ec2`
* S3 Bucket: `devops-s3-392284424672`
* IAM Role: `devops-role`

---

# 🚀 PART 1: Setup SSH Access

---

## 🔹 Step 1: Generate SSH Key (aws-client)

```bash
ssh-keygen -t rsa -f /root/.ssh/id_rsa -N ""
```

---

## 🔹 Step 2: Copy Public Key to EC2

```bash
ssh-copy-id root@<ec2-public-ip>
```

👉 OR manually on EC2

```bash
cat /root/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```

---

## 🔹 Step 3: Verify SSH Access

```bash
ssh root@<ec2-public-ip>
```

---

# 🟢 PART 2: Create Private S3 Bucket (Console)

---

## 🔹 Step 4: Create Bucket

1. Go to **S3 → Create bucket**
2. Bucket name → `devops-s3-392284424672`
3. Region → us-east-1
4. Keep **Block Public Access ON** ✅
5. Click **Create bucket**

---

## 🔹 Step 5: Verify Bucket

* Bucket should be private by default
* No public access enabled

---

# 🟡 PART 3: Create IAM Policy (Console)

---

## 🔹 Step 6: Create Policy

1. Go to **IAM → Policies → Create policy**
2. Choose **JSON** tab
3. Paste below policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:GetObject"
      ],
      "Resource": "arn:aws:s3:::devops-s3-392284424672/*"
    },
    {
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::devops-s3-392284424672"
    }
  ]
}
```

4. Click **Next → Name → `devops-s3-policy`**
5. Click **Create policy**

---

# 🔵 PART 4: Create IAM Role (Console)

---

## 🔹 Step 7: Create Role

1. Go to **IAM → Roles → Create role**
2. Trusted entity → **AWS Service**
3. Use case → **EC2**
4. Click **Next**

---

## 🔹 Step 8: Attach Policy

* Select → `devops-s3-policy`
* Click **Next**

---

## 🔹 Step 9: Name Role

* Role name → `devops-role`
* Click **Create role**

---

# 🟣 PART 5: Attach Role to EC2

---

## 🔹 Step 10: Attach IAM Role

1. Go to **EC2 → Instances**
2. Select → `devops-ec2`
3. Actions → Security → **Modify IAM role**
4. Select → `devops-role`
5. Click **Update IAM role**

---

# 🔍 PART 6: Test S3 Access

---

## 🔹 Step 11: SSH into EC2

```bash
ssh root@<ec2-public-ip>
```

---

## 🔹 Step 12: Upload File

```bash
echo "Test file" > test.txt
aws s3 cp test.txt s3://devops-s3-392284424672/
```

---

## 🔹 Step 13: List Files

```bash
aws s3 ls s3://devops-s3-392284424672/
```

---

## ✅ Expected Output

```text
test.txt
```

---

# 💡 Key Learning

* IAM Roles remove need for access keys
* EC2 securely accesses S3 via attached role
* Policies define fine-grained permissions
* Console-based setup is intuitive and visual

---

# ⚠️ Challenges Faced

* Confusion between IAM role and policy
* Forgetting to attach role to EC2
* Incorrect bucket ARN in policy

---

# 🔧 Fix / Learning

* Created correct JSON policy
* Attached role properly to EC2 instance
* Verified permissions using CLI commands

---

# 🧩 Summary

Successfully configured EC2 instance `devops-ec2` to access private S3 bucket `devops-s3-392284424672` using IAM role `devops-role`. Verified secure access by uploading and listing files without using access keys.

---
