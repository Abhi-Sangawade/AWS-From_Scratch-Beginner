---

# 📅 Day 34: Create a Lambda Function Using CLI

---

## 🧠 Task

* Create a Python script → `lambda_function.py`
* Return response

  * Status code → 200
  * Body → *Welcome to KKE AWS Labs!*
* Zip the script → `function.zip`
* Create Lambda function → `xfusion-lambda-cli`
* Runtime → Python
* IAM Role → `lambda_execution_role`

---

## 🎯 Objective

* Understand AWS Lambda (serverless compute)
* Learn Lambda deployment using AWS CLI
* Package and deploy code using ZIP
* Use IAM role for execution

---

## ☁️ AWS Details

* Service: AWS Lambda
* Region: us-east-1
* Function Name: `xfusion-lambda-cli`
* Runtime: Python
* IAM Role: `lambda_execution_role`

---

# 🚀 Steps to Execute (AWS CLI)

---

# 🟢 PART 1: Create Python Script

---

## 🔹 Step 1: Create File

```bash
vi lambda_function.py
```

---

## 🔹 Step 2: Add Code

```python
def lambda_handler(event, context):
    return {
        'statusCode': 200,
        'body': 'Welcome to KKE AWS Labs!'
    }
```

---

## 🔹 Step 3: Save File

```bash
:wq
```

---

# 🟡 PART 2: Zip the Script

---

## 🔹 Step 4: Create ZIP File

```bash
zip function.zip lambda_function.py
```

---

# 🔵 PART 3: Create Lambda Function

---

## 🔹 Step 5: Create Function Using CLI

```bash
aws lambda create-function \
--function-name xfusion-lambda-cli \
--runtime python3.8 \
--role arn:aws:iam::<account-id>:role/lambda_execution_role \
--handler lambda_function.lambda_handler \
--zip-file fileb://function.zip \
--region us-east-1
```

---

## 🔹 Step 6: Verify Function

```bash
aws lambda get-function \
--function-name xfusion-lambda-cli
```

---

# 🔍 PART 4: Test the Lambda Function

---

## 🔹 Step 7: Invoke Function

```bash
aws lambda invoke \
--function-name xfusion-lambda-cli \
output.json
```

---

## 🔹 Step 8: Check Output

```bash
cat output.json
```

Expected:

```json
{
  "statusCode": 200,
  "body": "Welcome to KKE AWS Labs!"
}
```

---

# 💡 Key Learning

* Lambda enables serverless execution without managing servers
* Functions are deployed using ZIP packages
* IAM roles control execution permissions
* CLI helps automate deployments

---

# ⚠️ Challenges Faced

* Correct handler format (`file.function`)
* Providing correct IAM role ARN
* Packaging ZIP file properly

---

# 🔧 Fix / Learning

* Used correct handler → `lambda_function.lambda_handler`
* Ensured IAM role exists before creation
* Verified function using CLI

---

# 🧩 Summary

Successfully created a Lambda function `xfusion-lambda-cli` using AWS CLI by packaging a Python script into a ZIP file and assigning the appropriate IAM role. Verified execution by invoking the function and validating the response.

---
