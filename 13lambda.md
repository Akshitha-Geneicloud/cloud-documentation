
# **AWS Lambda, Cost Optimization**
## **This document explains**:
1. AWS lambda
2. Cost Optimization in AWS



# **1. What is AWS Lambda? (Simple Explanation)**

### **Definition**

AWS Lambda is a service where you run your code **without servers**.  
You do NOT manage EC2, OS, patches, scaling — AWS handles everything.

###  **Super Simple Meaning**

**“Just upload your code → AWS runs it whenever needed → You pay only for the time your code runs.”**

### **When is Lambda used?**

-   Run small functions automatically (image resizing, log processing)
    
-   Run code when an event happens (S3 upload, API request)
    
-   Backend for small websites
    
-   Automation tasks (turn off EC2, take backups)
----------


# **2. Cost Optimization in AWS (Very Simple Meaning)**

### Definition

Cost Optimization = **reducing AWS bill without reducing performance**.

### Why it's needed?

Because AWS charges for everything you use:

-   Compute (EC2)
    
-   Storage (S3, EBS)
    
-   Databases (RDS)
    
-   Networking (Data transfer)
    

Sometimes we keep resources running even when NOT required → unnecessary cost.

----------

# **3. AWS Services That Need Cost Optimization (Simple Points)**

Here are the **main services** where most companies waste money:

----------

## **A) EC2 Instances**

The biggest cost!

### Why they cost more?

-   Running 24/7
    
-   Large instance type used even if CPU usage is low
    
-   Unused EBS volumes
    
-   Unattached Elastic IP
    
-   Stopped instances still attached to volumes
    

### How to optimize?

-   Use **Auto-Stop** schedules
    
-   Use **Auto Scaling**
    
-   Use **Spot Instances**
    
-   Right-size (use correct instance size)
    
-   Delete unused volumes & Elastic IPs
    

----------

##  **B) EBS (Elastic Block Storage)**

Storage attached to EC2.

### Problems:

-   Unused old volumes
    
-   Too much storage allocated
    
-   Snapshots kept forever
    

### Optimization:

-   Delete unused EBS
    
-   Use Lifecycle to delete old snapshots
    

----------

##  **C) S3**

Storage seems cheap, but large files + long retention → high cost.

### Problems:

-   Keeping logs forever
    
-   Storing unnecessary objects
    
-   Wrong storage class
    

### Optimization:

-   Use **S3 Lifecycle Rules** → move data to Glacier
    
-   Delete old log files
    
-   Use **INTELLIGENT-TIERING**
    

----------

##  **D) RDS Databases**

Very expensive service.

### Problems:

-   Large DB instance for small workload
    
-   Multi-AZ enabled even when not needed
    
-   Backups stored for long time
    

### Optimization:

-   Right-size RDS
    
-   Disable Multi-AZ for dev environments
    
-   Reduce backup retention
    

----------

##  **E) Lambda**

Cheap, but costs increase if:

-   Function runs for long time
    
-   High number of invocations
    

### Optimization:

-   Reduce function duration
    
-   Use smaller memory size
    
-   Cache results
    

----------

##  **F) Load Balancers (ALB/NLB)**

Charges per hour + per request.

### Optimization:

-   Delete unused LBs
    
-   Turn off after working hours for dev/test
    

----------

##  **G) CloudWatch Logs**

Many beginners forget this!

### Problem:

Log data stored for months → very expensive.

### Optimization:

-   Set log retention to 30 days
    
-   Delete unused log groups
