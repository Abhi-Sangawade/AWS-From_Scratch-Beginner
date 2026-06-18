---

# 📅 Day 45: Configure NAT Gateway for Internet Access in a Private VPC

---

## 🧠 Task

* Create public subnet → `devops-pub-subnet`
* Create and attach Internet Gateway
* Create public route table → `devops-pub-rt`
* Allocate Elastic IP
* Create NAT Gateway → `devops-natgw`
* Update private route table to use NAT Gateway
* Verify internet access from private EC2 instance

---

## 🎯 Objective

* Enable internet access for private EC2 instances
* Understand NAT Gateway architecture
* Configure routing between public and private subnets
* Verify outbound internet connectivity securely

---

## ☁️ AWS Details

* Service: VPC + NAT Gateway + EC2 + S3
* Region: us-east-1
* VPC: `devops-priv-vpc`
* Private Subnet: `devops-priv-subnet`
* Public Subnet: `devops-pub-subnet`
* NAT Gateway: `devops-natgw`
* EC2 Instance: `devops-priv-ec2`
* S3 Bucket: `devops-nat-251816030`

---

# 🚀 Architecture Overview

```text
Private EC2 → NAT Gateway → Internet Gateway → Internet/S3
```

---

# 🟢 PART 1: Create Public Subnet

---

## 🔹 Step 1: Create Public Subnet

1. Go to **VPC → Subnets → Create subnet**
2. Select VPC → `devops-priv-vpc`
3. Subnet name → `devops-pub-subnet`
4. Choose Availability Zone
5. Assign CIDR block
6. Enable:

```text
Auto-assign public IPv4 address
```

---

# 🟡 PART 2: Create Internet Gateway

---

## 🔹 Step 2: Create IGW

1. Go to **VPC → Internet Gateways → Create**
2. Name → `devops-igw`

---

## 🔹 Step 3: Attach IGW to VPC

* Select IGW
* Actions → Attach to VPC
* Choose → `devops-priv-vpc`

---

# 🔵 PART 3: Configure Public Route Table

---

## 🔹 Step 4: Create Route Table

1. Go to **VPC → Route Tables → Create**
2. Name → `devops-pub-rt`
3. Select VPC → `devops-priv-vpc`

---

## 🔹 Step 5: Add Internet Route

Routes:

```text
Destination → 0.0.0.0/0
Target → Internet Gateway
```

---

## 🔹 Step 6: Associate Public Subnet

* Subnet associations → Edit
* Select → `devops-pub-subnet`

---

# 🟣 PART 4: Create NAT Gateway

---

## 🔹 Step 7: Allocate Elastic IP

1. Go to **EC2 → Elastic IPs → Allocate**
2. Copy allocated Elastic IP

---

## 🔹 Step 8: Create NAT Gateway

1. Go to **VPC → NAT Gateways → Create**
2. Name → `devops-natgw`
3. Subnet → `devops-pub-subnet`
4. Elastic IP → Select allocated EIP
5. Click **Create NAT Gateway**

---

## 🔹 Step 9: Wait for NAT Gateway

Status should become:

```text
Available
```

---

# 🔴 PART 5: Update Private Route Table

---

## 🔹 Step 10: Identify Private Route Table

* Find route table associated with:

  * `devops-priv-subnet`

---

## 🔹 Step 11: Add NAT Route

Add route:

```text
Destination → 0.0.0.0/0
Target → devops-natgw
```

---

# 🔍 PART 6: Verification

---

## 🔹 Step 12: Wait for Cron Job

The EC2 instance automatically uploads:

```text
datacenter-test.txt
```

to:

```text
devops-nat-251816030
```

⏳ Wait 2–3 minutes

---

## 🔹 Step 13: Verify in S3

Go to:

```text
S3 → devops-nat-251816030
```

Expected:

```text
datacenter-test.txt
```

---

# 💡 Key Learning

* NAT Gateway allows private instances to access the internet securely
* Private instances remain inaccessible from the internet directly
* Public subnet requires IGW + public route table
* Proper routing is critical for connectivity

---

# ⚠️ Challenges Faced

* NAT Gateway stuck in pending ❌
* Wrong route table updated ❌
* Public subnet missing IGW route ❌
* No public IP assignment in subnet ❌

---

# 🔧 Fix / Learning

* Ensured NAT Gateway is created in public subnet
* Added correct route `0.0.0.0/0 → NAT Gateway`
* Verified IGW attachment and public routing
* Waited for cron job execution before verification

---

# 🧩 Summary

Successfully configured NAT Gateway `devops-natgw` to provide internet access for EC2 instance `devops-priv-ec2` running in private subnet `devops-priv-subnet`. Verified outbound connectivity through successful file upload to S3 bucket `devops-nat-251816030`.

---
