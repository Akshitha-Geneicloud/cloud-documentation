1. What is Cloud Computing?
-Basic Definition

Cloud computing is the on-demand delivery of computing services — like servers, storage, databases, networking, software, analytics, and more — over the internet (“the cloud”) instead of using your local computer or physical servers.

-Simple Example

Before cloud:

You needed to buy physical servers, install operating systems, manage hardware, and maintain everything yourself.

With cloud:

You can rent these resources from a cloud provider (like AWS, Azure, or Google Cloud) and use them anytime, from anywhere.

2. AWS Root User
-Definition

The root user is the original account created when you first sign up for AWS. It has full, unrestricted access to all resources and services in your AWS account.

-Important Characteristics

Created using your email ID and password during sign-up.

Has complete administrative access — can delete resources, close the account, or change billing information.

Should NOT be used for daily tasks.

AWS recommends using it only once — to create an IAM admin user.

-Security Best Practices for Root User

Enable MFA (Multi-Factor Authentication) for added protection.

Do not share root credentials with anyone.

Use strong, unique password.

Store root credentials securely (preferably offline).

Use IAM users/roles for all other activities.

3. IAM (Identity and Access Management) User
-Definition

IAM is a service that helps you securely manage access to AWS resources.
It lets you create users, groups, and roles and control who can access what in your AWS account.

-IAM User

An IAM user is a person or application that has specific permissions to use AWS services.

Example:

You can create:

admin-user (full access)

developer-user (EC2 and S3 access)

billing-user (view billing only)