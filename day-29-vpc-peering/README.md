---

# 🚀 Day 29: Establishing Secure Communication Between Public and Private VPCs via VPC Peering

## 📌 Objective

Demonstrate how to enable communication between a **public VPC** and a **private VPC** using **VPC Peering**, ensuring secure connectivity between EC2 instances across both networks.

---

## 🧩 Architecture Overview

* **Public VPC (Default VPC)**

  * EC2: `xfusion-public-ec2`

* **Private VPC**

  * Name: `xfusion-private-vpc`
  * CIDR: `10.1.0.0/16`
  * Subnet: `xfusion-private-subnet` (`10.1.1.0/24`)
  * EC2: `xfusion-private-ec2`

---

## 🛠️ Tasks Performed

### 1️⃣ Verify Existing Resources

Ensure the following resources are already available:

| Resource Type  | Name                     | Details                |
| -------------- | ------------------------ | ---------------------- |
| Public EC2     | `xfusion-public-ec2`     | Running in default VPC |
| Private VPC    | `xfusion-private-vpc`    | CIDR: `10.1.0.0/16`    |
| Private Subnet | `xfusion-private-subnet` | CIDR: `10.1.1.0/24`    |
| Private EC2    | `xfusion-private-ec2`    | Inside private subnet  |

---

### 2️⃣ Create VPC Peering Connection

* Navigate to **VPC Dashboard → Peering Connections**
* Click **Create Peering Connection**

**Configuration:**

* Name: `xfusion-vpc-peering`
* Requester VPC: Default VPC
* Accepter VPC: `xfusion-private-vpc`

✅ After creation, **Accept the Peering Request**

---

### 3️⃣ Update Route Tables

#### 📍 Public VPC Route Table

* Add route:

  ```
  Destination: 10.1.0.0/16
  Target: xfusion-vpc-peering
  ```

#### 📍 Private VPC Route Table

* Add route:

  ```
  Destination: <Default VPC CIDR>
  Target: xfusion-vpc-peering
  ```

---

### 4️⃣ Update Security Groups

#### 🔐 Private EC2 Security Group

Allow traffic from Public VPC:

* **ICMP (Ping)** → Source: Default VPC CIDR
* **SSH (Port 22)** → Source: Default VPC CIDR

---

### 5️⃣ Configure SSH Access

From AWS client host:

```bash
ssh-keygen -t rsa
```

Copy public key:

```bash
cat ~/.ssh/id_rsa.pub
```

Add this key to:

```
/home/ec2-user/.ssh/authorized_keys
```

on **xfusion-public-ec2**

---

### 6️⃣ Test Connectivity

#### 🔹 Step 1: SSH into Public EC2

```bash
ssh ec2-user@<public-ec2-ip>
```

#### 🔹 Step 2: Ping Private EC2

```bash
ping <private-ec2-private-ip>
```

✅ Successful response confirms:

* VPC Peering is working
* Route tables are correctly configured
* Security groups allow traffic

---

## ✅ Expected Outcome

* Public EC2 can communicate with Private EC2
* Private network remains isolated from the internet
* Secure internal communication established via VPC Peering

---

## 🧠 Key Learnings

* VPC Peering enables **private communication between VPCs**
* Route tables are critical for directing traffic
* Security groups act as **firewall controls**
* No internet gateway is required for inter-VPC communication

---

## ⚠️ Common Issues & Fixes

| Issue            | Fix                         |
| ---------------- | --------------------------- |
| Ping not working | Check ICMP rule in SG       |
| SSH fails        | Verify key & port 22 access |
| No route         | Ensure route tables updated |
| Peering inactive | Accept request              |

---

## 🏁 Conclusion

This setup demonstrates a real-world use case of connecting isolated environments securely using **VPC Peering**, a fundamental concept in AWS networking and DevOps architecture.

---
