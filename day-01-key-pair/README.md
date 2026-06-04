# 📅 Day 1: Create Key Pair (AWS)

---

## 🎯 Task

Create an AWS key pair with:

* **Name:** `datacenter-kp`
* **Type:** `RSA`
* **Region:** `us-east-1`

---

## 🧠 What is a Key Pair?

A key pair is used to securely connect to EC2 instances.

* Public key → Stored in AWS
* Private key → Downloaded (`.pem` file)

---

## 🚀 Steps

1. Login to AWS Console
2. Select region → **us-east-1**
3. Go to **EC2 Dashboard**
4. Click **Key Pairs → Create Key Pair**
5. Enter:

   * Name: `datacenter-kp`
   * Type: `RSA`
6. Click **Create** and download the `.pem` file

---

## ⚠️ Important

* Use **us-east-1 region only**
* Name must be exactly **datacenter-kp**
* Select **RSA**, not ED25519

---

## ❌ Common Mistakes

* Wrong region
* Incorrect key name
* Wrong key type

---

## 💡 What I Learned Today

### What a Key Pair is and why it's used 🔑

A key pair is a secure authentication method used to connect to EC2 instances without passwords. It ensures safe access by using cryptographic keys.

### Difference between public key and private key

* **Public key:** Stored in AWS and attached to EC2 instances
* **Private key (.pem):** Downloaded by the user and used to securely connect to the instance

### Importance of regions in AWS

AWS resources are region-specific. Selecting the correct region (like `us-east-1`) ensures resources are created in the expected location and avoids configuration issues.

### Basics of EC2 security and access

EC2 instances are accessed securely using key pairs and controlled using security groups, ensuring only authorized users can connect.

---

## 🛠️ What I Built / Practiced

* Logged into AWS Console
* Navigated to EC2 dashboard
* Created a key pair → `datacenter-kp` (RSA)
* Downloaded and securely stored the `.pem` file

---

## ⚠️ Challenges

* Initially ignored the region selection ⚠️
* Confused between RSA and ED25519 key types

---

## 🔧 Fix / Learning

* Understood that region matters for resource creation
* Learned RSA is widely used and expected in most labs

---

## 🧩 Key Takeaway

Even a simple step like creating a key pair is crucial for secure cloud access.

---

## 🧩 Summary

```bash
Create an RSA key pair named datacenter-kp in us-east-1 using EC2
```

---

## ✅ Outcome

Key pair successfully created and ready for EC2 usage 🚀
