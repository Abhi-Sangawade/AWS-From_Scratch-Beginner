# 📅 Day 2: Create Security Group (AWS)

---

## 🧠 Task

Create a security group in AWS under the **default VPC** with specific inbound rules.

---

## 🎯 Objective

* **Security Group Name:** `nautilus-sg`
* **Description:** Security group for Nautilus App Servers
* **Region:** `us-east-1`

### Inbound Rules:

| Type | Port | Source    |
| ---- | ---- | --------- |
| HTTP | 80   | 0.0.0.0/0 |
| SSH  | 22   | 0.0.0.0/0 |

---

## 🚀 Steps to Execute

### 1. Login to AWS

* Open the **Console URL**
* Enter Username & Password

---

### 2. Select Region

* Top-right corner → Select

```bash
us-east-1 (N. Virginia)
```

---

### 3. Go to EC2 Dashboard

* Search → **EC2**
* Open EC2 service

---

### 4. Create Security Group

Navigate to:

```bash
EC2 → Security Groups → Create Security Group
```

Fill details:

* Name → `nautilus-sg`
* Description → Security group for Nautilus App Servers
* VPC → Default

---

### 5. Add Inbound Rules

Add the following:

* **HTTP**

  * Port: 80
  * Source: `0.0.0.0/0`

* **SSH**

  * Port: 22
  * Source: `0.0.0.0/0`

👉 Click **Create Security Group**

---

## 💡 What I Learned Today

### What is a Security Group

A security group acts as a virtual firewall that controls inbound and outbound traffic for AWS resources like EC2.

### Importance of Ports and Protocols

Different services run on specific ports (HTTP → 80, SSH → 22), and allowing correct ports ensures proper access.

### CIDR Range (0.0.0.0/0)

This means access is allowed from anywhere (public access), which should be used carefully.

---

## 🛠️ What I Built / Practiced

* Logged into AWS Console
* Navigated to EC2 Security Groups
* Created a security group `nautilus-sg`
* Configured inbound rules for HTTP and SSH

---

## ⚠️ Challenges

* Understanding CIDR notation (`0.0.0.0/0`)
* Confusion about which ports to open

---

## 🔧 Fix / Learning

* Learned that `0.0.0.0/0` allows traffic from anywhere
* Understood standard ports (80 for HTTP, 22 for SSH)

---

## 🧩 Key Takeaway

Security groups are essential for controlling access and securing AWS resources.

---

## 🧩 Summary

```bash
Create a security group 'nautilus-sg' with HTTP (80) and SSH (22) access in us-east-1
```

---

## ✅ Outcome

Successfully created a security group and configured inbound rules for web and SSH access.
