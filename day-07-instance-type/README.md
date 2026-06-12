# 🚀 Day 7: EC2 Instance Type Modification for Cost Optimization

## 📌 Challenge Overview

As part of infrastructure optimization, the Nautilus DevOps team identified an EC2 instance that was consuming more resources than required.

To optimize cloud costs and improve resource utilization, the instance type needs to be changed from **t2.micro** to **t2.nano** while ensuring the instance remains healthy and operational.

---

# 🎯 Objectives

* Modify the EC2 instance type from **t2.micro** to **t2.nano**
* Ensure the instance remains available after modification
* Verify instance health before and after the change
* Perform all operations in the **us-east-1** region
* Understand EC2 resizing and cost optimization concepts

---

# ☁️ AWS Resource Details

| Property              | Value             |
| --------------------- | ----------------- |
| Service               | Amazon EC2        |
| Instance Name         | xfusion-ec2       |
| Current Instance Type | t2.micro          |
| Target Instance Type  | t2.nano           |
| Region                | us-east-1         |
| Purpose               | Cost Optimization |

---

# 🧠 Why Change an Instance Type?

EC2 instance resizing helps organizations:

* Reduce infrastructure costs
* Improve resource utilization
* Eliminate over-provisioning
* Match resources with workload requirements
* Optimize cloud spending

### Example

If an application consistently consumes:

```text
CPU Usage: Low
Memory Usage: Low
```

Running it on a larger instance type increases unnecessary AWS costs.

A smaller instance type can provide sufficient resources while reducing monthly expenses.

---

# ⚙️ Prerequisites

Before making changes:

* AWS Console access
* Appropriate IAM permissions
* Existing EC2 instance
* Correct AWS region selected
* Status checks completed successfully

---

# 🧭 Step-by-Step Implementation

## Step 1: Login to AWS Console

* Open AWS Management Console
* Navigate to EC2 Dashboard
* Select Region: **us-east-1**

---

## Step 2: Verify Instance Health

Navigate to:

```text
EC2 → Instances
```

Select:

```text
xfusion-ec2
```

Verify:

| Check          | Expected Status |
| -------------- | --------------- |
| Instance State | Running         |
| Status Checks  | 2/2 Passed      |

⚠️ If status checks are still initializing, wait until both checks pass.

---

## Step 3: Stop the Instance

AWS requires the instance to be stopped before modifying the instance type.

Navigate to:

```text
Instance State → Stop Instance
```

Wait until the instance state becomes:

```text
Stopped
```

---

## Step 4: Modify Instance Type

Navigate to:

```text
Actions
  └── Instance Settings
         └── Change Instance Type
```

Select:

```text
t2.nano
```

Click:

```text
Apply
```

---

## Step 5: Start the Instance

Navigate to:

```text
Instance State → Start Instance
```

Wait until:

```text
Running
```

---

## Step 6: Validate the Changes

Confirm:

| Validation     | Expected Result |
| -------------- | --------------- |
| Instance Name  | xfusion-ec2     |
| Instance Type  | t2.nano         |
| Instance State | Running         |
| Status Checks  | 2/2 Passed      |

---

# 💻 AWS CLI Method

## Stop Instance

```bash
aws ec2 stop-instances \
  --instance-ids i-xxxxxxxxxxxxxxxxx
```

---

## Change Instance Type

```bash
aws ec2 modify-instance-attribute \
  --instance-id i-xxxxxxxxxxxxxxxxx \
  --instance-type "{\"Value\":\"t2.nano\"}"
```

---

## Start Instance

```bash
aws ec2 start-instances \
  --instance-ids i-xxxxxxxxxxxxxxxxx
```

---

## Verify Instance Type

```bash
aws ec2 describe-instances \
  --instance-ids i-xxxxxxxxxxxxxxxxx \
  --query "Reservations[*].Instances[*].[InstanceType]"
```

---

# 🔍 Verification Checklist

✅ Instance stopped successfully

✅ Instance type changed to **t2.nano**

✅ Instance started successfully

✅ Status checks passed

✅ Application accessible after restart

---

# ⚠️ Common Mistakes

* Attempting to change the instance type while the instance is running
* Performing operations in the wrong AWS region
* Not verifying status checks before modification
* Selecting an incompatible instance type
* Forgetting that public IP addresses may change after restart (unless using Elastic IP)

---

# 💡 Key Learnings

* EC2 instances can be resized based on workload requirements.
* Proper sizing helps reduce AWS costs.
* Instance types determine CPU, memory, and networking capacity.
* Instances must be stopped before changing the instance type.
* Health checks should always be validated before and after modifications.

---

# 🧩 Real-World Use Case

A development server was initially deployed using:

```text
t2.micro
```

After monitoring utilization metrics, it was found that the server was using only a small percentage of its allocated resources.

The instance was resized to:

```text
t2.nano
```

Result:

* Reduced AWS infrastructure cost
* Improved resource efficiency
* No impact on application functionality

---

# 🏁 Outcome

Successfully changed the EC2 instance type from **t2.micro** to **t2.nano**, verified instance health, and implemented a practical AWS cost optimization strategy while maintaining service availability.
