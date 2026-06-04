## 🔐 Scenario 01: Lost EC2 Key Pair

### 🎯 Problem Statement

An EC2 instance is running in production.

The PEM file used to access the instance has been lost.

The server is still running and contains critical application data.

How do you regain access?

---

## ⚠️ Impact

* SSH access unavailable
* Application still running
* Risk of downtime
* Unable to perform maintenance activities

---

# 🚀 Solution

## Method 1: AWS Systems Manager (Recommended)

### Prerequisites

* SSM Agent installed on EC2
* IAM Role attached with SSM permissions
* Instance has internet access or VPC endpoints configured

### Steps

1. Open AWS Console
2. Navigate to EC2
3. Select the instance
4. Click **Connect**
5. Select **Session Manager**
6. Connect to the instance

Once connected, create a new SSH key pair and update access.

---

## Method 2: Recover Access Using Rescue Instance

### Step 1: Stop the EC2 Instance

```text
EC2 → Instances → Select Instance → Stop
```

---

### Step 2: Detach Root EBS Volume

```text
EC2 → Volumes → Select Root Volume → Detach
```

Example:

```text
vol-xxxxxxxxxxxxxxxxx
```

---

### Step 3: Launch a Temporary Rescue Instance

Requirements:

* Same Region
* Same Availability Zone
* Linux AMI

Example:

```text
Amazon Linux 2023
```

---

### Step 4: Attach the Original Volume

Attach the detached volume to the rescue instance.

Example Device Name:

```text
/dev/xvdf
```

---

### Step 5: Connect to Rescue Instance

```bash
ssh -i rescue-key.pem ec2-user@<Public-IP>
```

---

### Step 6: Mount the Volume

Check available disks:

```bash
lsblk
```

Create mount directory:

```bash
sudo mkdir /mnt/recovery
```

Mount volume:

```bash
sudo mount /dev/xvdf1 /mnt/recovery
```

---

### Step 7: Create a New SSH Key Pair

On your local machine:

#### RSA Key

```bash
ssh-keygen -t rsa -b 4096 -f new-ec2-key
```

Generated Files:

```text
new-ec2-key
new-ec2-key.pub
```

OR

#### ED25519 Key

```bash
ssh-keygen -t ed25519 -f new-ec2-key
```

Generated Files:

```text
new-ec2-key
new-ec2-key.pub
```

Display public key:

```bash
cat new-ec2-key.pub
```

Copy the entire output.

---

### Step 8: Update authorized_keys

Open:

```bash
sudo vim /mnt/recovery/home/ec2-user/.ssh/authorized_keys
```

Append the new public key.

Example:

```text
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQ...
```

Save and exit.

---

### Step 9: Unmount Volume

```bash
sudo umount /mnt/recovery
```

---

### Step 10: Detach Volume from Rescue Instance

```text
EC2 → Volumes → Detach
```

---

### Step 11: Reattach Volume to Original Instance

Attach as:

```text
/dev/xvda
```

(or original root device)

---

### Step 12: Start Original Instance

```text
EC2 → Instances → Start Instance
```

---

### Step 13: Connect Using New Key

```bash
chmod 400 new-ec2-key

ssh -i new-ec2-key ec2-user@<Public-IP>
```

---

# 💡 Lessons Learned

* Always keep PEM files secure
* Store backup copies in a secure location
* Enable AWS Systems Manager Session Manager
* Use IAM Roles for emergency access
* Maintain recovery procedures for production environments

---

# 🎤 Interview Question

### What happens if you lose an EC2 Key Pair?

**Answer:**

AWS cannot recover the private key because only the public key is stored by AWS.

To regain access:

1. Use AWS Systems Manager Session Manager (if configured).
2. Detach the root EBS volume.
3. Attach it to a rescue instance.
4. Generate a new SSH key pair.
5. Update the `authorized_keys` file.
6. Reattach the volume and restart the original instance.

This restores SSH access without rebuilding the server.

---

# ✅ Outcome

Successfully recovered access to an EC2 instance after losing the original PEM file without data loss or server rebuild.

