# 📅 Day 22: Configuring Secure SSH Access to an EC2 Instance

---

## 🧠 Task

Create an EC2 instance `devops-ec2` (t2.micro) and configure **passwordless SSH access** from the `aws-client` host using an SSH key (`id_rsa`).

---

## 🎯 Objective

* Understand SSH key-based authentication
* Enable secure, passwordless login to EC2
* Practice secure remote access setup

---

## ☁️ AWS Details

* Service: Amazon EC2
* Region: us-east-1
* Instance Name: `devops-ec2`
* Instance Type: `t2.micro`
* SSH Key Path: `/root/.ssh/id_rsa` (on aws-client)

---

## 🚀 Steps to Execute

---

### 🔹 Step 1: Launch EC2 Instance

```bash
aws ec2 run-instances \
  --image-id <ami-id> \
  --instance-type t2.micro \
  --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=devops-ec2}]'
```

---

### 🔹 Step 2: Get Public IP

```bash
aws ec2 describe-instances \
--filters "Name=tag:Name,Values=devops-ec2" \
--query "Reservations[].Instances[].PublicIpAddress" \
--output text
```

---

### 🔹 Step 3: Generate SSH Key on aws-client

```bash
ssh-keygen -t rsa
```

👉 Saved at:

```
/root/.ssh/id_rsa
/root/.ssh/id_rsa.pub
```

---

### 🔹 Step 4: Verify Key Files

```bash
ls -l /root/.ssh
```

👉 Confirm:

* `id_rsa` (private key)
* `id_rsa.pub` (public key)

---

### 🔹 Step 5: Copy Public Key

```bash
cat /root/.ssh/id_rsa.pub
```

👉 Copy the entire key

---

### 🔹 Step 6: Login to EC2 Instance

```bash
ssh ubuntu@<ec2-public-ip>
```

Then switch to root:

```bash
sudo -i
```

---

### 🔹 Step 7: Configure SSH on EC2

```bash
cd /root/.ssh
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

---

### 🔹 Step 8: Add Public Key

```bash
vi authorized_keys
```

👉 Paste the public key

OR

```bash
echo "<paste-public-key>" >> /root/.ssh/authorized_keys
chmod 600 /root/.ssh/authorized_keys
```

---

### 🔹 Step 9: Test Passwordless SSH

From aws-client:

```bash
ssh root@<ec2-public-ip>
```

✅ Successfully logged in without password

---

## 🔍 Verification

* Able to SSH as `root` without password
* Key-based authentication working
* Correct permissions:

  * `.ssh` → `700`
  * `authorized_keys` → `600`

---

## ⚠️ Challenges Faced

* Initial `Permission denied (publickey)` error
* Tried connecting from local machine instead of aws-client
* Missing key in EC2 authorized_keys

---

## 🔧 Fix / Learning

* Generated SSH key on aws-client
* Copied public key correctly to EC2
* Used correct access flow:

  ```
  Local → aws-client → EC2
  ```
* Switched to root user before configuring SSH

---

## 💡 Key Learning

* SSH key authentication is more secure than passwords
* Public key goes to server, private key stays on client
* `.ssh` permissions are critical for SSH to work
* Bastion/jump host pattern is used in real-world setups

---

## 🧩 Summary

Successfully configured **secure passwordless SSH access** to EC2 instance `devops-ec2` using an SSH key from the aws-client host, resolving authentication issues and ensuring proper key-based login.

---
