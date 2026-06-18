---

# 🚀 Day 33 – Create an AWS Lambda Function

## 📌 Task Overview

The Nautilus DevOps team is adopting **serverless architecture** using Amazon Web Services Lambda to simplify operations and improve scalability.

As part of this initiative, we created a basic Lambda function that returns a custom greeting. This demonstrates

* ⚡ Rapid deployment
* 📈 Automatic scaling
* 💰 Cost efficiency (pay-per-use)

---

## 🎯 Objective

Create a Lambda function with the following specifications

| Requirement       | Details                  |
| ----------------- | ------------------------ |
| Function Name     | `datacenter-lambda`      |
| Runtime           | Python                   |
| Response Body     | Welcome to KKE AWS Labs! |
| Status Code       | 200                      |
| IAM Role          | `lambda_execution_role`  |
| Deployment Method | AWS Console              |

---

## 🛠️ Step-by-Step Implementation

### 1️⃣ Create IAM Role

1. Navigate to **IAM → Roles → Create Role**
2. Select **AWS Service → Lambda**
3. Attach policy:

   * `AWSLambdaBasicExecutionRole`
4. Name the role:

   ```
   lambda_execution_role
   ```
5. Click **Create Role**

---

### 2️⃣ Create Lambda Function

1. Go to **Lambda → Create Function**
2. Choose **Author from scratch**
3. Configure:

   * Function name: `datacenter-lambda`
   * Runtime: Python (latest available version)
   * Execution role: Use existing role → `lambda_execution_role`
4. Click **Create Function**

---

### 3️⃣ Write Lambda Code

Replace the default code with

```python
def lambda_handler(event, context)
    return {
        'statusCode': 200,
        'body': 'Welcome to KKE AWS Labs!'
    }
```

---

### 4️⃣ Deploy the Function

* Click **Deploy** to save changes

---

### 5️⃣ Test the Function

1. Click **Test**
2. Configure a test event (default is fine)
3. Run the test

### ✅ Expected Output

```json
{
  "statusCode": 200,
  "body": "Welcome to KKE AWS Labs!"
}
```

---

## 📸 Architecture Overview (Concept)

![Image](https://images.openai.com/static-rsc-4/RbJMfcFxPIYyo1Tpght--K12LU9L3kxVXuVTX2cRL9YwNNwhFEA_8xfQl03i0yIACn1M8ev8GEanpP1-LGW61HwfmESebZ7h7JF2gdM-3alNkj9N3jLHYvAueuLph8oPcTaaGuQbhWdP2Zmf80L4n20EKLsvgTpCO3Ry0NQW79rHbzqGXjhaidW0NmXpGtDX?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/tTqfYfs9Fx95UCLtb1dB-EyYw3bBTf7V-A3iASvhOCnwszwgVLCmdW_DA1fM1_Mlfxxw4pR9JECTY1_63b1zSbVeCKVuY3Mt4H5SsTSblSxsrELerfmJM4BnQ6bUdQk1DbibKkuHBjg3EUaaw7BMZtQswNh1_OasJwUfstcNkqKeYXc4D5v2DvwayxdtwvQZ?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/gJuBWvf-GRxl_-r9y8U6BYDR5ZBXWRkBxOaaJymVdLJYhvJduRJ7yO2YHH-sB-r4cqmhZRe3KZlWSS430WdFs5EjddDazPnKGdI3Bb4TyZ32nTVG2XBQ3hXRSJNd7QhgwCoVdilrwxEM2iX-aX_mIlNWYYbTO-8dHOGGlDm-3Dz9GprvilXvDq3lKA42bjeX?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/3tBenddAjd6-Z2LqkfE5KrgLjBBTXwFfk3ilGVkicjMGKUD9g_i8znHMRvYoWIVSqco9kTY4rPe0QIqRW7F0Yctda_OcctYqoHSj1m84C9dsqU5AJu8Pz64Jat7hp9r-nZtCVESqIkHCwCCXG6OgDZuqiI_ujlo5b8SpcVP9lqOjzGMRaGxQk4LhDflQzngd?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/6eVK61hc9XbLOo74fgHEH7QpbXrCNnnZWEHmUfeczCg4x85gWtOiIbgBV_aV6mORu9Gm3POgkqDOtaMjlzYcVCYzcBWCnGk0ksOdLezoM6oozm37p3uCm3_BwiR2_n_P0J2NESG-_3WfkjLnPg7MNWTqa_h3YYq8ZD5RS_SVGuhbNW1vErlOyL4jlbgApXnn?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/QouAlxGi3vlHNcDv_t6Xf31vBRxwheiy5ZKCWy5QXAw8SyGGw_I1ZjANAmhQHf2Ap3irknKE0ncJnw5FZzhnpVKOJ2l8RvgKQrLXXm20hWCltxHCSIfEa2v_6yJYQU6FCi__IGVlrrJylBdEfgASQCTwPyORUy7rIR4NjcPiKxqhZ7Bx5gnVWsoPJy5AxrTX?purpose=fullsize)

---

## 💡 Key Learnings

* ✔️ Lambda eliminates server management
* ✔️ IAM roles control secure execution permissions
* ✔️ Python runtime makes quick prototyping easy
* ✔️ Event-driven architecture enables flexibility

---

## 🧠 Use Cases

* API backends
* Automation scripts
* File processing (S3 triggers)
* Scheduled jobs (CloudWatch Events)

---

## 🏁 Conclusion

This hands-on task demonstrates how quickly a serverless function can be deployed using Amazon Web Services Lambda. It highlights the power of **event-driven, scalable, and cost-effective cloud solutions**.

---
