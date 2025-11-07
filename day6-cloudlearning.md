# TASK
Hosting a website on S3.

# Hosting a website on S3

- Hosting a static website on Amazon S3 is a quick, scalable, and cost-effective way to publish web content without needing a traditional web server.  
- It’s ideal for documentation, portfolios, and frontend-only applications.  
- While it supports only HTTP access, you can later integrate CloudFront if you want HTTPS and global caching.
 ## Step-by-Step Guide

### **1. Create an S3 Bucket**
1. Open **AWS Management Console → S3**.
2. Click **Create bucket**
3. Enter a **unique bucket name**, for example:  amzn-s3-name-bucket
4. Choose a **Region** (e.g.,ap-south-1 for Mumbai).
5. Under **Block Public Access**, **uncheck** “Block all public access”.
6. Confirm the warning and click **Create bucket**.

 ### **2. Upload Website Files**
7. Open the newly created bucket.
8. Click **Upload** → Add the website files (index.html, styles.css, script.js, etc.).
9. Click **Upload** to complete.

### **3. Enable Static Website Hosting**
1. Go to the **Properties** tab.
2. Scroll down to **Static website hosting** and click **Edit**.
3. Choose **Enable**.
4. Select **Host a static website**.
5. Specify:
   - **Index document:** index.html
6. Click **Save changes**. 
### **4. Make the Bucket Public**

By default, objects in your S3 bucket are private.  
To make your website publicly accessible:

1. Go to the **Permissions** tab.
2. Scroll to **Bucket Policy** → Click **Edit**.
3. Add this policy:

**json**
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::amzn-s3-name-bucket/*"
    }
  ]
}

- **Version**-Defines the policy language version.
- **Statement**-Contains one or more permission statements.One statement block here.
- **Sid**-Optional label for the statement.
- **Effect**-Allow or Deny the action.
- **Principal**-Who the policy applies to.
- **Action**-What action is allowed.
- **Resource**-What resources it applies to.





