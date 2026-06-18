# Day 49: Centralized Audit Logging with VPC Peering

---

## 📌 Task

The Nautilus DevOps team needs to build a secure and scalable centralized audit logging architecture in AWS.

The objective is to collect logs from a private EC2 instance, securely transfer them to a public EC2 instance through VPC Peering, and upload them into a private S3 bucket.

Requirements:

* Create public VPC → `xfusion-pub-vpc`
* Create public subnet → `xfusion-pub-subnet`
* Create route table → `xfusion-pub-rt`
* Attach Internet Gateway
* Launch EC2 → `xfusion-pub-ec2`
* Create IAM role → `xfusion-s3-role`
* Create private S3 bucket → `xfusion-s3-logs-1353`
* Configure VPC Peering → `xfusion-vpc-peering`
* Configure route tables
* Configure cron jobs:

  * Private EC2 → Public EC2
  * Public EC2 → S3 Bucket

Final file path:

```text
s3://xfusion-s3-logs-1353/xfusion-priv-vpc/boot/boots.log
```

---

## 🎯 Objective

This task demonstrates:

* Secure VPC communication
* VPC Peering
* IAM Role-based authentication
* Cross-VPC file transfer
* S3 centralized logging
* Cron automation
* Troubleshooting networking and permissions

---

## ☁️ AWS Services Used

| Service     | Resource             |
| ----------- | -------------------- |
| VPC         | xfusion-pub-vpc      |
| EC2         | xfusion-pub-ec2      |
| IAM         | xfusion-s3-role      |
| S3          | xfusion-s3-logs-1353 |
| VPC Peering | xfusion-vpc-peering  |
| Cron        | Automated transfers  |

---

## 🏗 Architecture

```text
Private VPC
(xfusion-priv-vpc)
        ↓
Private EC2
(xfusion-priv-ec2)
        ↓
SCP via VPC Peering
        ↓
Public EC2
(xfusion-pub-ec2)
        ↓
IAM Role Authentication
        ↓
Private S3 Bucket
(xfusion-s3-logs-1353)
```

---

# 🚀 Implementation Steps

## Step 1: Create Public VPC

```text
AWS Console
→ VPC
→ Create VPC
```

| Parameter | Value           |
| --------- | --------------- |
| Name      | xfusion-pub-vpc |
| CIDR      | 10.1.0.0/16     |

---

## Step 2: Create Public Subnet

```text
VPC
→ Subnets
→ Create Subnet
```

| Parameter | Value              |
| --------- | ------------------ |
| Name      | xfusion-pub-subnet |
| CIDR      | 10.1.1.0/24        |

Enable:

```text
Auto Assign Public IPv4
```

---

## Step 3: Create Route Table + Internet Gateway

Create:

```text
xfusion-pub-rt
xfusion-igw
```

Attach:

```text
xfusion-igw → xfusion-pub-vpc
```

Route:

| Destination | Target |
| ----------- | ------ |
| 0.0.0.0/0   | IGW    |

---

## Step 4: Launch Public EC2

Configuration:

| Parameter | Value           |
| --------- | --------------- |
| Name      | xfusion-pub-ec2 |
| AMI       | Ubuntu          |
| Type      | t2.micro        |
| Key Pair  | xfusion-key     |

Security Group:

```text
SSH : 22
```

---

## Step 5: Create S3 Bucket

```text
Bucket Name:
xfusion-s3-logs-1353
```

Keep:

```text
Private Bucket
Block Public Access Enabled
```

---

## Step 6: Create IAM Policy

```json
{
 "Version":"2012-10-17",
 "Statement":[
   {
     "Effect":"Allow",
     "Action":[
       "s3:PutObject",
       "s3:GetObject",
       "s3:ListBucket"
     ],
     "Resource":[
       "arn:aws:s3:::xfusion-s3-logs-1353",
       "arn:aws:s3:::xfusion-s3-logs-1353/*"
     ]
   }
 ]
}
```

Save as:

```text
xfusion-s3-policy
```

---

## Step 7: Create IAM Role

```text
Role Name:
xfusion-s3-role
```

Trusted entity:

```text
EC2
```

Attach:

```text
xfusion-s3-policy
```

Attach role to:

```text
xfusion-pub-ec2
```

---

## Step 8: Configure VPC Peering

Create:

```text
xfusion-vpc-peering
```

Requester:

```text
xfusion-pub-vpc
```

Accepter:

```text
xfusion-priv-vpc
```

Accept request.

---

## Step 9: Update Route Tables

Public route:

```text
Private-VPC-CIDR → xfusion-vpc-peering
```

Private route:

```text
Public-VPC-CIDR → xfusion-vpc-peering
```

---

## Step 10: Configure Private → Public Transfer

SSH:

```bash
ssh -i /root/.ssh/xfusion-key.pem ubuntu@<private-ip>
```

Generate key:

```bash
ssh-keygen
```

Copy key:

```bash
ssh-copy-id ubuntu@<public-private-ip>
```

Cron:

```bash
crontab -e
```

Add:

```bash
*/1 * * * * scp -o StrictHostKeyChecking=no /var/log/boots.log ubuntu@<public-private-ip>:/tmp/boots.log
```

---

## Step 11: Configure Public → S3 Transfer

SSH:

```bash
ssh -i /root/.ssh/xfusion-key.pem ubuntu@<public-ip>
```

Install AWS CLI if needed:

```bash
sudo apt update
sudo apt install awscli -y
```

Cron:

```bash
crontab -e
```

Add:

```bash
*/1 * * * * /usr/bin/aws s3 cp /tmp/boots.log s3://xfusion-s3-logs-1353/xfusion-priv-vpc/boot/boots.log
```

Restart:

```bash
sudo systemctl restart cron
```

---

# ⚠️ Issues Faced During Task Execution

### 1. SSH connection hanging to Public EC2

Issue:

```text
Connecting to <public-ip> port 22
```

Root cause:

* Missing SSH rule in SG
* Internet Gateway not attached
* Public route missing
* Public subnet misconfiguration

---

### 2. VPC Peering validation failure

Error:

```text
VPC peering 'xfusion-vpc-peering' not found
```

Root cause:

* Missing Name tag

---

### 3. Route configuration issue

Issue:

```text
Private and Public VPC could not communicate
```

Root cause:

Missing routes in:

* `xfusion-priv-rt`
* `xfusion-pub-rt`

---

### 4. SSH key issues

Errors:

```text
No such file or directory
Permission denied
```

Fix:

```bash
chmod 400 xfusion-key.pem
```

---

### 5. Wrong instance/IP confusion

Result:

```text
boots.log not found
```

Root cause:

Commands executed on incorrect EC2

---

### 6. SCP transfer hanging

Root cause:

* Wrong target private IP
* Missing SG rules
* SSH not configured between VPCs

---

### 7. AWS CLI missing

Error:

```text
aws: command not found
```

Fix:

```bash
sudo apt install awscli -y
```

---

### 8. S3 permission limitations

Errors:

```text
AccessDenied
403 Forbidden
```

Fix:

Added:

```text
s3:GetObject
s3:ListBucket
```

---

### 9. Cron jobs missing

Error:

```text
no crontab for ubuntu
```

Fix:

Created cron entries on:

* Private EC2
* Public EC2

---

### 10. Suspended background jobs

Issue:

```text
[1]- Stopped scp
[2]+ Stopped ssh
```

Fix:

```bash
jobs -l
kill <PID>
```

---

## 💡 Key Learning

* VPC Peering requires proper route configuration on both sides
* Security Groups silently block SSH traffic
* IAM roles avoid hardcoded credentials
* Cron jobs require explicit command paths
* Debugging networking takes most of the effort in cloud setups

---

## ✅ Final Result

Successfully configured:

✅ Public VPC and networking
✅ Internet Gateway and route tables
✅ Public EC2 instance
✅ IAM role and S3 bucket
✅ VPC peering and routes
✅ Private → Public log transfer
✅ Public → S3 upload
✅ Automated cron jobs

Verified output:

```text
s3://xfusion-s3-logs-1353/xfusion-priv-vpc/boot/boots.log
```

🚀 Day 49 completed — successfully built centralized audit logging with VPC Peering, EC2, IAM roles, S3, and cron automation while troubleshooting networking, SSH, IAM, and file transfer issues.
