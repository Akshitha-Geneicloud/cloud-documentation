1. **Cloud Computing**<br>
-Basic Definition:<br>
Cloud computing is the on-demand delivery of computing services — like servers, storage, databases, networking, software, analytics, and more — over the internet (“the cloud”) instead of using your local computer or physical servers.<br>

-Simple Example:<br>

Before cloud-<br>

You needed to buy physical servers, install operating systems, manage hardware, and maintain everything yourself.<br>

With cloud-<br>

You can rent these resources from a cloud provider (like AWS, Azure, or Google Cloud) and use them anytime, from anywhere.<br>

2. **Root User**<br>
-Definition<br>
The root user is the original account created when you first sign up for any cloud platform. It has full, unrestricted access to all resources and services in your cloud account.<br>
-Important Characteristics <br>

Created using your email ID and password during sign-up.<br>

Has complete administrative access — can delete resources, close the account, or change billing information.<br>

Should NOT be used for daily tasks.<br>

cloud platforms like AWS recommends using it only once — to create an IAM admin user.<br>

-Security Best Practices for Root User

Enable MFA (Multi-Factor Authentication) for added protection.<br>

Do not share root credentials with anyone.<br>

Use strong, unique password.<br>

Store root credentials securely (preferably offline).<br>

Use IAM users/roles for all other activities.<br>

3. **IAM (Identity and Access Management) User**<br>
-Definition<br>

IAM is a service that helps you securely manage access to cloud resources.
It lets you create users, groups, and roles and control who can access what in your cloud account.<br>

-IAM User:<br>

An IAM user is a person or application that has specific permissions to use cloud services.<br>

-Example:<br>

You can create-<br>

admin-user (full access)<br>

developer-user (EC2 and S3 access)<br>

billing-user (view billing only)<br>

4.**Roles**<br>

A role defines what actions a user or service can perform.<br>

It gives permissions to perform specific tasks like creating, deleting, or viewing resources.

Roles help assign the right level of access to the right person or service.

-Example:<br>

Admin Role: Full access to all resources.<br>

Developer Role: Can create and modify resources but cannot delete critical ones.<br>

Viewer Role: Can only view resources, not change anything.<br>

5.**Policies**<br>

A policy is a set of rules that decides what actions are allowed or denied for a role or user.<br>

It defines permissions in detail (like read, write, delete).<br>

Policies are attached to roles to control access.<br>

-Example:<br>

A policy can allow:<br>

Viewing storage buckets<br>

Creating virtual machines<br>

Denying deletion of databases<br>