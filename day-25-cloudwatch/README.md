# 📅 Day 25: Setting Up an EC2 Instance and CloudWatch Alarm

---

## 🧠 Task

* Launch an EC2 instance → `nautilus-ec2` (Ubuntu)
* Create a CloudWatch alarm → `nautilus-alarm`
* Monitor CPU Utilization
* Trigger alarm if CPU ≥ 90% for 5 minutes
* Send notification via SNS → `nautilus-sns-topic`

---

## 🎯 Objective

* Understand EC2 provisioning
* Learn CloudWatch monitoring and alarms
* Configure SNS notifications for alerts

---

## ☁️ AWS Details

* Service: EC2 + CloudWatch + SNS
* Region: us-east-1
* Instance Name: `nautilus-ec2`
* Alarm Name: `nautilus-alarm`
* Metric: CPUUtilization
* Threshold: ≥ 90%
* Period: 5 minutes
* SNS Topic: `nautilus-sns-topic`

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
7. Click **Launch Instance**

---

## 🔹 Step 2: Verify EC2 Instance

* Ensure instance is **running**
* Note Instance ID

---

## 🔹 Step 3: Create CloudWatch Alarm

1. Go to **CloudWatch → Alarms**
2. Click **Create Alarm**
3. Click **Select Metric**

---

## 🔹 Step 4: Choose Metric

1. Select:

   * EC2 → Per-Instance Metrics
2. Choose:

   * `CPUUtilization` for `nautilus-ec2`
3. Click **Select Metric**

---

## 🔹 Step 5: Configure Alarm

Set:

* Statistic → Average
* Period → 5 minutes
* Threshold type → Static
* Condition → Greater than or equal to
* Threshold value → `90`

---

## 🔹 Step 6: Configure Actions

1. In **Alarm state trigger**
2. Select:

   * Send notification to SNS topic
3. Choose → `nautilus-sns-topic`

---

## 🔹 Step 7: Name Alarm

* Alarm Name → `nautilus-alarm`
* Click **Create Alarm**

---

# 🔍 Verification

---

## ✅ 1. Check Alarm Status

* Go to **CloudWatch → Alarms**
* Status should be:

```text
OK
```

---

## ✅ 2. Trigger Test (Optional)

SSH into instance:

```bash
sudo apt update
sudo apt install stress -y
stress --cpu 2 --timeout 300
```

👉 This increases CPU usage

Expected:

```text
OK → ALARM
```

---

## 💡 Key Learning

* CloudWatch monitors AWS resources in real-time
* Alarms help automate alerting
* SNS enables notification delivery
* CPU metrics are critical for performance monitoring

---

## ⚠️ Challenges Faced

* Selecting correct metric (per-instance)
* Understanding alarm thresholds and periods
* Linking SNS topic correctly

---

## 🔧 Fix / Learning

* Selected EC2-specific CPUUtilization metric
* Configured threshold properly (≥ 90%)
* Verified SNS topic integration

---

## 🧩 Summary

Successfully launched EC2 instance `nautilus-ec2` and configured CloudWatch alarm `nautilus-alarm` to monitor CPU utilization, triggering notifications via SNS when threshold conditions are met.

---
