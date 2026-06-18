---

# 📅 Day 42: Building and Managing NoSQL Databases with AWS DynamoDB

---

## 🧠 Task

* Create DynamoDB table → `datacenter-tasks`
* Primary key → `taskId` (String)
* Insert two tasks
* Verify task status values

---

## 🎯 Objective

* Understand NoSQL databases (DynamoDB)
* Create and manage tables
* Insert and retrieve items
* Work with key-value data models

---

## ☁️ AWS Details

* Service: DynamoDB
* Region: us-east-1
* Table Name: `datacenter-tasks`
* Primary Key: `taskId` (String)

---

# 🚀 PART 1: Create DynamoDB Table

---

## 🔹 Step 1: Create Table (Console)

1. Go to **DynamoDB → Tables → Create table**
2. Table name → `datacenter-tasks`
3. Partition key → `taskId` (String)
4. Keep default settings
5. Click **Create table**

---

## 🔹 Step 2: Verify Table Status

Ensure table status:

```text id="dynamo1"
Active
```

---

# 🟢 PART 2: Insert Data

---

## 🔹 Step 3: Add Task 1

1. Open table → `datacenter-tasks`
2. Go to **Explore table items → Create item**

Add JSON:

```json id="dynamo2"
{
  "taskId": "1",
  "description": "Learn DynamoDB",
  "status": "completed"
}
```

---

## 🔹 Step 4: Add Task 2

```json id="dynamo3"
{
  "taskId": "2",
  "description": "Build To-Do App",
  "status": "in-progress"
}
```

---

# 🔍 PART 3: Verify Data

---

## 🔹 Step 5: Check Items

* Go to **Explore items**

Expected:

```text id="dynamo4"
Task 1 → status: completed  
Task 2 → status: in-progress
```

---

## 🔹 Step 6: Query Individual Items

Use console filter:

* taskId = 1
* taskId = 2

---

# 💡 Key Learning

* DynamoDB is a NoSQL key-value database
* Tables require only a primary key
* Flexible schema allows dynamic attributes
* High scalability and low latency

---

# ⚠️ Challenges Faced

* Understanding NoSQL vs relational databases
* JSON structure while inserting items
* Identifying primary key usage

---

# 🔧 Fix / Learning

* Used correct partition key (`taskId`)
* Inserted data in JSON format
* Verified items using console

---

# 🧩 Summary

Successfully created DynamoDB table `datacenter-tasks`, inserted task records, and verified their status values (`completed` and `in-progress`) using AWS Console.

---
