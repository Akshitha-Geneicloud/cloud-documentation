# **VPC**
# **Overview:**

1.  Why VPC is needed
    
2.  What is VPC
    
3.  Key Components of VPC
    
4.  Core Components (Detailed Architecture View)
    
5.  CIDR
    
6.  Public & Private Instances
    

----------

# **1. WHY VPC?**

Earlier:

-   Companies like X, Y, and Z maintained their own on-premise data centers.
    
-   These required **high maintenance, cost, hardware, cooling, and manpower**.
    

After moving to AWS:

-   AWS provided data centers in multiple regions (Mumbai, USA, etc.).
    
-   Companies used AWS servers and resources on a **pay-as-you-go** basis.
    
-   Multiple companies shared the **same physical server** but with logical isolation.
    

### **The Problem (2013/14)**

Companies raised concerns:

-   Sharing resources on the same server felt **insecure**.
    
-   Risk of **data exposure or hacking** increased.
    

### **Solution: AWS VPC**

AWS introduced **VPC** to give each company:

-   Its **own private isolated network**,
    
-   Dedicated environment inside the AWS cloud,
    
-   Full network control (like having a private data center).
    

----------

# **2. WHAT IS VPC?**

### **Definition**

A **VPC (Virtual Private Cloud)** is your private, isolated virtual network inside AWS.

### **Key Points**

-   DevOps engineers request AWS to create a VPC in a specific region.
    
-   Acts like a **mini data center** inside AWS.
    
-   You control:
    
    -   Network structure
        
    -   Communication between systems
        
    -   Internet exposure
        
-   Completely isolated from other users’ networks.
    

### **Simple Explanation**

When you create a VPC, AWS gives you a **private LAN** inside the cloud.

----------

# **3. Key Components of VPC**

## **Subnets**

-   Divide the VPC into smaller units (like rooms in a house).
    
-   **Public Subnet** → Connected to internet through IGW.
    
-   **Private Subnet** → No direct internet access (used for backend).
    

----------

## **Internet Gateway (IGW)**

-   Allows the VPC to connect to the internet.
    
-   Required for public-facing applications.
    

----------

## **Route Tables**

-   Define where traffic moves.
    
-   Every subnet must have one route table.
    
-   Examples:
    
    -   Traffic to internet → goes to IGW
        
    -   Traffic to private subnet → stays internal
        

----------

## **NAT Gateway**

-   Allows **private instances** to access the internet (for updates, API calls).
    
-   But prevents inbound internet traffic.
    
-   Placed in a **public subnet**.
    

----------

## **Security Groups**

-   Instance-level firewalls (stateful).
    
-   Control inbound and outbound traffic.
    

----------

## **Network ACLs (NACLs)**

-   Subnet-level firewalls (stateless).
    
-   Used for broader network rules.
    

----------

# **4. Core Components of Amazon VPC (Architecture View)**

## **VPC**

-   A logically isolated section of AWS cloud.
    
-   Defined by an **IPv4 CIDR block** (e.g., 10.0.0.0/16).
    
-   Can have up to **65,536 IP addresses**.
    
-   Works like an on-prem data center but scalable and resilient.
    

----------

## **Subnets**

Located inside **one Availability Zone (AZ)**.  
Benefits:

-   **Segmentation** of workloads
    
-   **High availability** across AZs
    

Types:

-   Public Subnet → Routes to IGW
    
-   Private Subnet → Uses NAT for outbound traffic
    

----------

## **Route Tables**

Each route defines:

-   **Destination CIDR** (where traffic goes)
    
-   **Target** (IGW, NAT, instance, etc.)
    

Acts as the **roadmap** of the VPC.

----------

## **Network ACLs (NACLs)**

-   Stateless firewall at subnet level.
    
-   Requires explicit allow/deny rules.
    
-   Used for controlling entire subnet traffic patterns.
    

----------

## **Internet Gateway (IGW)**

-   Enables internet communication.
    
-   Only **one IGW per VPC**.
    
-   Public subnets need a route to IGW (0.0.0.0/0).
    

----------

## **NAT Gateway**

-   Used for private subnet outbound access.
    
-   Prevents inbound external traffic.
    
-   Deployed in Public Subnet.
    

----------

## **Security Groups**

-   Stateful firewall at instance level.
    
-   If outbound is allowed, return traffic is automatically allowed.
    
-   Protects EC2, RDS, and other resources.
    

----------

## **CIDR (Classless Inter-Domain Routing)**

Defines IP ranges like:

-   **10.0.0.0/16**
    
-   **172.16.0.0/12**
    
-   **192.168.0.0/16**
    

These private IP ranges ensure:

-   No conflicts
    
-   Full network isolation
    

----------

# **5. Public Instance vs Private Instance**

## **What is an Instance?**

An **EC2 instance** is simply a virtual machine.

----------

## **Public Instance**

An instance that is accessible from the internet.

### **Key Features**

-   Located in **Public Subnet**
    
-   Subnet routes to the **Internet Gateway (IGW)**
    
-   Has a **Public IP** / Elastic IP
    
-   Used for **web servers, APIs, etc.**
    

### **Example**

Public IP: **13.234.56.12**

----------

## **Private Instance**

Not accessible directly from the internet.

### **Key Features**

-   Launched in **Private Subnet**
    
-   No route to IGW
    
-   Only private IP
    
-   Used for **databases, backend layers, internal services**
    

### **Example**

Private IP: **10.0.1.5**
