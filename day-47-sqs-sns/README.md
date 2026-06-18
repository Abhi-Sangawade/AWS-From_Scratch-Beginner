# Day 47 – Integrating AWS SQS and SNS for Reliable Messaging

## 📌 Task Overview

The Nautilus DevOps team needed to implement a **priority-based messaging system** using **Amazon SNS**, **Amazon SQS**, and **AWS Lambda**.

The requirement was to

* Create separate queues for **high-priority** and **low-priority** messages
* Use an SNS topic to publish messages
* Route messages to the correct SQS queue based on message attributes
* Process messages using a Lambda function
* Deploy the entire infrastructure using **AWS CloudFormation**

---

# 🏗️ Architecture

```text
                +----------------------+
                |      SNS Topic       |
                | devops-Priority...   |
                +----------+-----------+
                           |
          +----------------+----------------+
          |                                 |
   High Priority Filter              Low Priority Filter
          |                                 |
+----------------------+       +----------------------+
| High Priority Queue  |       | Low Priority Queue   |
| devops-High...       |       | devops-Low...        |
+----------+-----------+       +----------+-----------+
           \                           /
            \                         /
             \                       /
              +-------------------+
              | Lambda Function   |
              | devops-priorities |
              +-------------------+
```

---

# 📂 CloudFormation Template Location

Create the CloudFormation template on the AWS client host:

```bash
/root/devops-priority-stack.yml
```

Stack Name:

```bash
devops-priority-stack
```

---

# 📄 Complete CloudFormation Template

Save the following content into:

```bash
/root/devops-priority-stack.yml
```

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: SNS + SQS Priority Queue Stack with Lambda

Resources:

  HighPriorityQueue:
    Type: AWS::SQS::Queue
    Properties:
      QueueName: devops-High-Priority-Queue

  LowPriorityQueue:
    Type: AWS::SQS::Queue
    Properties:
      QueueName: devops-Low-Priority-Queue

  PriorityTopic:
    Type: AWS::SNS::Topic
    Properties:
      TopicName: devops-Priority-Queues-Topic

  LambdaExecutionRole:
    Type: AWS::IAM::Role
    Properties:
      RoleName: lambda_execution_role
      AssumeRolePolicyDocument:
        Version: '2012-10-17'
        Statement:
          - Effect: Allow
            Principal:
              Service:
                - lambda.amazonaws.com
            Action:
              - sts:AssumeRole

      ManagedPolicyArns:
        - arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole

      Policies:
        - PolicyName: LambdaSQSSNSPolicy
          PolicyDocument:
            Version: '2012-10-17'
            Statement:

              - Effect: Allow
                Action:
                  - sqs:ReceiveMessage
                  - sqs:DeleteMessage
                  - sqs:GetQueueAttributes
                Resource:
                  - !GetAtt HighPriorityQueue.Arn
                  - !GetAtt LowPriorityQueue.Arn

              - Effect: Allow
                Action:
                  - sns:Publish
                Resource: "*"

  PriorityLambdaFunction:
    Type: AWS::Lambda::Function
    Properties:
      FunctionName: devops-priorities-queue-function
      Runtime: python3.9
      Handler: index.lambda_handler
      Timeout: 60
      Role: !GetAtt LambdaExecutionRole.Arn

      Code:
        ZipFile: |
          import json

          def lambda_handler(event, context):
              print("Received Event:")
              print(json.dumps(event))

              for record in event['Records']:
                  body = record['body']
                  print(f"Processing message: {body}")

              return {
                  'statusCode': 200,
                  'body': 'Messages processed successfully'
              }

  HighPrioritySubscription:
    Type: AWS::SNS::Subscription
    Properties:
      TopicArn: !Ref PriorityTopic
      Protocol: sqs
      Endpoint: !GetAtt HighPriorityQueue.Arn
      FilterPolicy:
        priority:
          - high

  LowPrioritySubscription:
    Type: AWS::SNS::Subscription
    Properties:
      TopicArn: !Ref PriorityTopic
      Protocol: sqs
      Endpoint: !GetAtt LowPriorityQueue.Arn
      FilterPolicy:
        priority:
          - low

  HighQueuePolicy:
    Type: AWS::SQS::QueuePolicy
    Properties:
      Queues:
        - !Ref HighPriorityQueue
      PolicyDocument:
        Version: '2012-10-17'
        Statement:
          - Effect: Allow
            Principal: "*"
            Action: sqs:SendMessage
            Resource: !GetAtt HighPriorityQueue.Arn
            Condition:
              ArnEquals:
                aws:SourceArn: !Ref PriorityTopic

  LowQueuePolicy:
    Type: AWS::SQS::QueuePolicy
    Properties:
      Queues:
        - !Ref LowPriorityQueue
      PolicyDocument:
        Version: '2012-10-17'
        Statement:
          - Effect: Allow
            Principal: "*"
            Action: sqs:SendMessage
            Resource: !GetAtt LowPriorityQueue.Arn
            Condition:
              ArnEquals:
                aws:SourceArn: !Ref PriorityTopic

  HighPriorityEventSource:
    Type: AWS::Lambda::EventSourceMapping
    Properties:
      BatchSize: 1
      EventSourceArn: !GetAtt HighPriorityQueue.Arn
      FunctionName: !Ref PriorityLambdaFunction
      Enabled: true

  LowPriorityEventSource:
    Type: AWS::Lambda::EventSourceMapping
    Properties:
      BatchSize: 1
      EventSourceArn: !GetAtt LowPriorityQueue.Arn
      FunctionName: !Ref PriorityLambdaFunction
      Enabled: true
```

---

# 🚀 Deploy the Stack

Run the following command:

```bash
aws cloudformation create-stack \
  --stack-name devops-priority-stack \
  --template-body file:///root/devops-priority-stack.yml \
  --capabilities CAPABILITY_NAMED_IAM
```

---

# 📋 Verify Stack Creation

```bash
aws cloudformation describe-stacks \
  --stack-name devops-priority-stack
```

---

# 📤 Publish Test Messages

## Get SNS Topic ARN

```bash
topicarn=$(aws sns list-topics \
--query "Topics[?contains(TopicArn, 'devops-Priority-Queues-Topic')].TopicArn" \
--output text)
```

---

## Publish High Priority Messages

```bash
aws sns publish \
--topic-arn $topicarn \
--message 'High Priority message 1' \
--message-attributes '{"priority" : { "DataType":"String", "StringValue":"high"}}'
```

```bash
aws sns publish \
--topic-arn $topicarn \
--message 'High Priority message 2' \
--message-attributes '{"priority" : { "DataType":"String", "StringValue":"high"}}'
```

---

## Publish Low Priority Messages

```bash
aws sns publish \
--topic-arn $topicarn \
--message 'Low Priority message 1' \
--message-attributes '{"priority" : { "DataType":"String", "StringValue":"low"}}'
```

```bash
aws sns publish \
--topic-arn $topicarn \
--message 'Low Priority message 2' \
--message-attributes '{"priority" : { "DataType":"String", "StringValue":"low"}}'
```

---

# 🔍 Verify Lambda Processing

Check Lambda logs:

```bash
aws logs describe-log-groups
```

Get the latest log stream:

```bash
aws logs describe-log-streams \
--log-group-name /aws/lambda/devops-priorities-queue-function \
--order-by LastEventTime \
--descending
```

View logs:

```bash
aws logs get-log-events \
--log-group-name /aws/lambda/devops-priorities-queue-function \
--log-stream-name <log-stream-name>
```

---

# ✅ Expected Result

* SNS topic receives messages
* Filter policies route messages correctly
* High-priority messages go to the high-priority queue
* Low-priority messages go to the low-priority queue
* Lambda function consumes messages from both queues
* High-priority messages are processed first

---

# 🧠 Key Concepts Covered

* Amazon SNS
* Amazon SQS
* SNS Subscription Filter Policies
* AWS Lambda Event Source Mapping
* IAM Roles & Policies
* Infrastructure as Code (IaC)
* AWS CloudFormation
* Event-Driven Architecture

---

# 🎯 Outcome

Successfully implemented a **priority-based event-driven architecture** using AWS managed services with complete infrastructure automation through CloudFormation.
