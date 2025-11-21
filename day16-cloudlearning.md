
# **TASK:Automatically Stopping EC2 Instances Using Lambda and EventBridge**

## **1. Introduction**

This guide explains how to automate stopping specific EC2 instances at a scheduled time daily using:

-   **AWS Lambda**
    
-   **Amazon EventBridge (Scheduler)**
    
-   **IAM Role with EC2 permissions**
    

The automation helps reduce cost by shutting down non-production instances during off-hours.

----------

## **2. Components**

-   **AWS Lambda:** Executes code serverlessly to stop EC2 instances.
    
-   **Amazon EventBridge:** Triggers Lambda functions based on a schedule.
    
-   **IAM Role:** Grants permissions for Lambda to perform EC2 actions.
    

----------

## **3. IAM Role and Policy**

### **3.1 Execution Role for Lambda**

Lambda needs an execution role allowing it to stop EC2 instances.

### **3.2 Minimal IAM Policy**

Attach this policy to the Lambda execution role:
```{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:StopInstances",
        "ec2:DescribeInstances"
      ],
      "Resource": "*"
    }
  ]
}
```

**Trust Relationship:**  
Allow Lambda service to assume the role:
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "lambda.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```
----------

## **4. Lambda Function**

The Lambda function uses the **boto3** library (Python SDK) to stop instances.
# AWS Lambda Function: Stop EC2 Instances

```python
import boto3

def lambda_handler(event, context):
    # Initialize EC2 client in Mumbai region
    ec2 = boto3.client('ec2', region_name='ap-south-1')

    # List of EC2 instances to stop
    instances_to_stop = [
        "instance-1",
        "instance-2",
        "instance-n"
    ]

    # to stop the instances
    response = ec2.stop_instances(InstanceIds=instances_to_stop)

    # Return response for logging
    return {
        'statusCode': 200,
        'body': f'Instances stopping: {instances_to_stop}',
        'response': response
    }
```
    

----------

## **5. EventBridge Scheduling**

-   Used **cron expression** in UTC to define schedule.
    
-   Example: To stop instances at **9:00 PM IST** → UTC = 3:30 PM → `cron(30 15 * * ? *)`
    
-   EventBridge triggers the Lambda function at scheduled times automatically.
    

**Target:** Lambda function created above.  
**Permissions:** Default EventBridge permissions are sufficient; the Lambda role handles EC2 permissions.

----------

## **6. Workflow Summary**

1.  Create **IAM role** with EC2 permissions.
    
2.  Develop and deploy **Lambda function**.
    
3.  Configure **EventBridge schedule** with the cron expression.
    
4.  Test Lambda manually.
    
5.  Monitor scheduled execution via **CloudWatch Logs**.
