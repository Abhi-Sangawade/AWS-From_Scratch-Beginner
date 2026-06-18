---

# 📅 Day 35: Deploying and Managing Applications on AWS

---

## 🧠 Task

* Create private RDS → `devops-rds` (MySQL 8.4.5)
* Configure EC2 → `devops-ec2` to connect with RDS
* Enable passwordless SSH access
* Deploy PHP application
* Verify DB connection via browser

---

## 🎯 Objective

* Integrate EC2 with RDS (application + database)
* Configure secure networking (private DB access)
* Automate SSH access
* Deploy a simple web application

---

## ☁️ AWS Details

* Service: EC2 + RDS
* Region: us-east-1
* EC2 Instance: `devops-ec2`
* RDS Instance: `devops-rds`
* Database: `devops_db`
* Engine: MySQL 8.4.5
* Instance Type: db.t3.micro

---

# 🚀 PART 1: Create RDS Instance

---

## 🔹 Step 1: Create Database

* Go to **RDS → Create database**
* Template → Sandbox
* Engine → MySQL 8.4.5
* DB Instance Identifier → `devops-rds`
* Master username → `devops_admin`
* Password → Set securely
* Instance class → `db.t3.micro`

---

## 🔹 Step 2: Storage Configuration

* Storage type → gp2
* Storage size → 5 GiB

---

## 🔹 Step 3: Additional Settings

* Database name → `devops_db`
* Public access → ❌ No (Private)

👉 Create database

---

## 🔹 Step 4: Wait for Availability

* Status → `Available`

---

# 🔐 PART 2: Security Group Configuration

---

## 🔹 Step 5: Update RDS Security Group

Allow inbound:

```text
Type → MySQL
Port → 3306
Source → devops-ec2 Security Group
```

---

## 🔹 Step 6: Update EC2 Security Group

Allow inbound:

```text
HTTP → Port 80 → 0.0.0.0/0
SSH → Port 22 → Your IP (or 0.0.0.0/0 for lab)
```

---

# 🔑 PART 3: Setup Passwordless SSH

---

## 🔹 Step 7: Generate SSH Key (aws-client)

```bash
ssh-keygen -t rsa -f /root/.ssh/id_rsa -N ""
```

---

## 🔹 Step 8: Copy Public Key to EC2

```bash
ssh-copy-id root@<ec2-public-ip>
```

👉 OR manually:

```bash
cat /root/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```

---

## 🔹 Step 9: Test SSH

```bash
ssh root@<ec2-public-ip>
```

---

# 🌐 PART 4: Deploy PHP Application

---

## 🔹 Step 10: Copy Application File

```bash
scp /root/index.php root@<ec2-public-ip>:/var/www/html/
```

---

## 🔹 Step 11: Update Database Connection

Edit `index.php`:

```php
<?php
$conn = new mysqli("RDS-ENDPOINT", "devops_admin", "PASSWORD", "devops_db");

if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}

echo "Connected successfully";
?>
```

👉 Replace:

* `RDS-ENDPOINT`
* `PASSWORD`

---

## 🔹 Step 12: Install Required Packages (on EC2)

```bash
sudo apt update
sudo apt install apache2 php php-mysql -y
sudo systemctl start apache2
sudo systemctl enable apache2
```

---

# 🔍 PART 5: Verification

---

## 🔹 Step 13: Access Application

Open browser:

```text
http://<ec2-public-ip>
```

---

## ✅ Expected Output

```text
Connected successfully
```

---

# 💡 Key Learning

* RDS should be private for security
* EC2 connects to RDS via internal network
* Security groups act as firewalls
* PHP + MySQL integration is common in web apps

---

# ⚠️ Challenges Faced

* RDS connectivity issues due to SG rules
* Incorrect DB endpoint or credentials
* Missing PHP MySQL extension
* SSH access setup confusion

---

# 🔧 Fix / Learning

* Allowed EC2 SG in RDS SG
* Verified endpoint and credentials
* Installed `php-mysql` package
* Configured SSH properly

---

# 🧩 Summary

Successfully deployed a web application on EC2 (`devops-ec2`) connected to a private RDS database (`devops-rds`). Verified connectivity by displaying "Connected successfully" via browser.

---
