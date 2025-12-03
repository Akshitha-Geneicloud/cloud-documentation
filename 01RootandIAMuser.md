
# **AWS Basics**
# Overview:
- What is Cloud Computing?
- What is Root User?
- IAM
- IAM User
- What are Roles and Policies
## **1. Cloud Computing**

### **Definition**

Cloud computing is the on-demand delivery of computing services—such as servers, storage, databases, networking, software, and analytics—over the internet (“the cloud”) instead of relying on local computers or physical servers.

### **Simple Example**

**Before Cloud:**  
You had to buy physical servers, install operating systems, maintain hardware, and manage everything manually.

**With Cloud:**  
You can rent these computing resources from cloud providers (AWS, Azure, GCP) and use them anytime from anywhere, paying only for what you need.

----------

## **2. Root User**

### **Definition**

The root user is the primary account created when you first sign up for a cloud platform. It has complete and unrestricted access to all resources and configurations.

### **Important Characteristics**

-   Created using your email and password during sign-up.
    
-   Has full administrative access (can delete resources, close account, modify billing, etc.).
    
-   Should NOT be used for everyday tasks.
    
-   Cloud providers recommend using it only once—to create the first IAM admin user.
    

### **Security Best Practices**

-   Enable **MFA (Multi-Factor Authentication)**.
    
-   Do **not share root credentials**.
    
-   Use a strong, unique password.
    
-   Store credentials securely (preferably offline).
    
-   Use IAM users or roles for all regular operations.
    

----------

## **3. IAM (Identity and Access Management)**

IAM is a cloud service that enables secure management of user identities and access to cloud resources. It helps define **who** can access **what**.

----------

## **4. IAM User**

### **Definition**

An IAM user represents a person or application that has been granted specific permissions to access cloud services.

### **Examples of IAM Users**

-   **admin-user** → Full access
    
-   **developer-user** → Access to EC2 and S3
    
-   **billing-user** → Billing access only
    

----------

## **5. Roles**

### **Definition**

A role is a set of permissions that define what actions a user or service can perform.  
Roles ensure that each person or service gets the minimum required access.

### **Examples**

-   **Admin Role:** Full access to all resources.
    
-   **Developer Role:** Can create and modify resources but cannot delete critical ones.
    
-   **Viewer Role:** Can only view resources without making changes.
    

----------

## **6. Policies**

### **Definition**

A policy is a document (set of rules) that defines what actions are **allowed** or **denied** for a user or role. Policies are attached to roles to manage permissions.

### **Example Actions Allowed in a Policy**

-   View storage buckets
    
-   Create virtual machines
    
-   Deny deletion of databases
