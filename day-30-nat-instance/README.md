---

# 📅 Day 30: Enable Internet Access for Private EC2 using NAT Instance

---

## 🧠 Task

* Create public subnet → `datacenter-pub-subnet`
* Launch NAT Instance → `datacenter-nat-instance`
* Configure NAT using iptables
* Enable internet access for private EC2 → `datacenter-priv-ec2`
* Verify by uploading file to S3 → `datacenter-nat-5637`

---

## 🎯 Objective

* Understand private subnet internet access
* Learn NAT Instance setup (cost-effective alternative to NAT Gateway)
* Configure routing and IP forwarding
* Verify connectivity using real use case (S3 upload)

---

## ☁️ AWS Details

* VPC: `datacenter-priv-vpc`
* Private Subnet: `datacenter-priv-subnet`
* Public Subnet: `datacenter-pub-subnet`
* Private EC2: `datacenter-priv-ec2`
* NAT Instance: `datacenter-nat-instance`
* S3 Bucket: `datacenter-nat-5637`
* Region: us-east-1

---

# 🚀 Architecture Overview

```text id="natflow1"
Private EC2 → Route Table → NAT Instance → Internet → S3
```

---

# 🟢 PART 1: Create Public Subnet

---

## 🔹 Step 1: Create Subnet

* Go to **VPC → Subnets → Create subnet**
* Name → `datacenter-pub-subnet`
* VPC → `datacenter-priv-vpc`
* CIDR → `10.0.2.0/24`

---

## 🔹 Step 2: Enable Public IP

* Select subnet → Edit settings
* Enable:
  ✔ Auto-assign public IPv4

---

# 🟡 PART 2: Setup Internet Gateway

---

## 🔹 Step 3: Create & Attach IGW

* VPC → Internet Gateway
* Create → `datacenter-igw`
* Attach to VPC → `datacenter-priv-vpc`

---

## 🔹 Step 4: Route Table for Public Subnet

* Create route table → `datacenter-pub-rt`
* Add route:

```text id="natflow2"
0.0.0.0/0 → Internet Gateway
```

* Associate with → `datacenter-pub-subnet`

---

# 🔵 PART 3: Launch NAT Instance

---

## 🔹 Step 5: Launch Instance

* Name → `datacenter-nat-instance`
* AMI → Amazon Linux 2023
* Subnet → `datacenter-pub-subnet`
* Enable Public IP → YES

---

## 🔹 Step 6: Security Group

Create custom SG:

* Allow SSH → port 22
* Allow ALL traffic → from private subnet CIDR (10.0.0.0/16 or specific subnet)

---

## 🔹 Step 7: Disable Source/Destination Check

👉 VERY IMPORTANT

* EC2 → Select NAT instance
* Actions → Networking → Change source/destination check → Disable

---

# 🔴 PART 4: Configure NAT (Inside Instance)

---

## 🔹 Step 8: Connect to NAT Instance

```bash id="nat1"
ssh ec2-user@<public-ip>
```

---

## 🔹 Step 9: Install iptables (Required for AL2023)

```bash id="nat2"
sudo dnf install iptables-services -y
```

---

## 🔹 Step 10: Enable IP Forwarding

```bash id="nat3"
echo "net.ipv4.ip_forward = 1" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

---

## 🔹 Step 11: Configure NAT Rules

```bash id="nat4"
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```

---

## 🔹 Step 12: Save iptables Rules

```bash id="nat5"
sudo service iptables save
sudo systemctl enable iptables
```

---

# 🟣 PART 5: Update Private Route Table

---

## 🔹 Step 13: Modify Private Subnet Route

* Go to Route Table of private subnet
* Add:

```text id="natflow3"
0.0.0.0/0 → NAT Instance
```

---

# 🔍 PART 6: Verification

---

## ✅ Step 14: Check S3 Upload

* Go to S3 → `datacenter-nat-5637`

👉 Verify file:

```text id="natflow4"
datacenter-test.txt
```

---

## ✅ Step 15: Confirm Cron Working

* File should appear within 1 minute

---

# 💡 Key Learning

* Private EC2 cannot access internet directly
* NAT Instance enables outbound internet access
* Source/Destination check must be disabled
* iptables is required for NAT in Amazon Linux 2023

---

# ⚠️ Challenges Faced

* iptables not installed by default
* Forgetting to disable source/destination check
* Incorrect route table configuration
* Security group blocking traffic

---

# 🔧 Fix / Learning

* Installed `iptables-services` manually
* Enabled IP forwarding
* Configured NAT rules correctly
* Updated private route table

---

# 🧩 Summary

Successfully enabled internet access for private EC2 `datacenter-priv-ec2` using NAT Instance `datacenter-nat-instance`. Verified connectivity by confirming file upload to S3 bucket `datacenter-nat-5637`.

---
