---

# 📅 Day 26: Configuring an EC2 Instance as a Web Server with Nginx

---

## 🧠 Task

* Launch an EC2 instance → `nautilus-ec2` (Ubuntu)
* Configure it as a web server using **Nginx**
* Use **User Data script** for automation
* Allow HTTP (port 80) access from the internet

---

## 🎯 Objective

* Understand EC2 provisioning with automation
* Learn how to use User Data scripts
* Configure a basic web server using Nginx

---

## ☁️ AWS Details

* Service: Amazon EC2
* Region: us-east-1
* Instance Name: `nautilus-ec2`
* AMI: Ubuntu
* Web Server: Nginx
* Port: 80 (HTTP)

---

# 🚀 Steps to Execute (AWS Console)

---

## 🔹 Step 1: Launch EC2 Instance

1. Go to **EC2 → Instances → Launch Instance**
2. Name → `nautilus-ec2`
3. AMI → Ubuntu
4. Instance Type → t2.micro
5. Key Pair → Select/Create
6. Network → Default VPC

---

## 🔹 Step 2: Configure Security Group

👉 Under Network Settings:

* Allow:

  * Type → HTTP
  * Port → 80
  * Source → `0.0.0.0/0`

---

## 🔹 Step 3: Add User Data Script

👉 Scroll to **Advanced Details → User Data**

Paste:

```bash id="d26-1"
#!/bin/bash
apt update -y
apt install nginx -y
systemctl start nginx
systemctl enable nginx
```

---

## 🔹 Step 4: Launch Instance

* Click **Launch Instance**

---

## 🔹 Step 5: Wait for Initialization

* Ensure instance state → `running`
* Status checks → `2/2 checks passed`

---

## 🔍 Verification

---

## ✅ 1. Access via Browser

* Copy Public IP of instance
* Open:

```text
http://<public-ip>
```

👉 Expected:

```text
Welcome to nginx!
```

---

## ✅ 2. Verify via SSH (Optional)

```bash id="d26-2"
ssh ubuntu@<public-ip>
```

Check:

```bash id="d26-3"
sudo systemctl status nginx
```

👉 Status should be:

```text
active (running)
```

---

## 💡 Key Learning

* User Data scripts automate instance setup
* Nginx can be installed during launch (no manual steps)
* Security groups control web access
* EC2 can act as a simple web server

---

## ⚠️ Challenges Faced

* Forgetting to open port 80
* User Data script not running properly
* Checking service status after launch

---

## 🔧 Fix / Learning

* Enabled HTTP access in security group
* Verified User Data execution logs
* Ensured Nginx service is running

---

## 🧩 Summary

Successfully launched EC2 instance `nautilus-ec2` and configured it as a web server using Nginx via User Data script, making it accessible over HTTP.

---
