# Day 48: Automating Infrastructure Deployment with AWS CloudFormation

---

## 📌 Task

The Nautilus DevOps team needs to implement a Lambda function using a CloudFormation stack.

Requirements:

* Create CloudFormation template → `/root/xfusion-lambda.yml`
* Create stack → `xfusion-lambda-app`
* Create Lambda function → `xfusion-lambda`
* Runtime → Python
* Function should return:

```text
Welcome to KKE AWS Labs!
```

* Status code → `200`
* Create and use IAM role → `lambda_execution_role`

---

## 🎯 Objective

This task demonstrates:

* Infrastructure as Code (IaC)
* CloudFormation stack deployment
* Lambda automation
* IAM role creation through CloudFormation
* Infrastructure provisioning using YAML templates

---

## ☁️ AWS Services Used

| Service        | Resource              |
| -------------- | --------------------- |
| CloudFormation | xfusion-lambda-app    |
| Lambda         | xfusion-lambda        |
| IAM            | lambda_execution_role |

---

## 🏗 Architecture

```text
CloudFormation Stack
        ↓
IAM Role
        ↓
Lambda Function
        ↓
Execute Python Code
        ↓
Return Response
```

---

# 🚀 Step 1: Create CloudFormation Template

Create template file:

```bash
vi /root/xfusion-lambda.yml
```

Add the following YAML:

```yaml
Resources:

  LambdaExecutionRole:
    Type: AWS::IAM::Role
    Properties:
      RoleName: lambda_execution_role
      AssumeRolePolicyDocument:
        Version: '2012-10-17'
        Statement:
          - Effect: Allow
            Principal:
              Service: lambda.amazonaws.com
            Action: sts:AssumeRole

      ManagedPolicyArns:
       - arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole


  XFusionLambdaFunction:
    Type: AWS::Lambda::Function
    Properties:
      FunctionName: xfusion-lambda
      Runtime: python3.9
      Handler: index.lambda_handler
      Role: !GetAtt LambdaExecutionRole.Arn
      Timeout: 5

      Code:
        ZipFile: |
          def lambda_handler(event, context):
              return {
                  "statusCode": 200,
                  "body": "Welcome to KKE AWS Labs!"
              }
```

Save file.

---

# 🚀 Step 2: Validate Template

Validate before deployment:

```bash
aws cloudformation validate-template \
--template-body file:///root/xfusion-lambda.yml
```

Expected output:

```json
{
   "Parameters":[]
}
```

## Note

This output indicates:

* YAML syntax is valid
* CloudFormation structure is valid
* No template parameters exist

---

# 🚀 Step 3: Create CloudFormation Stack

Run:

```bash
aws cloudformation create-stack \
--stack-name xfusion-lambda-app \
--template-body file:///root/xfusion-lambda.yml \
--capabilities CAPABILITY_NAMED_IAM
```

Expected:

```json
{
   "StackId":"arn:aws:cloudformation:..."
}
```

---

# 🚀 Step 4: Verify Stack Creation

Check status:

```bash
aws cloudformation describe-stacks \
--stack-name xfusion-lambda-app \
--query "Stacks[0].StackStatus" \
--output text
```

Expected:

```text
CREATE_COMPLETE
```

---

# 🚀 Step 5: Verify Lambda Function

List Lambda functions:

```bash
aws lambda list-functions
```

Expected:

```text
xfusion-lambda
```

---

# 🚀 Step 6: Test Lambda Function

Invoke Lambda:

```bash
aws lambda invoke \
--function-name xfusion-lambda \
response.json
```

View response:

```bash
cat response.json
```

Expected:

```json
{
   "statusCode":200,
   "body":"Welcome to KKE AWS Labs!"
}
```

---

# ⚠️ Common Errors Faced

## Error: Unsupported Structure

```text
Template format error: unsupported structure
```

### Cause

Incorrect file path syntax.

Wrong:

```bash
file:/root/xfusion-lambda.yml
```

Correct:

```bash
file:///root/xfusion-lambda.yml
```

---

## Error: Stack Already Exists

```text
AlreadyExistsException
```

### Cause

Previous stack exists.

Fix:

```bash
aws cloudformation delete-stack \
--stack-name xfusion-lambda-app
```

Then recreate stack.

---

## Error: Stack entered ROLLBACK_COMPLETE

### Verify:

```bash
aws cloudformation describe-stack-events \
--stack-name xfusion-lambda-app
```

Common causes:

* YAML indentation issue
* Missing IAM permissions
* Invalid runtime
* Existing resource conflicts

---

# 💡 Key Learning

* CloudFormation automates infrastructure deployment
* IAM resources can be created directly inside templates
* `!GetAtt` dynamically references generated resource attributes
* Template validation checks syntax before deployment
* Stack events help debug deployment failures

---

# ✅ Summary

Successfully deployed:

```text
Stack → xfusion-lambda-app
IAM Role → lambda_execution_role
Lambda Function → xfusion-lambda
```

Verified Lambda response:

```text
Status Code: 200
Message: Welcome to KKE AWS Labs!
```
