---

# 📅 Day 38: Deploying Containerized Applications with Amazon ECS

---

## 🧠 Task

* Create private ECR repository → `datacenter-ecr`
* Build Docker image from `/root/pyapp`
* Tag image → `latest`
* Push image to ECR
* Create ECS cluster → `datacenter-cluster` (Fargate)
* Create task definition → `datacenter-taskdefinition`
* Deploy service → `datacenter-service`

---

## 🎯 Objective

* Understand container-based deployments in AWS
* Use ECR for image storage
* Deploy containers using ECS Fargate
* Run applications without managing servers

---

## ☁️ AWS Details

* Service: ECR + ECS
* Region: us-east-1
* ECR Repo: `datacenter-ecr`
* ECS Cluster: `datacenter-cluster`
* Task Definition: `datacenter-taskdefinition`
* Service: `datacenter-service`

---

# 🚀 Architecture Overview

```text id="ecsflow1"
Docker → ECR → ECS (Fargate) → Running Container
```

---

# 🟢 PART 1: Create ECR Repository

---

## 🔹 Step 1: Create Repository

1. Go to **ECR → Create repository**
2. Visibility → **Private**
3. Name → `datacenter-ecr`
4. Click **Create**

---

## 🔹 Step 2: Note Repository URI

👉 Example:

```text id="ecsuri"
<account-id>.dkr.ecr.us-east-1.amazonaws.com/datacenter-ecr
```

---

# 🟡 PART 2: Build and Push Docker Image

---

## 🔹 Step 3: Navigate to Dockerfile

```bash id="ecs1"
cd /root/pyapp
```

---

## 🔹 Step 4: Build Docker Image

```bash id="ecs2"
docker build -t datacenter-ecr .
```

---

## 🔹 Step 5: Authenticate to ECR

```bash id="ecs3"
aws ecr get-login-password --region us-east-1 | \
docker login --username AWS --password-stdin <account-id>.dkr.ecr.us-east-1.amazonaws.com
```

---

## 🔹 Step 6: Tag Image

```bash id="ecs4"
docker tag datacenter-ecr:latest <account-id>.dkr.ecr.us-east-1.amazonaws.com/datacenter-ecr:latest
```

---

## 🔹 Step 7: Push Image

```bash id="ecs5"
docker push <account-id>.dkr.ecr.us-east-1.amazonaws.com/datacenter-ecr:latest
```

---

# 🔵 PART 3: Create ECS Cluster (Fargate)

---

## 🔹 Step 8: Create Cluster

1. Go to **ECS → Clusters → Create cluster**
2. Select → **Fargate**
3. Name → `datacenter-cluster`
4. Click **Create**

---

# 🟣 PART 4: Create Task Definition

---

## 🔹 Step 9: Create Task Definition

1. Go to **ECS → Task Definitions → Create**
2. Launch type → **Fargate**
3. Name → `datacenter-taskdefinition`

---

## 🔹 Step 10: Configure Task

* CPU → 256
* Memory → 512

---

## 🔹 Step 11: Add Container

* Container name → `datacenter-container`
* Image → ECR image URI
* Port mapping → 80

---

# 🔴 PART 5: Create ECS Service

---

## 🔹 Step 12: Create Service

1. Go to **ECS → Cluster → datacenter-cluster**
2. Click **Create service**

---

## 🔹 Step 13: Configure Service

* Launch type → Fargate
* Task definition → `datacenter-taskdefinition`
* Service name → `datacenter-service`
* Desired tasks → 1

---

## 🔹 Step 14: Networking

* Select VPC + subnets
* Assign public IP → Enabled (if needed)
* Attach security group → allow HTTP

---

## 🔹 Step 15: Deploy Service

* Click **Create service**

---

# 🔍 PART 6: Verification

---

## 🔹 Step 16: Check Running Tasks

* ECS → Cluster → Services

```text id="ecsverify1"
Running tasks: 1
```

---

## 🔹 Step 17: Access Application

* Use public IP or load balancer (if configured)

---

# 💡 Key Learning

* ECR stores Docker images securely
* ECS Fargate removes server management
* Task definitions define container behavior
* Services ensure desired number of tasks

---

# ⚠️ Challenges Faced

* ECR authentication issues
* Incorrect image tagging
* Task not starting due to config issues
* Networking misconfiguration

---

# 🔧 Fix / Learning

* Used correct ECR login command
* Verified image URI before deployment
* Configured proper CPU/memory
* Ensured security group allows traffic

---

# 🧩 Summary

Successfully built and pushed a Docker image to ECR (`datacenter-ecr`), created ECS cluster (`datacenter-cluster`), defined task (`datacenter-taskdefinition`), and deployed service (`datacenter-service`) using Fargate to run containerized application.

---
