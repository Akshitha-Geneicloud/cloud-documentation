
## 🚀 Automating EC2 Start & Stop Using AWS Lambda (A Practical Cloud Automation)

As part of my hands-on learning in **AWS Cloud and DevOps**, I worked on a simple yet impactful automation task:  
**automatically stopping EC2 instances at night and starting them again in the morning**.

This small automation helps reduce **unnecessary cloud costs** and introduces core AWS concepts like **Lambda, IAM, and EventBridge**.

----------

## 🔍 Problem Statement

Manually starting and stopping EC2 instances every day is:

-   Time-consuming
    
-   Error-prone
    
-   Inefficient for cost management
    

The goal was to **fully automate this process** without manual intervention.

----------

## 🛠️ Solution Overview

I implemented the solution using the following AWS services:

-   **AWS Lambda** – to execute start and stop actions
    
-   **IAM Roles & Policies** – to grant secure, least-privilege access
    
-   **Amazon EventBridge** – to trigger Lambda functions on a schedule
    

To keep things simple and secure, I used **two separate Lambda functions**:

-   One Lambda to **stop EC2 instances**
    
-   One Lambda to **start EC2 instances**
    

----------

## ⚙️ High-Level Workflow

1.  An **EventBridge rule** runs at a scheduled time (cron-based).
    
2.  The rule triggers the respective **Lambda function**.
    
3.  Lambda uses **Boto3 (AWS SDK for Python)** to interact with EC2.
    
4.  EC2 instances are **started or stopped automatically**.
    
5.  Execution logs are stored in **CloudWatch Logs** for monitoring.
    

----------

## 🔐 Security Approach (IAM)

Each Lambda function uses a **dedicated IAM role** with **minimal permissions**:

-   Start Lambda → `ec2:StartInstances`
    
-   Stop Lambda → `ec2:StopInstances`
    
-   Common read permission → `ec2:DescribeInstances`
    

This follows the **principle of least privilege**, which is a best practice in AWS security.

----------

## 🧠 Key Learnings

-   How AWS Lambda enables **serverless automation**
    
-   Importance of **IAM roles and trust policies**
    
-   Understanding **cron schedules in UTC**
    
-   Debugging scheduled executions using **CloudWatch logs**
    
-   Writing **clean and reusable Boto3-based Lambda functions**
    

----------

## 💡 Why This Matters

This automation:

-   Saves cloud costs 💰
    
-   Removes repetitive manual work
    
-   Scales easily to multiple instances
    
-   Demonstrates real-world DevOps thinking
    

It’s a great example of how **small automations can make a big impact** in cloud environments.

----------

## 📌 Conclusion

This project strengthened my understanding of **AWS serverless architecture and automation workflows**.  
It also reinforced how cloud-native services can be combined to build **efficient, secure, and cost-optimized solutions**.
