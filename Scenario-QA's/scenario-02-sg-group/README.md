# 🔥 Security Group Scenario-Based Questions & Answers

---

# Scenario 01: Unable to SSH into EC2 Instance

## Problem Statement

An EC2 instance is running and healthy.

You are trying to connect using:

```bash
ssh -i key.pem ec2-user@PUBLIC_IP
```

But the connection times out.

---

## Investigation

Check:

* Instance state
* Public IP
* Internet Gateway
* Route Table
* Security Group

Security Group found:

```text
Inbound Rules

HTTP 80     0.0.0.0/0
HTTPS 443   0.0.0.0/0
```

Missing:

```text
SSH 22
```

---

## Root Cause

Port 22 is not allowed in the Security Group.

---

## Fix

Add:

```text
Type: SSH
Protocol: TCP
Port: 22
Source: Your Public IP
```

Example:

```text
203.0.113.10/32
```

---

## Interview Question

Why would SSH fail even if the EC2 instance is running?

### Answer

SSH fails when:

* Port 22 is blocked in Security Group
* NACL blocks traffic
* Wrong key pair
* Instance has no public IP
* Route table issue

---

# Scenario 02: Website Not Accessible on Port 80

## Problem Statement

Nginx is running successfully.

Application is listening on:

```text
Port 80
```

Users receive:

```text
Connection Timed Out
```

---

## Investigation

Security Group:

```text
SSH 22
```

No HTTP rule.

---

## Root Cause

Port 80 blocked by Security Group.

---

## Fix

Add:

```text
HTTP
Port: 80
Source: 0.0.0.0/0
```

---

## Interview Question

What is the difference between a running service and accessibility?

### Answer

A service may run successfully inside the server, but Security Groups can prevent external access.

---

# Scenario 03: Application Running on Port 8080 Not Reachable

## Problem Statement

Application is listening:

```text
0.0.0.0:8080
```

Browser:

```text
http://PUBLIC-IP:8080
```

returns timeout.

---

## Investigation

Security Group:

```text
22
80
443
```

Port 8080 missing.

---

## Root Cause

Security Group blocks custom application port.

---

## Fix

Add:

```text
Custom TCP
Port: 8080
Source: Required CIDR
```

---

# Scenario 04: Database Connection Failure

## Problem Statement

Application server cannot connect to MySQL RDS.

Error:

```text
Connection refused
```

---

## Investigation

RDS Security Group:

```text
3306
Source: 0.0.0.0/0
```

or no rule exists.

---

## Best Practice Fix

Allow only application Security Group.

Example:

```text
Type: MYSQL/Aurora
Port: 3306
Source: sg-appserver
```

---

## Interview Question

Why use Security Group references instead of IP addresses?

### Answer

Security Group references are dynamic and more secure because access follows instances automatically.

---

# Scenario 05: Load Balancer Health Check Failing

## Problem Statement

ALB shows:

```text
Unhealthy Targets
```

---

## Investigation

Application works locally.

Security Group:

```text
80 from Internet
```

Missing:

```text
80 from ALB Security Group
```

---

## Root Cause

EC2 Security Group does not allow traffic from ALB.

---

## Fix

Allow:

```text
HTTP 80
Source: ALB Security Group
```

---

# Scenario 06: EC2 Can Access Internet but Internet Cannot Access EC2

## Problem Statement

Instance can:

```bash
yum update
```

Successfully.

But users cannot access application.

---

## Root Cause

Outbound rule exists.

Inbound rule missing.

---

## Interview Question

What is the default behavior of Security Groups?

### Answer

Security Groups are:

* Stateful
* Deny inbound by default
* Allow outbound by default

---

# Scenario 07: RDP Access Not Working for Windows Server

## Problem Statement

Cannot connect via Remote Desktop.

---

## Investigation

Port:

```text
3389
```

not allowed.

---

## Fix

Add:

```text
RDP
Port 3389
Source: Your Public IP
```

---

# Scenario 08: Security Group vs NACL Troubleshooting

## Problem Statement

Security Group allows SSH.

Still unable to connect.

---

## Investigation

NACL:

```text
Inbound 22 Allowed
Outbound 22 Denied
```

---

## Root Cause

NACL is stateless.

Return traffic blocked.

---

## Interview Question

Difference between Security Group and NACL?

### Answer

| Security Group                       | NACL                                      |
| ------------------------------------ | ----------------------------------------- |
| Stateful                             | Stateless                                 |
| Instance Level                       | Subnet Level                              |
| Allow Rules Only                     | Allow & Deny Rules                        |
| Return Traffic Automatically Allowed | Return Traffic Must Be Explicitly Allowed |

---

# Scenario 09: Application Exposed to Entire Internet

## Problem Statement

Database Security Group:

```text
3306
Source: 0.0.0.0/0
```

---

## Risk

Anyone on the internet can attempt database connections.

---

## Fix

Use:

```text
Source: Application Security Group
```

instead of:

```text
0.0.0.0/0
```

---

# Scenario 10: Multi-Tier Architecture Security Group Design

## Architecture

```text
Internet
   |
ALB
   |
Web Server
   |
Application Server
   |
Database
```

---

## Recommended Security Groups

### ALB SG

```text
80,443 from 0.0.0.0/0
```

### Web SG

```text
80,443 from ALB SG
```

### App SG

```text
8080 from Web SG
```

### DB SG

```text
3306 from App SG
```

---

## Interview Question

How would you secure a three-tier application using Security Groups?

### Answer

Use layered Security Groups and allow communication only between required tiers instead of exposing services directly to the internet.

