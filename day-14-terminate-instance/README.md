---

# 📅 Day 14: Terminate EC2 Instance

---

## 🧠 Task

Terminate the EC2 instance `datacenter-ec2` in the **us-east-1** region.
Ensure the instance reaches the **terminated** state before submission.

---

## 🎯 Objective

* Understand EC2 instance lifecycle
* Safely delete unused cloud resources
* Verify resource termination status

---

## ☁️ AWS Details

* Service: Amazon EC2
* Region: us-east-1
* Instance Name: `datacenter-ec2`

---

## 🚀 Steps to Execute (AWS Console)

### 1. Login to AWS Console

* Use provided credentials
* Select region: **us-east-1**

---

### 2. Navigate to EC2

* Search for **EC2**
* Open EC2 Dashboard

---

### 3. Select Instance

* Go to **Instances**
* Select instance: `datacenter-ec2`

---

### 4. Terminate Instance

* Click **Actions → Instance State → Terminate Instance**
* Confirm termination

---

### 5. Verify Termination

* Check instance state → `terminated`

---

## 💻 AWS CLI Method

### ✅ Step 1: Get Instance ID

```bash id="t1"
aws ec2 describe-instances --filters "Name=tag:Name,Values=datacenter-ec2" --query "Reservations[].Instances[].InstanceId" --output text
```

---

### ✅ Step 2: Terminate Instance

```bash id="t2"
aws ec2 terminate-instances --instance-ids <instance-id>
```

---

### ✅ Step 3: Verify Status

```bash id="t3"
aws ec2 describe-instances --instance-ids <instance-id> --query "Reservations[].Instances[].State.Name" --output text
```

👉 Expected Output:

```id="t4"
terminated
```

---

## 🔍 Important Notes

* Termination is irreversible
* All attached instance-store data will be lost
* EBS volumes may persist depending on configuration

---

## 💡 Key Learning

* EC2 instances follow lifecycle states (running → stopped → terminated)
* Proper cleanup avoids unnecessary costs
* Always verify termination before submission

---

## ⚠️ Challenges Faced

* Identifying correct instance ID
* Ensuring instance reaches fully terminated state
* Understanding impact of deletion

---

## 🧩 Summary

Successfully terminated EC2 instance `datacenter-ec2`, ensuring proper cleanup of unused resources and verifying the terminated state.

---

---

# 🚀 Pro Tip

👉 Interview question:
**What happens to EBS volumes after instance termination?**

👉 Answer:
“Depends on the ‘Delete on Termination’ setting—root volume is usually deleted, additional volumes may persist.”

---
