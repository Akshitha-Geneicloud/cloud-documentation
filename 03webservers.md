# **Nginx, Tomcat, Middleware, EC2 Deployment & IAM Region Restrictions**
# **Overview:**


1.  What is Nginx?
    
2.  What is Apache Tomcat?
    
3.  What is Middleware?
    
4.  Deploying an application on AWS EC2
    
5.  Installing & configuring Nginx on Linux
    
6.  Creating EC2 instance & IAM policies restricted to Mumbai region
    

----------

# **1.What is Nginx?**

### **Definition**

Nginx (pronounced “engine-x”) is a **high-performance web server** that can also act as:

-   **Reverse Proxy** → forwards traffic to backend servers like Tomcat, Node.js, Flask
    
-   **Load Balancer** → distributes traffic across multiple servers
    
-   **Static File Server** → serves HTML, CSS, JS, images, media
    

### **How Nginx Works**

1.  User sends request
    
2.  Nginx receives it on **port 80 (HTTP)** or **443 (HTTPS)**
    
3.  Nginx forwards the request to backend service (Tomcat, Node.js, Flask, Python app, etc.)
    
4.  Backend processes logic
    
5.  Nginx sends the final response back to the client
    

### **Simple Flow**

**User → Nginx → Backend Server → Nginx → User**

----------

# **2.What is Apache Tomcat?**

### **Definition**

Apache Tomcat is an **application server** used to run **Java-based web applications**, such as:

-   JSP (Java Server Pages)
    
-   Servlets
    
-   Spring Boot WAR deployments
    

> Tomcat ≠ Apache HTTP Server  
> Tomcat executes **Java** code, while Apache HTTP Server handles **static content**.

### **Example Flow**

`Client → Nginx (Web Server) → Tomcat (Application Server)
       → Runs Java Code → Response → Nginx → Browser` 

----------

# **3.What is Middleware?**

### **Definition**

Middleware is software that acts as a **bridge** between:

-   Web server (Nginx/IIS)
    
-   Application server (Tomcat/Node.js)
    

### **Functions of Middleware**

-   Manages communication between systems
    
-   Handles authentication & authorization
    
-   Manages logging, sessions, caching
    
-   Facilitates data exchange
    
-   Ensures smooth interaction between web layer and application logic
    

### **Analogy**

Middleware works like a **translator** ensuring different systems communicate smoothly.

----------

# **4.Deploying an Application on AWS EC2**

### **What is EC2?**

EC2 (Elastic Compute Cloud) is a virtual machine in AWS, running:

-   Linux
    
-   Windows
    
-   Any application or service you deploy
    

### **Deployment Flow**

1.  Deploy application on EC2 instance
    
2.  Configure a server (Nginx / Tomcat)
    
3.  Bind your app to EC2’s **Public IP / Public DNS**
    
4.  Users can now access your application over the internet
    

----------

# **5.Installing & Configuring Nginx on EC2 (Linux)**

### **Commands Used**

`sudo apt update -y               # Update  system packages
sudo apt install nginx -y        # Install Nginx
sudo systemctl enable nginx      # Enable Nginx on startup
sudo systemctl start nginx       # Start Nginx service
sudo ufw allow "Nginx HTTP"      # Allow HTTP traffic through firewall
sudo systemctl status nginx      # Check Nginx status` 

### **Stored in a script file**

You stored these commands in a file named:

`nginx-setup.sh` 

and executed it via Git Bash or Linux terminal.

----------

# **6.Creating EC2 Instance & IAM Policy Restricted to Mumbai Region**

### **Step-by-step Process**

----------

## **Step 1: Launch EC2 instance (as Root User)**

-   Go to AWS Console
    
-   Launch an EC2 instance in **Mumbai Region (ap-south-1)**
    
-   Configure networking, key pair, and security groups
    

----------

## **Step 2: Create IAM User**

-   Go to IAM → Users → Create User
    
-   Assign programmatic & console access
    

----------

## **Step 3: Create a Custom IAM Policy (Region Restricted)**

This policy allows EC2 access **only** in **Mumbai Region (ap-south-1)**.

### **Policy JSON**

`{  "Version":  "2012-10-17",  "Statement":  [  {  "Sid":  "Statement1",  "Effect":  "Allow",  "Action":  "ec2:*",  "Resource":  "*",  "Condition":  {  "StringEquals":  {  "ec2:Region":  "ap-south-1"  }  }  }  ]  }` 

----------

## **Step 4: Assign the Policy to IAM User**

-   Attach policy to IAM user
    
-   Login to AWS using IAM user credentials
    

----------

## **Step 5: Test Region Restriction**

-   Try accessing EC2 in other regions (e.g., Ohio, Singapore)
    
-   You will get **Access Denied**
    
-   Only **Mumbai region** EC2 resources will be accessible
    

✔ This confirms that your IAM user is restricted strictly to **ap-south-1 region**.
