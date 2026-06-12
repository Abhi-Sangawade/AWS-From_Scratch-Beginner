# 🔥 EC2 Instance Type Change - Scenario Based Questions

---

## Scenario 1: Unable to Change Instance Type

### Problem

You attempt to change the instance type from:

```text
t2.micro → t2.nano
```

but the option is grayed out.

### Root Cause

The instance is still running.

### Solution

1. Stop the instance
2. Wait until state becomes **Stopped**
3. Change the instance type
4. Start the instance

### Key Learning

EC2 instance type modification requires the instance to be in a stopped state.

---

## Scenario 2: Application Not Accessible After Resize

### Problem

Instance successfully starts after changing the instance type, but the application is unavailable.

### Investigation

* Instance Status Checks: Passed
* Security Groups: Correct
* Application Service: Not running

### Root Cause

Application service was not configured to start automatically.

### Solution

```bash
systemctl status nginx

systemctl start nginx
```

or

```bash
systemctl status httpd

systemctl start httpd
```

### Key Learning

Always verify application services after restarting an EC2 instance.

---

## Scenario 3: Public IP Address Changed

### Problem

After stopping and starting the instance, users cannot access the application.

### Investigation

Old IP:

```text
54.x.x.x
```

New IP:

```text
3.x.x.x
```

### Root Cause

The instance was using an auto-assigned public IP.

### Solution

Allocate and associate an Elastic IP.

### Key Learning

Elastic IP prevents IP address changes during stop/start operations.

---

## Scenario 4: Instance Fails Status Checks After Resize

### Problem

After changing the instance type, the instance remains in:

```text
Status Checks Failed
```

### Investigation

System Logs show boot errors.

### Root Cause

Operating system corruption or application startup issues.

### Solution

1. Review system logs
2. Verify attached volumes
3. Check startup services
4. Restore from snapshot if required

### Key Learning

Always verify health checks after infrastructure modifications.

---

## Scenario 5: Wrong Instance Type Selected

### Problem

Instead of:

```text
t2.nano
```

the engineer selected:

```text
c5.4xlarge
```

### Impact

Unexpected increase in AWS cost.

### Solution

Resize the instance again to the correct type.

### Key Learning

Always validate the selected instance type before applying changes.

---

## Scenario 6: Instance Resize Causes Performance Issues

### Problem

After changing:

```text
t2.micro → t2.nano
```

the application becomes slow.

### Investigation

CloudWatch Metrics:

```text
CPU Utilization: 95%
```

### Root Cause

The smaller instance lacks sufficient resources.

### Solution

Upgrade to:

```text
t2.micro
```

or

```text
t3.micro
```

### Key Learning

Cost optimization should not negatively impact application performance.

---

## Scenario 7: Root Volume Missing After Restart

### Problem

Instance fails to boot after resize.

### Investigation

Root EBS volume was accidentally detached.

### Solution

Reattach the root volume and restart the instance.

### Key Learning

Always verify storage configuration before troubleshooting instance issues.

---

## Scenario 8: EC2 Resize During Production Hours

### Problem

A production application experiences downtime during instance resizing.

### Root Cause

Stopping the instance caused service interruption.

### Solution

Schedule maintenance windows before performing instance modifications.

### Key Learning

Infrastructure changes should be planned to minimize business impact.

---

## Scenario 9: Instance Type Not Available

### Problem

Desired instance type:

```text
t2.nano
```

is unavailable.

### Root Cause

Selected Availability Zone has limited capacity.

### Solution

* Try another Availability Zone
* Use a similar instance family

### Key Learning

Instance availability may vary by region and Availability Zone.

---

## Scenario 10: Cost Optimization Exercise

### Problem

CloudWatch shows:

```text
CPU Usage: 3%
Memory Usage: Low
```

for several weeks.

### Decision

Resize:

```text
t2.micro → t2.nano
```

### Result

* Lower monthly AWS cost
* Same application performance
* Better resource utilization

### Key Learning

CloudWatch metrics should guide infrastructure sizing decisions.

---

# 🧩 Final Takeaway

EC2 instance resizing is a common cloud optimization task. Before changing an instance type, always verify:

* Instance health
* Resource utilization
* Application requirements
* Maintenance windows
* Public IP dependencies
* Storage configuration

A successful resize balances **cost optimization** and **application performance**.

