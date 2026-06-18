# 📅 Day 28: Creating a Private ECR Repository

---

## 🧠 Task

* Create a private ECR repository → `xfusion-ecr`
* Build a Docker image from `/root/pyapp`
* Tag the image as `latest`
* Push the image to ECR

---

## 🎯 Objective

* Understand Amazon ECR (Elastic Container Registry)
* Learn Docker image build and push workflow
* Work with real DevOps flow (Console + CLI)

---

## ☁️ AWS Details

* Service: Amazon ECR
* Region: us-east-1
* Repository: `xfusion-ecr`
* Image Tag: `latest`
* Source Directory: `/root/pyapp`

---

# 🚀 Execution Flow

```text
AWS Console → Create ECR Repository  
aws-client → Build Docker Image → Tag → Push → Verify in Console
```

---

# 🟢 PART 1: Create ECR Repository (Console)

---

## 🔹 Step 1: Navigate to ECR

* AWS Console → **Elastic Container Registry (ECR)**
* Click → **Create repository**

---

## 🔹 Step 2: Configure Repository

* Visibility → Private
* Repository name → `xfusion-ecr`

✅ Repository created successfully

---

## 🔹 Step 3: Repository Details (Verified)

* Repository URI:

```text
637423303501.dkr.ecr.us-east-1.amazonaws.com/xfusion-ecr
```

* Encryption → AES-256
* Tag mutability → Mutable

---

# 🟡 PART 2: Build & Push Docker Image (aws-client)

---

## 🔹 Step 4: Navigate to Project Directory

```bash
cd /root/pyapp
ls
```

📁 Files present:

* `Dockerfile`
* `app.py`
* `requirements.txt`

---

## 🔹 Step 5: Authenticate Docker to ECR

```bash
aws ecr get-login-password --region us-east-1 | \
docker login --username AWS --password-stdin 637423303501.dkr.ecr.us-east-1.amazonaws.com
```

✅ Output:

```text
Login Succeeded
```

---

## 🔹 Step 6: Build Docker Image

```bash
docker build -t xfusion-ecr .
```

✅ Image built successfully using base image:

```text
python:3.8-slim
```

---

## 🔹 Step 7: Tag Docker Image

```bash
docker tag xfusion-ecr:latest \
637423303501.dkr.ecr.us-east-1.amazonaws.com/xfusion-ecr:latest
```

---

## 🔹 Step 8: Push Image to ECR

```bash
docker push 637423303501.dkr.ecr.us-east-1.amazonaws.com/xfusion-ecr:latest
```

✅ Output:

```text
multiple layers pushed successfully  
latest: digest: sha256:7b5b2f85...  
```

---

# 🔍 PART 3: Verification

---

## ✅ Verify in AWS Console

* Navigate → ECR → `xfusion-ecr` → Images

✔ Image Tag: `latest`
✔ Status: Active
✔ Size: ~49 MB
✔ Push Time: Verified

---

## ✅ Image Details

* URI:

```text
637423303501.dkr.ecr.us-east-1.amazonaws.com/xfusion-ecr:latest
```

* Digest:

```text
sha256:7b5b2f85b5724bb1a79c7584441cca1f036fe24b782c4ccac583242b5bc48050
```

---

# 💡 Key Learning

* ECR is a private Docker registry in AWS
* Docker image must be tagged with repository URI before pushing
* Authentication is required before pushing images
* Console + CLI together form real DevOps workflow

---

# ⚠️ Challenges Faced

* Understanding repository URI vs name
* Docker authentication requirement
* Tagging format mistakes

---

# 🔧 Fix / Learning

* Used correct repository URI
* Authenticated using `get-login-password`
* Verified push using digest and console

---

# 🧩 Summary

Successfully created private ECR repository `xfusion-ecr`, built a Docker image from `/root/pyapp`, and pushed it to ECR with tag `latest`. Verified image presence and integrity using AWS Console.

---
