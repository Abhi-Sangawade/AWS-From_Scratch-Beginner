---

# ☁️ Day 39: Host a Static Website on AWS S3

## 🧠 Problem Statement

The Nautilus DevOps team needs to create an internal information portal that is publicly accessible using **AWS S3 static website hosting**.

---

## 📋 Requirements

* Create an S3 bucket named `xfusion-web-8965`
* Enable static website hosting
* Set `index.html` as the index document
* Allow **public access** to the bucket
* Upload `index.html` from `/root/` on AWS client host
* Verify website accessibility via S3 URL

---

## 🎯 Objective

* Host a static website using S3
* Configure public access permissions
* Deploy and verify website

---

## 🛠️ Implementation Steps

---

### 1️⃣ Create S3 Bucket

```bash
aws s3 mb s3://xfusion-web-8965
```

---

### 2️⃣ Disable Block Public Access

```bash
aws s3api put-public-access-block \
--bucket xfusion-web-8965 \
--public-access-block-configuration \
"BlockPublicAcls=false,IgnorePublicAcls=false,BlockPublicPolicy=false,RestrictPublicBuckets=false"
```

---

### 3️⃣ Enable Static Website Hosting

```bash
aws s3 website s3://xfusion-web-8965/ \
--index-document index.html
```

---

### 4️⃣ Upload Website File

```bash
aws s3 cp /root/index.html s3://xfusion-web-8965/
```

---

### 5️⃣ Set Bucket Policy for Public Access

```bash
aws s3api put-bucket-policy --bucket xfusion-web-8965 --policy '{
  "Version":"2012-10-17",
  "Statement":[
    {
      "Sid":"PublicReadGetObject",
      "Effect":"Allow",
      "Principal":"*",
      "Action":["s3:GetObject"],
      "Resource":["arn:aws:s3:::xfusion-web-8965/*"]
    }
  ]
}'
```

---

### 6️⃣ Get Website URL

```bash
aws s3 website s3://xfusion-web-8965
```

👉 OR format:

```bash
http://xfusion-web-8965.s3-website-<region>.amazonaws.com
```

---

## 🧪 Verification

Open the S3 website URL in a browser:

```bash
curl http://xfusion-web-8965.s3-website-<region>.amazonaws.com
```

---

## 🏁 Final Outcome

* S3 bucket created
* Static website hosting enabled
* Public access configured
* `index.html` uploaded
* Website accessible via S3 URL

---

## 🔍 Key Concepts

| Concept          | Description                          |
| ---------------- | ------------------------------------ |
| S3 Bucket        | Storage container for objects        |
| Static Hosting   | Serving HTML/CSS/JS directly from S3 |
| Bucket Policy    | Controls public/private access       |
| Website Endpoint | Public URL for hosted content        |

---

## 💡 Key Learnings

* Hosting static websites using AWS S3
* Managing bucket permissions securely
* Configuring public access policies
* Using AWS CLI for automation
* Understanding S3 website endpoints

---

## 🚀 Summary

Deployed a static website on AWS S3 by configuring public access and enabling website hosting — a simple yet powerful way to host lightweight web applications without managing servers.

---
