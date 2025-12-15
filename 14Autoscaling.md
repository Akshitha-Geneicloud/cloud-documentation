
# **AWS Auto scaling and load balancing**
## **This document explains**:
1. AWS Auto scaling
2. Load balancing




#  **AUTO SCALING (AWS AUTO SCALING / EC2 AUTO SCALING)**

###  **What is Auto Scaling?**

Auto Scaling automatically **adds or removes EC2 instances** based on the load (traffic, CPU, memory, etc.) so our application always has the right amount of capacity.

----------

#  **Why do we use Auto Scaling?**

-   To handle traffic spikes automatically
    
-   To reduce cost by shutting down extra instances when not needed
    
-   To increase reliability and avoid system overload
    

----------

#  **Key Components of Auto Scaling**

### **1. Launch Template**

-   Defines **AMI, instance type, security group, key pair, user data, etc.**
    
-   Blueprint for EC2 instances Auto Scaling launches.
    

### **2. Auto Scaling Group (ASG)**

-   A group of EC2 instances controlled by Auto Scaling.
    
-   You define:
    
    -   **Minimum instances**
        
    -   **Maximum instances**
        
    -   **Desired capacity**
        

### **3. Scaling Policies**

Auto Scaling uses policies to decide **when to scale**:

#### **a) Target Tracking**

-   Easiest method.
    
-   Example: Keep CPU at 40%.
    
-   Auto Scaling adds/removes instances to maintain that target.
    

#### **b) Step Scaling**

-   Adds/removes instances in steps.
    
-   Example:
    
    -   CPU > 70% → add 2 instances
        
    -   CPU > 90% → add 3 instances
        

#### **c) Scheduled Scaling**

-   Scale based on time.
    
-   Example: Add instances at 9 AM, remove at 9 PM.
    

----------

#  **How Auto Scaling Works**

1.  You define a Launch Template
    
2.  You create Auto Scaling Group
    
3.  You attach Scaling Policies
    
4.  CloudWatch monitors system metrics
    
5.  If threshold is crossed, ASG launches or terminates EC2 instances
    

----------

#  **Benefits of Auto Scaling**

-   Handles increased traffic automatically
    
-   Reduces cost
    
-   Improves performance and availability
    
-   Helps applications remain reliable
    

----------

#  **Example**

An e-commerce app has high traffic in the evening.

Auto Scaling:

-   Increases EC2 instances when traffic increases
    
-   Reduces EC2 instances at midnight to save cost
    

----------

----------

#  **AWS LOAD BALANCING (ELB – Elastic Load Balancing)**

###  **What is Load Balancing?**

Load balancing distributes incoming traffic across **multiple servers (EC2 instances)** so no single server gets overloaded.

----------

#  **Why do we use Load Balancers?**

-   To prevent a single instance from failing
    
-   For high availability
    
-   For better performance
    
-   To route traffic intelligently
    

----------

#  **Types of Load Balancers in AWS**


## **1. Application Load Balancer (ALB)**

-  Works at Layer 7 (HTTP/HTTPS)

### **Example:**

Lets consider a website like **amazon.com**.

-   **/login** should go to Login servers
    
-   **/orders** should go to Order servers
    
-   **/products** should go to Product servers
    

ALB can route based on **URL path**, **hostname**, **headers**, etc.  
Perfect for **web apps, microservices, APIs**.

----------

## **2. Network Load Balancer (NLB)**

-  Works at Layer 4 (TCP/UDP)

### **Example:**

A stock trading app where speed is extremely important.

-   Millions of users connecting every second
    
-   Requires very low latency
    
-   Uses TCP/UDP protocols
    

NLB is used for **high-speed, real-time applications**.

----------

## **3. Gateway Load Balancer (GWLB)**

 - Used for Security Appliances

### **Example:**

You want all incoming traffic to pass through a **firewall** or **intrusion detection system (IDS)**.

GWLB automatically sends traffic to:

-   Firewall
    
-   IDS/IPS
    
-   Security appliances
    

Used in **security-heavy architectures**.

----------

## **4. Classic Load Balancer (CLB)** – _Older version_

- Works at basic HTTP and TCP

### **Example:**

A simple **two-server** web application built years ago:

-   No advanced routing
    
-   Just distribute traffic between EC2 instances
    

CLB is **older** and not recommended for new setups.

----------

#  **Simple Summary**

-   **ALB →** Smart load balancer for websites/APIs
    
-   **NLB →** Super-fast load balancer for TCP/UDP apps
    
-   **GWLB →** Load balancer for firewalls/security tools
    
-   **CLB →** Old basic load balancer
----------

#  **Load Balancer Features**

-   Health checks (send traffic only to healthy instances)
    
-   Sticky sessions (same user returns to same server)
    
-   SSL termination
    
-   Scaling automatically with traffic
    
-   Integration with Auto Scaling Groups
    

----------

#  **Auto Scaling + Load Balancer**

### How they work together:

1.  Load Balancer distributes incoming traffic
    
2.  Auto Scaling adds/removes instances
    
3.  Load Balancer automatically registers new instances
    
4.  Users never notice scaling happening behind the scenes
    

----------

#  **Simple Architecture Picture**

**Users → Load Balancer → Auto Scaling Group → EC2 Instances** 

-   If traffic increases → ASG adds instances → Load Balancer sends traffic to them
    
-   If traffic decreases → ASG removes instances → Load Balancer stops sending traffic to them


