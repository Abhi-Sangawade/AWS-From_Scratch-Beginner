# 📅 Day 3: Create Subnet (AWS)

---

## 🧠 Task

Create a subnet named `devops-subnet` under the **default VPC** in AWS.

---

## 🎯 Objective

* **Subnet Name:** `devops-subnet`
* **VPC:** Default VPC
* **Region:** `us-east-1`

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

### 3. Go to VPC Dashboard

* Search → **VPC**
* Open VPC service

---

### 4. Create Subnet

Navigate to:

```bash
VPC → Subnets → Create Subnet
```

Fill details:

* VPC → Default
* Subnet name → `devops-subnet`
* Availability Zone → Any (default)
* IPv4 CIDR block → (leave default or auto-assign)

👉 Click **Create Subnet**

---

## 💡 What I Learned Today

### What is a Subnet

A subnet is a smaller network inside a VPC that helps organize and isolate resources.

### Role of VPC and Subnets

VPC provides the network, and subnets divide it into smaller sections for better management and security.

### Availability Zones

Subnets are tied to a specific Availability Zone, which helps in high availability and fault tolerance.

---

## 🛠️ What I Built / Practiced

* Logged into AWS Console
* Navigated to VPC service
* Created a subnet `devops-subnet`
* Understood subnet placement inside a VPC

---

## ⚠️ Challenges

* Confusion between VPC and Subnet
* Unsure about CIDR block selection

---

## 🔧 Fix / Learning

* Learned that subnets are subdivisions of a VPC
* Understood basics of CIDR allocation

---

## 🧩 Key Takeaway

Subnets help structure your cloud network and improve organization and security.

---

## 🧩 Summary

```bash
Create a subnet 'devops-subnet' under default VPC in us-east-1
```

---

## ✅ Outcome

Successfully created a subnet and understood basic AWS networking concepts.
