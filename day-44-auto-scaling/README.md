---

# 📅 Day 44: Implementing Auto Scaling for High Availability in AWS

---

## 🧠 Task

* Create Launch Template → `devops-launch-template`
* Install Nginx via User Data
* Create Auto Scaling Group → `devops-asg`
* Configure scaling (CPU ≥ 50%)
* Create Target Group → `devops-tg`
* Create Application Load Balancer → `devops-alb`
* Verify application via ALB DNS

---

## 🎯 Objective

* Ensure high availability using Auto Scaling
* Automatically scale instances based on CPU utilization
* Distribute traffic using Application Load Balancer
* Deploy scalable web application with Nginx

---

## ☁️ AWS Details

* Service: EC2 + ASG + ALB
* Region: us-east-1
* Launch Template: `devops-launch-template`
* Auto Scaling Group: `devops-asg`
* Target Group: `devops-tg`
* Load Balancer: `devops-alb`

---

# 🚀 Architecture Overview

```text id="asgflow1"
User → ALB → Target Group → Auto Scaling Group → EC2 (Nginx)
```

---

# 🟢 PART 1: Create Launch Template

---

## 🔹 Step 1: Create Launch Template

1. Go to **EC2 → Launch Templates → Create**
2. Name → `devops-launch-template`

---

## 🔹 Step 2: Configure Template

* AMI → Amazon Linux 2
* Instance type → t2.micro
* Security group → Allow HTTP (port 80)

---

## 🔹 Step 3: Add User Data Script

```bash id="asg1"
#!/bin/bash
yum update -y
yum install nginx -y
systemctl start nginx
systemctl enable nginx
```

---

# 🟡 PART 2: Create Target Group

---

## 🔹 Step 4: Create Target Group

1. Go to **EC2 → Target Groups → Create**
2. Name → `devops-tg`
3. Target type → Instance
4. Protocol → HTTP
5. Port → 80

---

# 🔵 PART 3: Create Application Load Balancer

---

## 🔹 Step 5: Create ALB

1. Go to **EC2 → Load Balancers → Create**
2. Type → Application Load Balancer
3. Name → `devops-alb`
4. Scheme → Internet-facing
5. Listener → HTTP (80)

---

## 🔹 Step 6: Configure Networking

* Select VPC
* Choose at least 2 subnets

---

## 🔹 Step 7: Configure Listener

* Forward traffic → `devops-tg`

---

# 🟣 PART 4: Create Auto Scaling Group

---

## 🔹 Step 8: Create ASG

1. Go to **EC2 → Auto Scaling Groups → Create**
2. Name → `devops-asg`
3. Select launch template → `devops-launch-template`

---

## 🔹 Step 9: Configure ASG

* Min capacity → 1
* Desired capacity → 1
* Max capacity → 2

---

## 🔹 Step 10: Attach Load Balancer

* Select → `devops-alb`
* Attach target group → `devops-tg`

---

# 🔴 PART 5: Configure Scaling Policy

---

## 🔹 Step 11: Set Scaling Policy

* Policy type → Target tracking
* Metric → Average CPU utilization
* Target value → **50%**

---

# 🟠 PART 6: Configure Health Checks

---

## 🔹 Step 12: Health Check Settings

* Type → ELB
* Path → `/`

---

# 🔍 PART 7: Verification

---

## 🔹 Step 13: Check Instances

```text id="asgverify1"
At least 1 instance running
```

---

## 🔹 Step 14: Verify Target Health

```text id="asgverify2"
Status: Healthy
```

---

## 🔹 Step 15: Access Application

* Copy ALB DNS
* Open in browser

👉 Expected:

```text id="asgverify3"
Welcome to nginx!
```

---

# 💡 Key Learning

* Auto Scaling ensures high availability
* ALB distributes traffic across instances
* Launch templates standardize instance configuration
* Scaling policies automate infrastructure based on load

---

# ⚠️ Challenges Faced

* Instances not registering in target group ❌
* Health check failures ❌
* Nginx not installed properly ❌

---

# 🔧 Fix / Learning

* Verified User Data script execution
* Checked security group rules (port 80 open)
* Ensured correct target group attachment
* Used proper health check path `/`

---

# 🧩 Summary

Successfully implemented Auto Scaling Group `devops-asg` with launch template `devops-launch-template`, configured ALB `devops-alb`, and deployed a highly available Nginx-based web application that scales based on CPU utilization.

---
