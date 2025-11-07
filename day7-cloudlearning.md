# TASK
Key Components in S3.
About cloud front in S3.



# Key components in S3

- **S3 Versioning**: Versioning means always keeping a record of previously uploaded files in S3. Points to Versioning are not enabled by default. Once enabled, it is enabled for all objects in a bucket. Versioning keeps all the copies of your file, so, it adds cost for storing multiple copies of your data.
- **Lifecycle Management**:Lifecycle Management, which helps automate how data is stored and deleted over time. You can define lifecycle rules to automatically transition objects between storage classes or permanently delete them after a specific duration. For example, you could create a rule that moves data to the Glacier storage class after 30 days and deletes it completely after a year.
- **Access control lists (ACLs)**: A document for verifying access to S3 buckets from outside your AWS account. An ACL is specific to each bucket. You can utilize S3 Object Ownership, an Amazon S3 bucket-level feature, to manage who owns the objects you upload to your bucket and to enable or disable ACLs.
- **Replication**:Ensures data availability and redundancy. Replication allows you to automatically copy objects from one bucket to another, either within the same AWS region or across different regions.
	- The two main types are **Same-Region Replication (SRR)** and **Cross-Region Replication (CRR)**. 
	- Same-Region Replication is typically used for compliance, logging, or backup within the same region, while Cross-Region Replication is used to create geographically distributed backups for disaster recovery or to reduce latency for users in different parts of the world.
- **Amazon Resource Name (ARN)**:The ARN is a globally unique identifier for AWS resources and plays a crucial role in permissions and policies.
- **S3 Object URL**:Each object in S3 has a unique address that can be accessed through what’s known as the S3 Object URL. This is a web-based link that follows a standard pattern.
- **Host static websites**:Amazon S3 also includes the capability to **host static websites** directly from a bucket. This is a very popular use case, especially for hosting portfolios, documentation, or static front-end applications built with HTML, CSS, and JavaScript.
- **Encryption**:Encryption, which protects data both in transit and at rest. Data in transit is automatically protected using HTTPS (SSL/TLS), while data at rest can be encrypted in several ways.
	- The simplest method is **Server-Side Encryption with S3-Managed Keys (SSE-S3)**, where AWS automatically encrypts and decrypts your data using managed keys.
	- A more advanced option is **Server-Side Encryption with AWS KMS Keys (SSE-KMS)**, which integrates with AWS Key Management Service and gives you more control over key management.
- **Event notifications**:S3 also supports **event notifications**, allowing it to integrate with other AWS services. You can configure S3 to send notifications to Amazon SNS, SQS, or trigger AWS Lambda functions when certain actions occur, such as object creation or deletion.
- **S3 Object Lock**:S3 Object Lock, which prevents objects from being deleted or modified for a defined period. 
- **Object tagging**:Allows you to assign key-value pairs to objects for better organization, billing, or lifecycle management.
	- For example, you might tag objects with “project=AI” or “team=marketing” and then use these tags in cost allocation reports. 

 ## CloudFront in S3


- Amazon CloudFront is a **Content Delivery Network (CDN)** service provided by AWS that Speeds up the delivery of your content such as web pages, images, videos, APIs, or even software downloads to users across the globe with low latency and high transfer speeds.
- For example, if my S3 bucket is in the Mumbai (ap-south-1) region, users in the United States or Europe may experience slower load times due to the physical distance between them and the data center. This is where CloudFront comes into play.

**1. What CloudFront Is:**  
Amazon CloudFront is a Content Delivery Network (CDN) that speeds up the delivery of web content, videos, and data to users worldwide with low latency.

**2. Why It’s Needed:**  
It brings content closer to users through global edge locations, reducing delay that occurs when users are far from the main AWS region.

**3. How It Works (Basics):**  
CloudFront caches copies of your data in nearby edge locations. When a user requests content, it’s served from the nearest cache, improving speed and reliability.
-  An Edge Location is a data center located in various cities around the world that is part of Amazon CloudFront’s global content delivery network (CDN).

**4. Origin Server:**  
The origin is the main source of your content — usually an S3 bucket, EC2 instance, or external web server — from which CloudFront retrieves data.

**5. Cache Hit and Miss:**  
If content is already in the edge location (cache hit), it’s served instantly. If not (cache miss), CloudFront fetches it from the origin and stores it for next time.

**6. Global Edge Network:**  
CloudFront uses a vast global network of edge locations and regional caches to ensure fast delivery even during high traffic.

**7. Security Features:**  
It integrates with AWS WAF and Shield for DDoS protection, supports HTTPS for secure data transfer, and allows signed URLs/cookies for restricted access.

**8. Origin Access Identity (OAI):**  
OAI lets CloudFront access private S3 buckets securely, ensuring users can’t bypass CloudFront to reach the bucket directly.

**9. Content Invalidation:**  
Invalidation removes outdated cached files across all edge locations so users always get the latest version of your content.

**10. Static and Dynamic Content:**  
CloudFront can deliver both cached static files (like images) and dynamic data (like API responses) efficiently using intelligent routing.

**11. Cost Efficiency:**  
By serving cached data from edge locations, CloudFront reduces load and data transfer costs from the origin.

**12. Monitoring:**  
It provides logs and metrics via CloudWatch and access logs, allowing you to analyze performance, cache hits, and user behavior.

**13. Summary:**  
CloudFront improves website performance, security, and scalability by caching and delivering data from global edge locations close to users.
![alt text](image.png)


 





