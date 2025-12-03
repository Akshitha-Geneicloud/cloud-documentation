# **HandsOn:Access Private EC2 from Public EC2 through SSH**
# **Overview:**



### Task Description

We must create **two Linux EC2 machines**:

-   **Public EC2 instance** → Has a public IP (reachable from your laptop)
    
-   **Private EC2 instance** → Has NO public IP (reachable only inside VPC)
    

The goal:

### **SSH from our laptop → Public EC2 → Private EC2**

This setup is called a **Bastion Host / Jump Host**.

----------

## **Why do we do this?**

Think of it like a house:

-   **Public machine = Gatehouse**
    
-   **Private machine = Inner secure room**
    

Benefits:

-   ✔ **More security** (private machine not exposed to internet)
    
-   ✔ **Controlled access** only from bastion host
    
-   ✔ **Industry-standard practice** for databases & internal servers
    

----------

## **High-Level Flow**

1.  Launch **Public EC2** in **public subnet**
    
2.  Launch **Private EC2** in **private subnet**
    
3.  Public SG: allow SSH from our laptop
    
4.  Private SG: allow SSH _only from public EC2_
    
5.  SSH from laptop → Public EC2
    
6.  SSH from Public EC2 → Private EC2
    

----------

# **STEP-BY-STEP IMPLEMENTATION**

----------

## **Step 1 — Create a VPC**

1.  Go to **AWS Console → VPC → Create VPC**
    
2.  Select:
    
    -   _Resources to create:_ **VPC Only**
        
    -   _Name:_ `my-vpc`
        
    -   _IPv4 CIDR:_ `10.0.0.0/16`
        

✔ VPC created.

----------

## **Step 2 — Create Two Subnets**

Go to **Subnets → Create subnet**

Create **Public Subnet**

-   Name: `public-subnet`
    
-   AZ: `ap-south-1a`
    
-   CIDR: `10.0.1.0/24`
    

Create **Private Subnet**

-   Name: `private-subnet`
    
-   AZ: `ap-south-1a`
    
-   CIDR: `10.0.2.0/24`
    

✔ Now you have public + private subnets.

----------

## **Step 3 — Create & Attach an Internet Gateway**

1.  Go to **Internet Gateways → Create IGW**
    
    -   Name: `my-igw`
        
2.  Select IGW → **Attach to VPC**  
    Choose: `my-vpc`
    

✔ Public subnet can now reach the internet.

----------

## **Step 4 — Configure Route Table**

1.  Go to **Route Tables**
    
2.  Rename existing one → `public-route-table`
    
3.  Select it → Routes → Edit
    
4.  Add:
    
    -   Destination: `0.0.0.0/0`
        
    -   Target: `my-igw`
        
5.  Save
    
6.  Go to **Subnet Associations**
    
    -   Select `public-subnet`
        
    -   Save
        

✔ Only public subnet has internet access.

----------

## **Step 5 — Create Security Groups**

### **1. public-sg**

-   Name: `public-sg`
    
-   Inbound rule:
    
    -   SSH (22)
        
    -   Source: **My IP**
        
-   Outbound: allow all (default)
    

### **2. private-sg**

-   Name: `private-sg`
    
-   Inbound rule:
    
    -   SSH (22)
        
    -   Source: **public-sg** (from dropdown)
        
-   Outbound: allow all
    

✔ Private instance is only accessible from public instance.

----------

## **Step 6 — Launch EC2 Instances**

### **Public EC2**

-   Name: `public-ec2`
    
-   AMI: Amazon Linux 2 / Ubuntu
    
-   Type: t2.micro / t3.micro
    
-   Key pair: choose or create
    
-   Network:
    
    -   VPC: `my-vpc`
        
    -   Subnet: `public-subnet`
        
    -   Auto-assign public IP: **Enable**
        
    -   Security Group: `public-sg`
        

### **Private EC2**

-   Name: `private-ec2`
    
-   AMI: same as public
    
-   Subnet: `private-subnet`
    
-   No public IP (default)
    
-   Security Group: `private-sg`
    

✔ Instances ready.

----------

# **Connecting to Your Instances**

----------

## **Step 8 — SSH into Public EC2 from Laptop**

In terminal:

`cd downloads chmod 400 mykey.pem
ssh -i mykey.pem ubuntu@<PUBLIC_IP>` 

Now you are inside **public-ec2**.

----------

## **Step 9 — SSH from Public → Private Instance**

### ✔ First, **copy your key** to the public EC2 instance

Run this **from your laptop** (not inside EC2):

`scp -i mykey.pem mykey.pem ubuntu@<PUBLIC_IP>:/home/ubuntu/` 

Then SSH again into public EC2:

`ssh -i mykey.pem ubuntu@<PUBLIC_IP>` 

Inside public EC2, run:

`chmod 400 mykey.pem
ssh -i mykey.pem ubuntu@<PRIVATE_IP>` 

✔ You are now inside **private-ec2**.
