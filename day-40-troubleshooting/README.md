---

# 📅 Day 40: Troubleshooting Internet Accessibility for an EC2-Hosted Application

---

## 🧠 Task

* Troubleshoot EC2 internet accessibility issue
* Verify VPC → `datacenter-vpc` configuration
* Ensure EC2 → `datacenter-ec2` is accessible on port 80
* Fix networking issues preventing public access

---

## 🎯 Objective

* Understand VPC networking components
* Debug real-world connectivity issues
* Verify end-to-end internet access flow
* Ensure web application is publicly reachable

---

## ☁️ AWS Details

* Service: EC2 + VPC
* Region: us-east-1
* VPC: `datacenter-vpc`
* EC2 Instance: `datacenter-ec2`
* Security Group: `datacenter-sg`
* Application: Nginx (port 80)

---

# 🚀 Architecture Overview

```text id="netflow1"
Internet → IGW → Route Table → Subnet → EC2 (Nginx)
```

---

# 🔍 TROUBLESHOOTING CHECKLIST

---

# 🟢 PART 1: Verify Internet Gateway (IGW)

---

## 🔹 Step 1: Check IGW

* Go to **VPC → Internet Gateways**
* Ensure IGW is:

```text id="chk1"
Attached to datacenter-vpc
```

👉 If not:

* Create IGW
* Attach to VPC

---

# 🟡 PART 2: Verify Route Table

---

## 🔹 Step 2: Check Public Route

* Go to **VPC → Route Tables**
* Select route table associated with subnet

Ensure:

```text id="chk2"
0.0.0.0/0 → Internet Gateway
```

❌ If missing → Add route

---

# 🔵 PART 3: Verify Subnet Configuration

---

## 🔹 Step 3: Check Subnet Settings

* Go to **Subnets → datacenter-subnet**

Ensure:

```text id="chk3"
Auto-assign public IPv4 → Enabled
```

---

# 🟣 PART 4: Verify EC2 Instance

---

## 🔹 Step 4: Check Public IP

* EC2 → Instance details

Ensure:

```text id="chk4"
Public IPv4 address → Present
```

❌ If not:

* Assign Elastic IP

---

# 🔴 PART 5: Verify Security Group

---

## 🔹 Step 5: Check Inbound Rules

Security group → `datacenter-sg`

```text id="chk5"
HTTP → Port 80 → 0.0.0.0/0
SSH → Port 22 → Your IP
```

---

# 🟠 PART 6: Verify Network ACL (NACL)

---

## 🔹 Step 6: Check NACL Rules

Ensure:

```text id="chk6"
Inbound → Allow HTTP (80)
Outbound → Allow all traffic
```

❌ If DENY present → Fix rules

---

# 🟤 PART 7: Verify Nginx Service

---

## 🔹 Step 7: SSH into EC2

```bash id="net1"
ssh ubuntu@<public-ip>
```

---

## 🔹 Step 8: Check Nginx Status

```bash id="net2"
sudo systemctl status nginx
```

👉 If not running:

```bash id="net3"
sudo systemctl start nginx
```

---

## 🔹 Step 9: Test Locally

```bash id="net4"
curl localhost
```

Expected:

```text id="chk7"
Welcome to nginx!
```

---

# 🔍 FINAL VERIFICATION

---

## 🔹 Step 10: Access from Browser

```text id="net5"
http://<public-ip>
```

---

## ✅ Expected Output

```text id="chk8"
Welcome to nginx!
```

---

# 💡 Key Learning

* Internet access depends on multiple layers (IGW + Route + SG + NACL)
* One misconfiguration can block entire connectivity
* Public subnet requires proper routing and IP assignment
* Troubleshooting should follow step-by-step flow

---

# ⚠️ Common Issues Found

* Missing Internet Gateway ❌
* Route table not configured ❌
* No public IP on EC2 ❌
* Security group blocking port 80 ❌
* Nginx not running ❌

---

# 🔧 Fix / Learning

* Attached IGW to VPC
* Added route `0.0.0.0/0 → IGW`
* Enabled public IP / assigned Elastic IP
* Updated security group rules
* Started and verified Nginx service

---

# 🧩 Summary

Successfully troubleshot and resolved internet accessibility issue for EC2 instance `datacenter-ec2` by fixing VPC configuration, routing, and service-level issues. Verified application accessibility via public IP on port 80.

---
