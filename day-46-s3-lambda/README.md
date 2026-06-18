# 🚀 Day 46: Event-Driven Processing with Amazon S3 and Lambda

Automating file transfers using AWS serverless services ⚡☁️

## 📌 Task Overview

The DevOps team needs an automated solution for securely managing uploaded files between two Amazon S3 buckets.

### Requirements

* Create a **public S3 bucket** for uploads.
* Create a **private S3 bucket** for secure storage.
* Configure an **AWS Lambda function** to automatically copy uploaded files from the public bucket to the private bucket.
* Store operation logs in a **DynamoDB table**.
* Test the complete event-driven workflow using a sample file upload.

---

# 🏗️ Architecture

```text
User Uploads File
        │
        ▼
Public S3 Bucket
(devops-public-28293)
        │
        ▼
S3 Event Trigger
        │
        ▼
AWS Lambda Function
(devops-copyfunction)
        │
 ┌──────┴────────┐
 ▼               ▼
Copy File     Store Logs
to Private    in DynamoDB
S3 Bucket
```

---

# 📋 Resources Created

| Resource Type     | Resource Name         |
| ----------------- | --------------------- |
| Public S3 Bucket  | devops-public-28293   |
| Private S3 Bucket | devops-private-29825  |
| Lambda Function   | devops-copyfunction   |
| IAM Role          | lambda_execution_role |
| DynamoDB Table    | devops-S3CopyLogs     |

---

# 🪣 Step 1: Create Public S3 Bucket

Create a bucket that allows public access for uploaded files.

```bash
aws s3api create-bucket \
--bucket devops-public-28293 \
--region us-east-1
```

Disable public access block:

```bash
aws s3api put-public-access-block \
--bucket devops-public-28293 \
--public-access-block-configuration \
BlockPublicAcls=false,IgnorePublicAcls=false,BlockPublicPolicy=false,RestrictPublicBuckets=false
```

Attach bucket policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadAccess",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::devops-public-28293/*"
    }
  ]
}
```

Apply the policy:

```bash
aws s3api put-bucket-policy \
--bucket devops-public-28293 \
--policy file://public-policy.json
```

---

# 🔒 Step 2: Create Private S3 Bucket

Create a secure private bucket.

```bash
aws s3api create-bucket \
--bucket devops-private-29825 \
--region us-east-1
```

Keep default public access block settings enabled.

---

# 🗄️ Step 3: Create DynamoDB Table

Create a table to store Lambda execution logs.

```bash
aws dynamodb create-table \
--table-name devops-S3CopyLogs \
--attribute-definitions AttributeName=LogID,AttributeType=S \
--key-schema AttributeName=LogID,KeyType=HASH \
--billing-mode PAY_PER_REQUEST
```

---

# 🛡️ Step 4: Create IAM Role for Lambda

Create trust relationship file:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "lambda.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

Create the IAM role:

```bash
aws iam create-role \
--role-name lambda_execution_role \
--assume-role-policy-document file://trust-policy.json
```

---

# 📜 Step 5: Create IAM Policy

Create policy file:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject"
      ],
      "Resource": "arn:aws:s3:::devops-public-28293/*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::devops-private-29825/*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "dynamodb:PutItem"
      ],
      "Resource": "arn:aws:dynamodb:*:*:table/devops-S3CopyLogs"
    },
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents"
      ],
      "Resource": "*"
    }
  ]
}
```

Create the policy:

```bash
aws iam create-policy \
--policy-name lambda-s3-dynamodb-policy \
--policy-document file://lambda-policy.json
```

Attach policy to role:

```bash
aws iam attach-role-policy \
--role-name lambda_execution_role \
--policy-arn arn:aws:iam::<ACCOUNT-ID>:policy/lambda-s3-dynamodb-policy
```

Attach AWS Lambda basic execution policy:

```bash
aws iam attach-role-policy \
--role-name lambda_execution_role \
--policy-arn arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole
```

---

# ⚙️ Step 6: Update Lambda Function Code

Navigate to `/root` directory and edit the file:

```bash
cd /root
vi lambda-function.py
```

Replace:

```python
REPLACE-WITH-YOUR-DYNAMODB-TABLE
```

with:

```python
devops-S3CopyLogs
```

Replace:

```python
REPLACE-WITH-YOUR-PRIVATE-BUCKET
```

with:

```python
devops-private-29825
```

Zip the Lambda function:

```bash
zip function.zip lambda-function.py
```

---

# 🚀 Step 7: Create Lambda Function

Create the Lambda function:

```bash
aws lambda create-function \
--function-name devops-copyfunction \
--runtime python3.9 \
--handler lambda-function.lambda_handler \
--zip-file fileb://function.zip \
--role arn:aws:iam::<ACCOUNT-ID>:role/lambda_execution_role
```

---

# 🔔 Step 8: Configure S3 Event Notification

Allow S3 to invoke Lambda:

```bash
aws lambda add-permission \
--function-name devops-copyfunction \
--principal s3.amazonaws.com \
--statement-id s3invoke \
--action "lambda:InvokeFunction" \
--source-arn arn:aws:s3:::devops-public-28293
```

Create notification configuration file:

```json
{
  "LambdaFunctionConfigurations": [
    {
      "LambdaFunctionArn": "arn:aws:lambda:us-east-1:<ACCOUNT-ID>:function:devops-copyfunction",
      "Events": [
        "s3:ObjectCreated:*"
      ]
    }
  ]
}
```

Apply notification configuration:

```bash
aws s3api put-bucket-notification-configuration \
--bucket devops-public-28293 \
--notification-configuration file://notification.json
```

---

# 🧪 Step 9: Test the Workflow

Upload the sample file:

```bash
aws s3 cp /root/sample.zip s3://devops-public-28293/
```

---

# ✅ Verification Steps

## Verify File Copy

Check the private bucket:

```bash
aws s3 ls s3://devops-private-29825/
```

Expected output:

```text
sample.zip
```

---

## Verify DynamoDB Logs

Scan the DynamoDB table:

```bash
aws dynamodb scan \
--table-name devops-S3CopyLogs
```

Expected log entry includes:

* Source bucket name
* Destination bucket name
* Object key
* Timestamp / LogID

---

# 🎯 Outcome

Successfully implemented an **event-driven serverless architecture** using:

* Amazon S3
* AWS Lambda
* DynamoDB
* IAM Roles & Policies

The solution automatically:

✅ Detects file uploads
✅ Copies files securely
✅ Stores transfer logs
✅ Improves automation and visibility

---

# 📚 Key Learnings

* Configuring S3 event notifications
* Automating workflows using AWS Lambda
* Managing permissions with IAM roles and policies
* Using DynamoDB for lightweight logging
* Building event-driven architectures in AWS

---

# 🏁 Final Result

Whenever a file is uploaded to the public bucket:

1. S3 triggers the Lambda function
2. Lambda copies the file to the private bucket
3. Lambda writes log details into DynamoDB
4. The entire workflow runs automatically without manual intervention 🚀
