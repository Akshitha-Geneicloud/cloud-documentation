# TASK
Latency.
How to identify latency in AWS.
Scenario Based on latency.
SQS,SNS.



# Latency

-	Latency means delay — the time it takes for data to travel from one point to another.

-	In AWS, latency refers to the time delay between when a user sends a 	request (like opening a webpage or accessing an API) and when AWS services respond to it.

- **Example:**  
If a user in India accesses an AWS server hosted in the US, the request takes more time to travel across the globe causing high latency.  
But if the server is in Mumbai (closer region), the response is faster which ensures low latency.

 ## Types of Latency 

 ### 1. **Client-Side Latency**

This happens before the request even reaches AWS.

-   Slow internet connection
    
-   High geographic distance between user and AWS region
    
-   DNS resolution delays
    

 _Example:_ A user in Singapore accessing an EC2 instance in North Virginia may experience high latency just due to distance.
### 2. **Network Latency**

-   Data travels over the internet through routers and ISPs.
    
-   Every “hop” increases delay.
    
-   Poor routing or congestion can also cause spikes.
    
 _AWS reduces this using:_

-   **AWS Global Accelerator**:optimizes routing through AWS backbone network
    
-   **Amazon CloudFront**:caches data closer to users at edge locations

### 3. **Service/Compute Latency**

-   The delay inside AWS when the request reaches compute services like EC2, Lambda, or ECS.
    
-   Reasons:
    
    -   Heavy CPU or memory load
        
    -   Cold start in AWS Lambda
        
    -   Network I/O bottlenecks

_Example:_ Lambda cold starts can cause 100–300 ms delay before execution begins.
### 4. **Storage Latency**

-   Delay when reading or writing data to:
    
    -   Amazon S3
        
    -   Amazon RDS
        
    -   Amazon DynamoDB
        
    -   Amazon EBS
        
   _Example:_ **RDS-** Latency increases with complex queries or insufficient indexing.
### 5. **Application Latency**

-   Caused by inefficient code, database locking, or long-running API calls.
    
-   Even if AWS services are fast, bad code logic adds delays.

 _Example:_ Profile the application using AWS X-Ray or CloudWatch ServiceLens.
## How to Identify Latency in AWS:
 
1.  **CloudWatch Logs:**
    
   -  Amazon CloudWatch Logs collect and store log data from your AWS resources, applications, and services — like EC2, Lambda, RDS, or API Gateway.

	- They help you monitor, search, and analyze what’s happening inside your system in real time.
        
2.  **ELB (Load Balancer) Access Logs:**
    
   -   These logs record every request passing through the load balancer.
        
    -   They include request time and backend processing time, helping us find where delays occur.
        
3.  **VPC Flow Logs**
    
   -   VPC Flow Logs capture information about the network traffic going to and from your AWS resources (like EC2 instances, subnets, or the whole VPC).

	- They don’t record actual data — just metadata like source, destination, ports, and time taken.        
	-    See how long packets take to travel between resources.
    
-   Check for dropped packets or failed connections (which increase latency).
    
-   Identify slow network paths or high traffic load.
4.  **Application Logs:**
    
- Application logging means recording important events or actions that happen inside our application — like when a user sends a request, when your code starts processing it, and when it finishes.

- It helps us find where delays (latency) are happening in our app.
## Scenario Based Example:
- I am a user who is trying to access a application say "amazon.com".
- I experience delay(latency) in accessing the application.
	- **What Could be Reason for the Latency:**
		- 1.  **User Side:** Slow internet or long distance from server 
    
		- 2.  **Network Side:** Data takes longer routes 
    
		- 3.  **Application Server:** Server busy or slow code
    
		- 4.  **Database:** Slow queries or too much load 
    
		- 5.  **DNS / Load Balancer:** Delay in routing 
	- **What are the solutions:**
		- 1.  **User Side:** Improve your internet connection or connect to a nearby AWS region.
    
		- 2.  **Network Side:** Use Amazon CloudFront or AWS Global Accelerator to speed up data delivery.
    
		- 3.  **Application Server:** Enable Auto Scaling and optimize your app code for faster responses.
    
		- 4.  **Database:** Use ElastiCache for caching and optimize or index your database queries.
    
		- 5.  **DNS / Load Balancer:** Use Route 53 latency-based routing and monitor ELB logs to fix delays.

# **SQS and SNS:**
### **Amazon SQS (Simple Queue Service)**

-   It is a message queue service that stores messages temporarily between different parts of an application.
    
-   It helps decouple systems meaning one part can send messages without waiting for the other part to process them.
- This is a one-to-one process.
    
-   Example: When a user places an order, the app sends the order details to an SQS queue, and another service picks it up later to process.
    
 **Used for:** Handling background tasks, buffering, and reducing app load.

----------

###  **Amazon SNS (Simple Notification Service)**

-   It is a messaging and notification service that sends messages to multiple subscribers at once.
    
-   It uses a **“publish/subscribe”** model where one service publishes a message, and all subscribers (like email, SMS, or Lambda) get it instantly.
- This is a one-to-many process.
    
-   Example: When an order is confirmed, SNS sends a notification to the user (email) and updates another service.
    
**Used for:** Sending alerts, notifications, or broadcasting messages to many receivers

