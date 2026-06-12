# 🎤 Scenario-Based Interview Questions – EC2 Instance Type Change

### 1. You notice an EC2 instance has 5% average CPU utilization for the last 30 days. What would you do?

**Answer:**

Review CloudWatch metrics and right-size the instance. If the workload is consistently low, downgrade to a smaller instance type to reduce AWS costs.

---

### 2. A manager asks you to change an EC2 instance from t2.micro to t2.nano. What precautions would you take?

**Answer:**

* Verify CPU and memory utilization
* Check instance health status
* Confirm application requirements
* Schedule a maintenance window
* Take a backup or snapshot
* Inform stakeholders about potential downtime

---

### 3. After changing the instance type, the application becomes slow. What would you do?

**Answer:**

* Check CloudWatch metrics
* Verify CPU and memory utilization
* Compare workload requirements with available resources
* Upgrade to a larger instance type if required

---

### 4. Why must an EC2 instance be stopped before changing the instance type?

**Answer:**

AWS needs to reallocate the underlying virtual hardware resources. Therefore, most instance type changes require the instance to be in a stopped state.

---

### 5. After stopping and starting the instance, users cannot access the application. What could be the reason?

**Answer:**

Possible reasons:

* Public IP changed
* Security Group issue
* Application service not running
* Route table or networking issue
* Health checks failing

---

### 6. The application is working but users report the URL is inaccessible after the resize. What would you check first?

**Answer:**

Check whether the instance was using an auto-assigned public IP. If so, the public IP may have changed after restart.

---

### 7. How can you prevent IP changes during instance stop/start operations?

**Answer:**

Associate an Elastic IP with the EC2 instance.

---

### 8. What AWS service would you use to determine whether an EC2 instance is over-provisioned?

**Answer:**

Amazon CloudWatch.

Review:

* CPU Utilization
* Network Traffic
* Disk Operations
* Memory Metrics (if enabled)

---

### 9. Your production EC2 instance runs a critical application. Would you directly resize it?

**Answer:**

No.

First:

* Take an EBS snapshot
* Verify backups
* Schedule maintenance
* Test in a non-production environment if possible

---

### 10. What is the business benefit of EC2 right-sizing?

**Answer:**

* Reduced AWS costs
* Better resource utilization
* Improved cloud efficiency
* Elimination of over-provisioning

---

### 11. What happens to attached EBS volumes when the instance type changes?

**Answer:**

Nothing.

EBS volumes remain attached and data is preserved during the instance type modification process.

---

### 12. What would you do if the desired instance type is unavailable in the current Availability Zone?

**Answer:**

* Try another compatible instance family
* Move the workload to another Availability Zone
* Launch a new instance with the desired type

---

### 13. How do you verify that the resize was successful?

**Answer:**

Check:

```bash
aws ec2 describe-instances \
  --instance-ids <instance-id>
```

or verify from the AWS Console:

* Instance Type
* Running State
* Status Checks
* Application Accessibility

---

### 14. What is the difference between vertical scaling and horizontal scaling?

**Answer:**

**Vertical Scaling:**

```text
t2.nano → t2.micro → t2.small
```

Increase CPU/RAM on the same instance.

**Horizontal Scaling:**

```text
1 EC2 → 2 EC2 → 4 EC2
```

Add more instances behind a Load Balancer.

---

### 15. In a real production environment, when would you resize an EC2 instance?

**Answer:**

* Cost optimization initiatives
* High CPU utilization
* Memory shortages
* Seasonal traffic changes
* Infrastructure modernization projects

