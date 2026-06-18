---

# 📅 Day 36: Load Balancing EC2 Instances with Application Load Balancer

---

## 🧠 Task

* Create Security Group → `nautilus-sg`
* Launch EC2 instance → `nautilus-ec2` (Ubuntu + Nginx via User Data)
* Create Application Load Balancer → `nautilus-alb`
* Create Target Group → `nautilus-tg`
* Route traffic from ALB → EC2 (port 80)
* Ensure application is accessible via ALB DNS

---

## 🎯 Objective

* Understand load balancing in AWS
* Configure Application Load Balancer (ALB)
* Register EC2 instance in target group
* Enable high availability and traffic routing

---

## ☁️ AWS Details

* Service: EC2 + ALB
* Region: us-east-1
* EC2 Instance: `nautilus-ec2`
* Load Balancer: `nautilus-alb`
* Target Group: `nautilus-tg`
* Security Group: `nautilus-sg`

---

# 🚀 Architecture Overview

```text id="albflow1"
User → ALB → Target Group → EC2 (Nginx)
```

---

# 🟢 PART 1: Create Security Group

---

## 🔹 Step 1: Create `nautilus-sg`

* Go to **EC2 → Security Groups → Create**
* Name → `nautilus-sg`

### Inbound Rule:

```text id="albsg1"
Type → HTTP
Port → 80
Source → default security group
```

👉 Attach this SG to EC2 instance

---

# 🟡 PART 2: Launch EC2 Instance

---

## 🔹 Step 2: Launch Instance

* Name → `nautilus-ec2`
* AMI → Ubuntu
* Instance type → t2.micro

---

## 🔹 Step 3: Add User Data Script

```bash id="alb2"
#!/bin/bash
apt update -y
apt install nginx -y
systemctl start nginx
systemctl enable nginx
```

---

## 🔹 Step 4: Attach Security Group

* Attach → `nautilus-sg`

---

# 🔵 PART 3: Create Target Group

---

## 🔹 Step 5: Create Target Group

* Name → `nautilus-tg`
* Target type → Instance
* Protocol → HTTP
* Port → 80
* VPC → same as EC2

---

## 🔹 Step 6: Register Target

* Select → `nautilus-ec2`
* Add to target group

---

# 🟣 PART 4: Create Application Load Balancer

---

## 🔹 Step 7: Create ALB

* Name → `nautilus-alb`
* Type → Internet-facing
* Listener → HTTP (80)
* Attach → Default security group

---

## 🔹 Step 8: Configure Subnets

* Select at least 2 subnets

---

## 🔹 Step 9: Attach Target Group

* Forward traffic → `nautilus-tg`

---

# 🔐 PART 5: Security Group Adjustments

---

## 🔹 Step 10: Update Default SG (ALB)

Allow inbound:

```text id="albsg2"
HTTP → Port 80 → 0.0.0.0/0
```

---

## 🔹 Step 11: Verify EC2 SG

Ensure:

```text id="albsg3"
HTTP → Port 80 → Source: default SG (ALB)
```

---

# 🔍 PART 6: Verification

---

## 🔹 Step 12: Check Target Health

* Go to Target Group → Targets

```text id="albverify1"
Status: healthy
```

---

## 🔹 Step 13: Access ALB DNS

* Copy ALB DNS
* Open in browser

👉 Expected:

```text id="albverify2"
Welcome to nginx!
```

---

# 💡 Key Learning

* ALB distributes traffic across targets
* Target groups define backend instances
* Health checks ensure application availability
* Security groups control ALB ↔ EC2 communication

---

# ⚠️ Challenges Faced

* Target group showing **Unused**
* Instance showing **Unhealthy**
* Security group misconfiguration

---

# 🔧 Fix / Learning

* Attached target group correctly to ALB listener
* Started Nginx on EC2
* Allowed HTTP traffic from ALB SG to EC2 SG
* Verified health check path `/`

---

# 🧩 Summary

Successfully deployed EC2 instance `nautilus-ec2` with Nginx, configured Application Load Balancer `nautilus-alb`, and routed traffic via target group `nautilus-tg`. Verified application accessibility using ALB DNS.

---
