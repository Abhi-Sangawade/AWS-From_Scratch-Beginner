---

# 📅 Day 43: Scaling and Managing Kubernetes Clusters with Amazon EKS

---

## 🧠 Task

* Create EKS cluster → `devops-eks`
* Use IAM role → `eksClusterRole`
* Configure **private endpoint access**
* Use **default VPC** with AZs → a, b, c
* Disable **EKS Auto Mode**
* Verify cluster readiness

---

## 🎯 Objective

* Understand Amazon EKS (Managed Kubernetes)
* Configure secure cluster access (private endpoint)
* Ensure high availability using multiple AZs
* Prepare cluster for production workloads

---

## ☁️ AWS Details

* Service: Amazon EKS
* Region: us-east-1
* Cluster Name: `devops-eks`
* IAM Role: `eksClusterRole`
* VPC: Default VPC
* Availability Zones: us-east-1a, us-east-1b, us-east-1c

---

# 🚀 Architecture Overview

```text id="eksflow1"
User → kubectl → EKS Control Plane → Worker Nodes (AZ a, b, c)
```

---

# 🟢 PART 1: Prerequisites

---

## 🔹 Step 1: Verify IAM Role

* Go to **IAM → Roles**
* Ensure role exists → `eksClusterRole`

👉 Must have policies:

* AmazonEKSClusterPolicy
* AmazonEKSServicePolicy

---

# 🟡 PART 2: Create EKS Cluster (Console)

---

## 🔹 Step 2: Create Cluster

1. Go to **EKS → Clusters → Create cluster**
2. Choose → **Custom configuration**

---

## 🔹 Step 3: Basic Configuration

* Name → `devops-eks`
* Kubernetes version → **Latest stable**
* Cluster IAM role → `eksClusterRole`

---

# 🔵 PART 3: Networking Configuration

---

## 🔹 Step 4: VPC Setup

* Select → **Default VPC**
* Select subnets from:

  * us-east-1a
  * us-east-1b
  * us-east-1c

---

## 🔹 Step 5: Endpoint Access

Set:

```text id="eks1"
Public access → Disabled  
Private access → Enabled
```

👉 Ensures cluster is not exposed to internet

---

# 🟣 PART 4: Cluster Settings

---

## 🔹 Step 6: Additional Settings

* Disable → **EKS Auto Mode**
* Keep logging default (optional)

---

## 🔹 Step 7: Create Cluster

* Click **Create**
* Wait for status:

```text id="eks2"
ACTIVE
```

---

# 🔍 PART 5: Verification

---

## 🔹 Step 8: Verify Cluster Status

* Go to **EKS → Clusters → devops-eks**

```text id="eks3"
Status: Active
```

---

## 🔹 Step 9: Verify Endpoint Access

```text id="eks4"
Endpoint: Private only
```

---

## 🔹 Step 10: Check Networking

* Subnets across 3 AZs
* High availability ensured

---

# 💡 Key Learning

* EKS is a managed Kubernetes service
* Private endpoint improves security
* Multi-AZ setup ensures high availability
* IAM roles control cluster permissions

---

# ⚠️ Challenges Faced

* Confusion between public vs private endpoint
* Selecting correct subnets across AZs
* IAM role permission setup

---

# 🔧 Fix / Learning

* Used private endpoint for secure cluster access
* Verified IAM role policies before creation
* Selected subnets across multiple AZs

---

# 🧩 Summary

Successfully created secure EKS cluster `devops-eks` using default VPC across multiple AZs, configured private endpoint access, and verified cluster readiness for Kubernetes workloads.

---
