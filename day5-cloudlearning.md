# TASK
About Amazon S3.
What is S3  in AWS.
What are the types of S3 Storage Class.
# What is s3?


Amazon S3 is a "Simple Storage Service" in *AWS* that stores files of different types like Photos, Audio, and Videos as Objects providing more scalability and security . It allows the users to store and retrieve any amount of data at any point of time from anywhere on the web. It facilitates features such as high availability, security, and simple connection to other AWS Services.

## Types of S3

- Amazon S3 provides several **storage classes**, each designed to meet different data storage needs in terms of cost, performance, and access frequency.
  -   ****Standard:**** Suitable for frequently accessed data, that needs to be highly available and durable.
  -    ****Standard Infrequent Access (Standard IA):**** This is a cheaper data-storage class and as the name suggests, this class is best suited for storing infrequently accessed data like log files or data archives. 
  -    ****Intelligent Tiering:**** This service class classifies your files automatically into frequently accessed and infrequently accessed and stores the infrequently accessed data in infrequent access storage to save costs. This is useful for unpredictable data access to an S3 bucket.
  -    ****One Zone Infrequent Access (One Zone IA):**** All the files on your S3 have their copies stored in a minimum of 3 Availability Zones. One Zone IA stores this data in a single availability zone. It is only recommended to use this storage class for infrequently accessed, non-essential data.
  -    ****Reduced Redundancy Storage (RRS):**** All the other S3 classes ensure the durability of 99.999999999%. RRS only ensures 99.99% durability. AWS no longer recommends RRS due to its less durability. However, it can be used to store non-essential data.

## S3 Bucket in AWS

Amazon S3 bucket is a fundamental Storage Container feature in AWS S3 Service. It provides a secure and scalable repository for storing of Objects such as Text data, Images, Audio and Video files over AWS Cloud. Each S3 bucket name should be named globally unique and should be configured with ACL (Access Control List).

## Why S3?

Amazon **S3 (Simple Storage Service)** is widely used because it provides a secure, scalable, and cost-effective way to store and manage data on the cloud. It eliminates the need for maintaining physical storage servers and allows users to access their data from anywhere, at any time, through the internet.Its **pay-as-you-go pricing** model ensures you only pay for what you use.

## Key Features of S3
 - **Data Storage**:It helps in storing and retrieving the data-intensitive applications as per needs in ideal time.
 - **Backup and Recovery**:Helps in recovery of critical data and maintain the data durability and availability for recovery needs.
 - **Hosting Static Websites**:Used for storing HTML, CSS and other web content from Users/developers allowing them for hosting Static Websites.
 - **Data Archiving**:Helps as a cost-effective solution for long-term data storing which are less frequently accessed applications.
 - **Access Control** – Manage permissions via IAM, bucket policies, and ACLs.



