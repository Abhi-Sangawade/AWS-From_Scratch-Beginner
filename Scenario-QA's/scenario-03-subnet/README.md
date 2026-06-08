# 🌐 AWS Subnet Scenario-Based Interview Questions & Answers

---

# Scenario 01: EC2 Instance in Public Subnet Has No Internet Access

## Problem Statement

An EC2 instance is launched in a Public Subnet.

A Public IP is assigned.

Unable to:

```bash
yum update -y
```

or

```bash
ping google.com
```

---

## Investigation

Subnet Route Table:

```text
10.0.0.0/16 → local
```

Missing:

```text
0.0.0.0/0 → Internet Gateway
```

---

## Root Cause

Internet Gateway route is missing.

---

## Fix

Add:

```text
0.0.0.0/0 → igw-xxxxxxxx
```

---

## Interview Question

What makes a subnet public?

### Answer

A subnet is considered public when:

* Route table contains:

```text
0.0.0.0/0 → Internet Gateway
```

* EC2 instance has a Public IP or Elastic IP.

---

# Scenario 02: Private Subnet Cannot Download Packages

## Problem Statement

EC2 launched in Private Subnet.

Unable to:

```bash
yum update
apt update
```

---

## Investigation

Route Table:

```text
10.0.0.0/16 → local
```

No NAT Gateway.

---

## Root Cause

Private subnet has no outbound internet access.

---

## Fix

Create:

```text
NAT Gateway
```

Public Route Table:

```text
0.0.0.0/0 → Internet Gateway
```

Private Route Table:

```text
0.0.0.0/0 → NAT Gateway
```

---

## Interview Question

Why do Private Subnets require NAT Gateway?

### Answer

Private instances need outbound internet access for updates and downloads while remaining inaccessible from the internet.

---

# Scenario 03: Public Subnet EC2 Not Reachable from Internet

## Problem Statement

Instance has:

```text
Public Subnet
Public IP
Security Group Open
```

Still cannot SSH.

---

## Investigation

Route Table:

```text
0.0.0.0/0 → NAT Gateway
```

---

## Root Cause

Public subnet incorrectly routes traffic through NAT Gateway.

---

## Fix

Change route:

```text
0.0.0.0/0 → Internet Gateway
```

---

# Scenario 04: Application Servers Exposed to Internet

## Problem Statement

Application servers should be private.

Users can access:

```text
http://Public-IP:8080
```

---

## Investigation

Application servers deployed in Public Subnet.

---

## Root Cause

Incorrect subnet placement.

---

## Fix

Move:

```text
Application Servers
Database Servers
```

to:

```text
Private Subnet
```

---

## Interview Question

Which resources should be placed in Private Subnets?

### Answer

* Application Servers
* Databases
* Internal Services
* Backend APIs

---

# Scenario 05: RDS Database Accessible from Internet

## Problem Statement

Security audit finds:

```text
Database publicly accessible
```

---

## Investigation

RDS deployed in Public Subnet.

---

## Root Cause

Database subnet design issue.

---

## Fix

Deploy RDS in:

```text
Private Subnet
```

Disable:

```text
Publicly Accessible = No
```

---

## Interview Question

Why should databases be deployed in Private Subnets?

### Answer

Databases should never be directly exposed to the internet for security reasons.

---

# Scenario 06: NAT Gateway Created but Internet Still Not Working

## Problem Statement

Private instances still cannot access internet.

---

## Investigation

NAT Gateway exists.

Public Subnet Route Table:

```text
10.0.0.0/16 → local
```

Missing:

```text
0.0.0.0/0 → Internet Gateway
```

---

## Root Cause

NAT Gateway itself has no internet access.

---

## Fix

Configure Public Subnet Route Table:

```text
0.0.0.0/0 → Internet Gateway
```

---

# Scenario 07: Multi-AZ Application Failure

## Problem Statement

One Availability Zone goes down.

Entire application becomes unavailable.

---

## Investigation

All resources deployed in:

```text
Subnet-A
AZ-1a
```

---

## Root Cause

No subnet redundancy.

---

## Fix

Create:

```text
Public Subnet A
Public Subnet B

Private Subnet A
Private Subnet B
```

across multiple AZs.

---

## Interview Question

Why create subnets across multiple Availability Zones?

### Answer

To achieve High Availability and Fault Tolerance.

---

# Scenario 08: Load Balancer Creation Fails

## Problem Statement

Application Load Balancer creation fails.

---

## Investigation

Only one Public Subnet selected.

---

## Root Cause

ALB requires:

```text
Minimum 2 Subnets
Different Availability Zones
```

---

## Fix

Create:

```text
Public Subnet-A
Public Subnet-B
```

and attach both.

---

## Interview Question

How many subnets are required for an Application Load Balancer?

### Answer

At least two subnets in different Availability Zones.

---

# Scenario 09: VPC Peering Traffic Not Working

## Problem Statement

Instances in two VPCs cannot communicate.

---

## Investigation

Peering connection active.

Subnet Route Tables missing routes.

---

## Root Cause

Subnet routing not configured.

---

## Fix

Add routes:

```text
10.1.0.0/16 → pcx-xxxx
10.2.0.0/16 → pcx-xxxx
```

---

# Scenario 10: Production Web Server Accidentally Deployed in Private Subnet

## Problem Statement

Website inaccessible.

---

## Investigation

Web Server located in:

```text
Private Subnet
```

No Load Balancer.

No Public IP.

---

## Root Cause

Incorrect subnet selection during deployment.

---

## Fix

Deploy:

```text
ALB → Public Subnet
Web Server → Private Subnet
```

or

```text
Web Server → Public Subnet
```

for small environments.

---

# 🎤 Frequently Asked Interview Questions

### 1. What is a Subnet?

A subnet is a logical division of a VPC IP address range used to organize resources and control network traffic.

---

### 2. Difference Between Public and Private Subnet?

| Public Subnet             | Private Subnet                      |
| ------------------------- | ----------------------------------- |
| Route to Internet Gateway | No direct route to Internet Gateway |
| Can have Public IP        | Usually no Public IP                |
| Internet Accessible       | Not Internet Accessible             |
| Hosts ALB, Bastion Host   | Hosts App Servers, Databases        |

---

### 3. How Do You Make a Subnet Public?

Add:

```text
0.0.0.0/0 → Internet Gateway
```

and assign Public IPs to instances.

---

### 4. Can a Private Subnet Access Internet?

Yes.

Using:

```text
NAT Gateway
```

or

```text
NAT Instance
```

---

### 5. Why Use Multiple Subnets?

* High Availability
* Fault Tolerance
* Better Security
* Multi-AZ Deployment
* Load Balancer Support

---

### 6. Can a Subnet Span Multiple Availability Zones?

### Answer

No.

A subnet can exist in only one Availability Zone.

---

### 7. What is the CIDR Block of a Subnet?

Example:

```text
VPC: 10.0.0.0/16

Public Subnet: 10.0.1.0/24
Private Subnet: 10.0.2.0/24
```

CIDR defines the IP range available within the subnet.

