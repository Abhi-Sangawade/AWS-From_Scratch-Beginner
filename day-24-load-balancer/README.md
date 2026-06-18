---

# 📅 Day 24: Setting Up an Application Load Balancer for an EC2 Instance

---

## 🧠 Task

* Create an Application Load Balancer → `nautilus-alb`
* Create a target group → `nautilus-tg`
* Create a security group → `nautilus-sg` (allow HTTP - port 80)
* Attach security group to ALB
* Route traffic from ALB (port 80) → EC2 (`nautilus-ec2`, port 80)
* Ensure EC2 security group allows traffic

---

## 🎯 Objective

* Understand ALB architecture and flow
* Configure target groups and listeners
* Debug real-world load balancer issues

---

## ☁️ AWS Details

* Service: EC2 + Application Load Balancer
* Region: us-east-1
* Load Balancer: `nautilus-alb`
* Target Group: `nautilus-tg`
* Instance: `nautilus-ec2`
* Security Group: `nautilus-sg`

---

# 🚀 Steps to Execute (AWS Console)

---

## 🔹 Step 1: Create Security Group (for ALB)

1. Go to **EC2 → Security Groups**
2. Click **Create Security Group**
3. Name → `nautilus-sg`
4. Inbound Rule:

   * Type → HTTP
   * Port → 80
   * Source → `0.0.0.0/0`
5. Create security group

---

## 🔹 Step 2: Create Target Group

1. Go to **EC2 → Target Groups**
2. Click **Create target group**
3. Target type → Instances
4. Name → `nautilus-tg`
5. Protocol → HTTP
6. Port → 80
7. VPC → Same as EC2
8. Register instance → `nautilus-ec2`
9. Create

---

## 🔹 Step 3: Create Application Load Balancer

1. Go to **EC2 → Load Balancers**
2. Click **Create Load Balancer**
3. Choose **Application Load Balancer**
4. Name → `nautilus-alb`
5. Scheme → Internet-facing
6. Listener → HTTP (80)
7. Select at least 2 subnets
8. Attach security group → `nautilus-sg`
9. Select target group → `nautilus-tg`
10. Create

---

## 🔹 Step 4: Configure Listener (Important)

1. Go to **Load Balancer → Listeners**
2. Edit rules

👉 Set:

```text
Forward to → nautilus-tg
```

---

## 🔹 Step 5: Update EC2 Security Group

1. Go to **EC2 → Instances → nautilus-ec2**
2. Open Security Group
3. Add inbound rule:

* Type → HTTP
* Port → 80
* Source → `nautilus-sg`

---

# 🔍 Verification

---

## ✅ 1. Target Group Status

Go to:
👉 Target Groups → Targets

Expected:

```text
Status: healthy ✅
```

---

## ✅ 2. Access ALB DNS

* Copy ALB DNS
* Open in browser

👉 Should display Nginx page

---

# ⚠️ Issues Faced & Fixes

---

## ❌ Issue 1: Target Group showing **Unused**

### 🔍 Cause:

* Target group not attached to ALB listener
* OR Availability Zone mismatch

### ✅ Fix:

* Attached target group in listener
* Enabled correct AZ for ALB

---

## ❌ Issue 2: Target Group showing **Unhealthy**

### 🔍 Cause:

* EC2 reachable but not responding
* Nginx not running
* Port 80 blocked

---

## 🔧 Fix Steps

### ✔ Start Nginx

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

---

### ✔ Verify Application

```bash
curl localhost
```

👉 Should return Nginx HTML

---

### ✔ Fix Security Group

* Allow HTTP (80) from `nautilus-sg`

---

### ✔ Check Health Check Settings

* Protocol → HTTP
* Port → 80
* Path → `/`

---

### ✔ Wait for Health Check

* Takes ~1–2 minutes

---

# 💡 Key Learning

* ALB setup requires correct chaining:

  ```
  ALB → Listener → Target Group → EC2
  ```
* Security groups must allow ALB → EC2 communication
* Health checks validate application availability
* Debugging is key in real-world cloud setups

---

# 🧩 Summary

Successfully configured Application Load Balancer `nautilus-alb` with target group `nautilus-tg`, resolved issues like **Unused and Unhealthy targets**, and ensured proper traffic routing to EC2 instance `nautilus-ec2`.

---
