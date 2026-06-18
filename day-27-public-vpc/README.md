# 📅 Day 27: Configuring a Public VPC with an EC2 Instance for Internet Access

---

## 🧠 Task

* Create a VPC → `devops-pub-vpc`
* Create a public subnet → `devops-pub-subnet`
* Enable auto-assign public IP
* Launch EC2 instance → `devops-pub-ec2`
* Allow SSH (port 22) from internet

---

## 🎯 Objective

* Understand VPC networking basics
* Configure public subnet with internet access
* Launch EC2 in custom VPC
* Enable SSH access securely

---

## ☁️ AWS Details

* Service: VPC + EC2
* Region: us-east-1
* VPC: `devops-pub-vpc`
* Subnet: `devops-pub-subnet`
* Instance: `devops-pub-ec2`
* Instance Type: t2.micro

---

# 🚀 Steps to Execute (AWS Console)

---

## 🔹 Step 1: Create VPC

1. Go to **VPC → Your VPCs**
2. Click **Create VPC**
3. Name → `devops-pub-vpc`
4. IPv4 CIDR → `10.0.0.0/16`
5. Create VPC

---

## 🔹 Step 2: Create Subnet

1. Go to **VPC → Subnets**
2. Click **Create Subnet**
3. Name → `devops-pub-subnet`
4. Select VPC → `devops-pub-vpc`
5. CIDR → `10.0.1.0/24`
6. Create

---

## 🔹 Step 3: Enable Auto Public IP

1. Select subnet → `devops-pub-subnet`
2. Click **Actions → Edit subnet settings**
3. Enable:

   * ✔ Auto-assign public IPv4 address
4. Save

---

## 🔹 Step 4: Create Internet Gateway

1. Go to **VPC → Internet Gateways**
2. Click **Create Internet Gateway**
3. Name → `devops-igw`
4. Create

---

## 🔹 Step 5: Attach Internet Gateway to VPC

1. Select IGW → `devops-igw`
2. Click **Attach to VPC**
3. Select → `devops-pub-vpc`
4. Attach

---

## 🔹 Step 6: Create Route Table

1. Go to **VPC → Route Tables**
2. Click **Create Route Table**
3. Name → `devops-rt`
4. VPC → `devops-pub-vpc`
5. Create

---

## 🔹 Step 7: Add Route to Internet

1. Select route table → `devops-rt`
2. Go to **Routes → Edit routes**
3. Add:

```text
Destination → 0.0.0.0/0
Target → devops-igw
```

4. Save

---

## 🔹 Step 8: Associate Subnet with Route Table

1. Go to **Subnet Associations**
2. Click **Edit associations**
3. Select → `devops-pub-subnet`
4. Save

---

## 🔹 Step 9: Launch EC2 Instance

1. Go to **EC2 → Launch Instance**
2. Name → `devops-pub-ec2`
3. AMI → Ubuntu
4. Instance Type → t2.micro
5. Network:

   * VPC → `devops-pub-vpc`
   * Subnet → `devops-pub-subnet`
6. Enable Public IP (auto assigned)

---

## 🔹 Step 10: Configure Security Group

Add inbound rule:

* Type → SSH
* Port → 22
* Source → `0.0.0.0/0`

---

## 🔍 Verification

---

## ✅ 1. Check Public IP

* Go to EC2 → Instance
* Verify Public IP is assigned

---

## ✅ 2. SSH Access

```bash
ssh -i <key.pem> ubuntu@<public-ip>
```

👉 Should connect successfully

---

## 💡 Key Learning

* Public VPC requires Internet Gateway + Route Table
* Subnet must allow public IP assignment
* Route `0.0.0.0/0` enables internet access
* Security groups control SSH access

---

## ⚠️ Challenges Faced

* Forgetting Internet Gateway
* Missing route table configuration
* Public IP not assigned
* SSH port not open

---

## 🔧 Fix / Learning

* Attached IGW to VPC
* Added route to `0.0.0.0/0`
* Enabled auto public IP in subnet
* Allowed SSH in security group

---

## 🧩 Summary

Successfully created a public VPC `devops-pub-vpc`, configured subnet `devops-pub-subnet` with internet access, and launched EC2 instance `devops-pub-ec2` accessible via SSH.

---
