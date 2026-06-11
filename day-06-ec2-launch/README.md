---

# 🚀 Day 6: Launch EC2 Instance (AWS)

## 📌 Task Overview

The Nautilus DevOps team is planning to migrate part of their infrastructure to the AWS cloud. Instead of performing a large-scale migration at once, they are breaking it down into smaller, manageable steps.

In this task, we create an **EC2 instance** with specific configurations.

---

## 🎯 Requirements

* Instance Name: **devops-ec2**
* AMI: **Amazon Linux**
* Instance Type: **t2.micro**
* Key Pair: **Create new RSA key pair (devops-kp)**
* Security Group: **Default security group**

---

## 🛠️ Step-by-Step Implementation

### 1️⃣ Login to AWS Console

* Open AWS Management Console
* Navigate to **EC2 Dashboard**

---

### 2️⃣ Launch Instance

* Click on **Launch Instance**

---

### 3️⃣ Configure Instance Details

#### 🔹 Name

```
devops-ec2
```

#### 🔹 AMI Selection

* Choose: **Amazon Linux (Amazon Linux 2 or latest available)**

#### 🔹 Instance Type

```
t2.micro
```

---

### 4️⃣ Create Key Pair

* Select: **Create new key pair**
* Key Pair Name:

```
devops-kp
```

* Key Type: **RSA**
* Download the `.pem` file (Important ⚠️)

---

### 5️⃣ Network & Security

* Use **default VPC and subnet**
* Select **Default Security Group**

  * Allows basic inbound/outbound rules

---

### 6️⃣ Launch Instance

* Click **Launch Instance**
* Wait until instance state becomes **Running**

---

## 🔍 Verification Steps

* Go to **EC2 Dashboard → Instances**
* Check:

  * Instance Name → `devops-ec2`
  * Status → `Running`
  * Instance Type → `t2.micro`

---

## 🔐 Connect to Instance (Optional)

Using SSH:

```bash
chmod 400 devops-kp.pem
ssh -i devops-kp.pem ec2-user@<public-ip>
```

---

## ✅ Key Learnings

* How to launch an EC2 instance
* Importance of key pairs for secure access
* Basic AWS networking (default security group)
* Understanding instance types and AMIs

---

## 🧠 Notes

* `t2.micro` is eligible for AWS Free Tier
* Always keep your `.pem` file secure
* Default security group allows basic connectivity

---

## 📌 Conclusion

Successfully launched an EC2 instance named **devops-ec2** using Amazon Linux, configured with a secure key pair and default networking setup. This marks an important step in understanding AWS infrastructure basics.

---
