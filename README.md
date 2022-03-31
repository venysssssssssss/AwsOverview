# Aws overview <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Traditionally, how to build infrastructure](#1-traditionally-how-to-build-infrastructure)
  - [1.1. How websites work](#11-how-websites-work)
  - [1.2. What is a server composed of?](#12-what-is-a-server-composed-of)
  - [1.3. IT Terminology](#13-it-terminology)
  - [1.4. Problems with traditional IT approach](#14-problems-with-traditional-it-approach)
- [2. What is Cloud Computing?](#2-what-is-cloud-computing)
  - [2.1. You've been using some Cloud services](#21-youve-been-using-some-cloud-services)
  - [2.2. The Deployment Models of the Cloud](#22-the-deployment-models-of-the-cloud)
    - [2.2.1. Private Cloud:](#221-private-cloud)
    - [2.2.2. Public Cloud:](#222-public-cloud)
    - [2.2.3. Hybrid Cloud:](#223-hybrid-cloud)
  - [2.3. The Five Characteristics of Cloud Computing](#23-the-five-characteristics-of-cloud-computing)
  - [2.4. Six Advantages of Cloud Computing](#24-six-advantages-of-cloud-computing)
  - [2.5. Problems solved by the Cloud](#25-problems-solved-by-the-cloud)
  - [2.6. Types of Cloud Computing](#26-types-of-cloud-computing)
  - [2.7. Example of Cloud Computing Types](#27-example-of-cloud-computing-types)
  - [2.8. Pricing of the Cloud – Quick Overview](#28-pricing-of-the-cloud--quick-overview)
  - [2.9. AWS Global Infrastructure](#29-aws-global-infrastructure)
  - [2.10. AWS Regions](#210-aws-regions)
  - [2.11. How to choose an AWS Region?](#211-how-to-choose-an-aws-region)
  - [2.12. AWS Availability Zones](#212-aws-availability-zones)
  - [2.13. AWS Points of Presence (Edge Locations)](#213-aws-points-of-presence-edge-locations)
  - [2.14. Tour of the AWS Console](#214-tour-of-the-aws-console)
  - [2.15. Shared Responsibility Model diagram](#215-shared-responsibility-model-diagram)
  - [2.16. AWS Acceptable Use Policy](#216-aws-acceptable-use-policy)
- [3. IAM - Identity and Access Management](#3-iam---identity-and-access-management)
  - [3.1. IAM: Users & Groups](#31-iam-users--groups)
  - [3.2. IAM: Permissions](#32-iam-permissions)
  - [3.3. IAM: Policies Structure](#33-iam-policies-structure)
  - [3.4. IAM – Password Policy](#34-iam--password-policy)
  - [3.5. Multi Factor Authentication - MFA](#35-multi-factor-authentication---mfa)
  - [3.6. MFA devices options in AWS](#36-mfa-devices-options-in-aws)
  - [3.7. How can users access AWS ?](#37-how-can-users-access-aws-)
  - [3.8. What's the AWS CLI?](#38-whats-the-aws-cli)
  - [3.9. What's the AWS SDK?](#39-whats-the-aws-sdk)
  - [3.10. IAM Roles for Services](#310-iam-roles-for-services)
  - [3.11. IAM Security Tools](#311-iam-security-tools)
  - [3.12. IAM Guidelines & Best Practices](#312-iam-guidelines--best-practices)
  - [3.13. Shared Responsibility Model for IAM](#313-shared-responsibility-model-for-iam)
  - [3.14. IAM Section – Summary](#314-iam-section--summary)
- [4. EC2 - Elastic Compute Cloud](#4-ec2---elastic-compute-cloud)
  - [4.1. Amazon EC2](#41-amazon-ec2)
  - [4.2. EC2 sizing & configuration options](#42-ec2-sizing--configuration-options)
  - [4.3. EC2 User Data](#43-ec2-user-data)
  - [4.4. EC2 Instance Types - Overview](#44-ec2-instance-types---overview)
  - [4.5. EC2 Instance Types](#45-ec2-instance-types)
    - [4.5.1. General Purpose](#451-general-purpose)
    - [4.5.2. Compute Optimized](#452-compute-optimized)
    - [4.5.3. Memory Optimized](#453-memory-optimized)
    - [4.5.4. Storage Optimized](#454-storage-optimized)
  - [4.6. EC2 Instance Types: example](#46-ec2-instance-types-example)
  - [4.7. Introduction to Security Groups](#47-introduction-to-security-groups)
  - [4.8. Security Groups Deeper Dive](#48-security-groups-deeper-dive)
  - [4.9. Security Groups Good to know](#49-security-groups-good-to-know)
  - [4.10. Classic Ports to know](#410-classic-ports-to-know)
  - [4.11. How to SSH into your EC2 Instance](#411-how-to-ssh-into-your-ec2-instance)
  - [4.12. EC2 Instance Connect](#412-ec2-instance-connect)
  - [4.13. EC2 Instances Purchasing Options](#413-ec2-instances-purchasing-options)
    - [4.13.1. EC2 On Demand](#4131-ec2-on-demand)
    - [4.13.2. EC2 Reserved Instances](#4132-ec2-reserved-instances)
    - [4.13.3. EC2 Spot Instances](#4133-ec2-spot-instances)
    - [4.13.4. EC2 Dedicated Hosts](#4134-ec2-dedicated-hosts)
    - [4.13.5. EC2 Dedicated Instances](#4135-ec2-dedicated-instances)
  - [4.14. Which purchasing option is right for me? (correlation with Hotel)](#414-which-purchasing-option-is-right-for-me-correlation-with-hotel)
  - [4.15. Shared Responsibility Model for EC2](#415-shared-responsibility-model-for-ec2)
  - [4.16. EC2 Section – Summary](#416-ec2-section--summary)
- [5. EC2 Instance Storage](#5-ec2-instance-storage)
  - [5.1. What's an EBS Volume?](#51-whats-an-ebs-volume)
  - [5.2. EBS Volume](#52-ebs-volume)
  - [5.3. EBS – Delete on Termination attribute](#53-ebs--delete-on-termination-attribute)
  - [5.4. EBS Snapshots](#54-ebs-snapshots)
  - [5.5. AMI Overview](#55-ami-overview)
  - [5.6. AMI Process (from an EC2 instance)](#56-ami-process-from-an-ec2-instance)
  - [5.7. EC2 Image Builder](#57-ec2-image-builder)
  - [5.8. EC2 Instance Store](#58-ec2-instance-store)
  - [5.9. EFS – Elastic File System](#59-efs--elastic-file-system)
  - [5.10. EFS Infrequent Access (EFS-IA)](#510-efs-infrequent-access-efs-ia)
  - [5.11. Shared Responsibility Model for EC2 Storage](#511-shared-responsibility-model-for-ec2-storage)
  - [5.12. Amazon FSx – Overview](#512-amazon-fsx--overview)
    - [5.12.1. Amazon FSx for Windows File Server](#5121-amazon-fsx-for-windows-file-server)
    - [5.12.2. Amazon FSx for Lustre](#5122-amazon-fsx-for-lustre)
  - [5.13. EC2 Instance Storage Summary](#513-ec2-instance-storage-summary)
- [6. Elastic Load Balancing & Auto Scaling Groups Section](#6-elastic-load-balancing--auto-scaling-groups-section)

## 1. Traditionally, how to build infrastructure

### 1.1. How websites work

- Clients have IP addresses.
- Servers have IP addresses.

### 1.2. What is a server composed of?

- Compute: CPU.
- Memory: RAM.
- Storage: Data.
- Database: Store data in a structured way.
- Network: Routers, switch, DNS server.

### 1.3. IT Terminology

- Network: cables, routers and servers connected with each other.
- Router: A networking device that forwards data packets between computer
  networks. They know where to send your packets on the internet!
- Switch: Takes a packet and send it to the correct server / client on your network.

### 1.4. Problems with traditional IT approach

- Pay for the rent for the data center.
- Pay for power supply, cooling, maintenance.
- Adding and replacing hardware takes time.
- Scaling is limited.
- Hire 24/7 team to monitor the infrastructure.
- How to deal with disasters? (earthquake, power shutdown, fire…).
- Can we externalize all this? **CLOUD**

## 2. What is Cloud Computing?

- Cloud computing is the on-demand delivery of compute power, database storage, applications, and other IT resources.
- Through a cloud services platform with pay-as-you-go pricing.
- You can provision exactly the right type and size of computing resources you need.
- You can access as many resources as you need, almost instantly.
- Simple way to access servers, storage, databases and a set of application services.
- Amazon Web Services owns and maintains the network-connected hardware required for these application services, while you provision and use what you need via a web application.

### 2.1. You've been using some Cloud services

- Gmail
  - E-mail cloud service
  - Pay for ONLY your emails stored (no infrastructure, etc.)
- Dropbox
  - Cloud Storage Service
  - Originally built on AWS
- Netflix
  - Built on AWS
  - Video on Demand

### 2.2. The Deployment Models of the Cloud

#### 2.2.1. Private Cloud:

- Cloud services used by a single organization, not exposed to the public.
- Complete control.
- Security for sensitive applications.
- Meet specific business needs.

#### 2.2.2. Public Cloud:

- Cloud resources owned and operated by a thirdparty cloud service provider delivered over the Internet.

#### 2.2.3. Hybrid Cloud:

- Keep some servers on premises and extend some capabilities to the Cloud
- Control over sensitive assets in your private infrastructure
- Flexibility and costeffectiveness of the public cloud

### 2.3. The Five Characteristics of Cloud Computing

- On-demand self service:
  - Users can provision resources and use them without human interaction from the service provider
- Broad network access:
  - Resources available over the network, and can be accessed by diverse client platforms
- Multi-tenancy and resource pooling:
  - Multiple customers can share the same infrastructure and applications with security and privacy
  - Multiple customers are serviced from the same physical resources
- Rapid elasticity and scalability:
  - Automatically and quickly acquire and dispose resources when needed
  - Quickly and easily scale based on demand
- Measured service:
  - Usage is measured, users pay correctly for what they have used

### 2.4. Six Advantages of Cloud Computing

- Trade capital expense (CAPEX) for operational expense (OPEX)
  - Pay On-Demand: don't own hardware
  - Reduced Total Cost of Ownership (TCO) & Operational Expense (OPEX)
- Benefit from massive economies of scale
  - Prices are reduced as AWS is more efficient due to large scale
- Stop guessing capacity
  - Scale based on actual measured usage
- Increase speed and agility
- Stop spending money running and maintaining data centers
- Go global in minutes: leverage the AWS global infrastructure

### 2.5. Problems solved by the Cloud

- Flexibility: change resource types when needed
- Cost-Effectiveness: pay as you go, for what you use
- Scalability: accommodate larger loads by making hardware stronger or adding additional nodes
- Elasticity: ability to scale out and scale-in when needed
- High-availability and fault-tolerance: build across data centers
- Agility: rapidly develop, test and launch software applications

### 2.6. Types of Cloud Computing

- Infrastructure as a Service (IaaS)
  - Provide building blocks for cloud IT
  - Provides networking, computers, data storage space
  - Highest level of flexibility
  - Easy parallel with traditional on-premises IT
- Platform as a Service (PaaS)
  - Removes the need for your organization to manage the underlying infrastructure
  - Focus on the deployment and management of your applications
- Software as a Service (SaaS)
  - Completed product that is run and managed by the service provider

### 2.7. Example of Cloud Computing Types

- Infrastructure as a Service
  - Amazon EC2 (on AWS)
  - GCP, Azure, Rackspace, Digital Ocean, Linode
- Platform as a Service:
  - Elastic Beanstalk (on AWS)
  - Heroku, Google App Engine (GCP), Windows Azure (Microsoft)
- Software as a Service:
  - Many AWS services (ex: Rekognition for Machine Learning)
  - Google Apps (Gmail), Dropbox, Zoom

### 2.8. Pricing of the Cloud – Quick Overview

- AWS has 3 pricing fundamentals, following the pay-as-you-go pricing
  model
  - Compute:
    - Pay for compute time
  - Storage:
    - Pay for data stored in the Cloud
  - Data transfer **OUT** of the Cloud:
    - Data transfer IN is free
  - Solves the expensive issue of traditional IT

### 2.9. AWS Global Infrastructure

- AWS Regions
- AWS Availability Zones
- AWS Data Centers
- AWS Edge Locations / Points of Presence
- https://infrastructure.aws/

### 2.10. AWS Regions

- AWS has Regions all around the world
- Names can be us-east-1, eu-west-3…
- A region is a cluster of data centers
- Most AWS services are region-scoped

### 2.11. How to choose an AWS Region?

- **Compliance** with data governance and legal requirements: data never leaves a region without your explicit permission
- **Proximity** to customers: reduced latency
- **Available** services within a Region: new services and new features aren't available in every Region
- **Pricing**: pricing varies region to region and is transparent in the service pricing page

### 2.12. AWS Availability Zones

- Each region has many availability zones (usually 3, min is 2, max is 6). Example:
  - ap-southeast-2a
  - ap-southeast-2b
  - ap-southeast-2c
- Each availability zone (AZ) is one or more discrete data centers with redundant power, networking, and connectivity
- They're separate from each other, so that they're isolated from disasters
- They're connected with high bandwidth, ultra-low latency networking

### 2.13. AWS Points of Presence (Edge Locations)

- Amazon has 216 Points of Presence (205 Edge Locations & 11 Regional Caches) in 84 cities across 42 countries
- Content is delivered to end users with lower latency

### 2.14. Tour of the AWS Console

- AWS has Global Services:
  - Identity and Access Management (IAM)
  - Route 53 (DNS service)
  - CloudFront (Content Delivery Network)
  - WAF (Web Application Firewall)
- Most AWS services are Region-scoped:
  - Amazon EC2 (Infrastructure as a Service)
  - Elastic Beanstalk (Platform as a Service)
  - Lambda (Function as a Service)
  - Rekognition (Software as a Service)
- Region Table: https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services

### 2.15. Shared Responsibility Model diagram

- AWS = RESPONSIBILITY FOR THE SECURITY OF THE CLOUD
- CUSTOMER = RESPONSIBILITY FOR THE SECURITY IN THE CLOUD
- https://aws.amazon.com/compliance/shared-responsibility-model/

### 2.16. AWS Acceptable Use Policy

- https://aws.amazon.com/aup/
- No Illegal, Harmful, or Offensive Use or Content
- No Security Violations
- No Network Abuse
- No E-Mail or Other Message Abuse

## 3. IAM - Identity and Access Management

### 3.1. IAM: Users & Groups

- IAM = Identity and Access Management, **Global service**.
- **Root account** created by default, shouldn't be used or shared.
- **Users** are people within your organization, and can be grouped.
- **Groups** only contain users, not other groups.
- Users don't have to belong to a group, and user can belong to multiple groups.

### 3.2. IAM: Permissions

- **Users or Groups** can be assigned JSON documents called policies.
- These policies define the **permissions** of the users.
- In AWS you apply the **least privilege** principle: don't give more permissions than a user needs.

### 3.3. IAM: Policies Structure

- Consists of:
  - **Version**: policy language version, always include "YYYY-MM-DD".
  - **Id**: an identifier for the policy (optional).
  - **Statement**: one or more individual statements (required).
- Statements consists of:
  - **Sid**: an identifier for the statement (optional).
  - **Effect**: whether the statement allows or denies access (Allow, Deny).
  - **Principal**: account/user/role to which this policy applied to.
  - **Action**: list of actions this policy allows or denies.
  - **Resource**: list of resources to which the actions applied to.
  - **Condition**: conditions for when this policy is in effect (optional).

### 3.4. IAM – Password Policy

- Strong passwords = higher security for your account.
- In AWS, you can setup a password policy:
  - Set a minimum password length.
  - Require specific character types:
    - Including uppercase letters.
    - Lowercase letters.
    - Numbers.
    - Non-alphanumeric characters.
- Allow all IAM users to change their own passwords.
- Require users to change their password after some time (password expiration).
- Prevent password re-use.

### 3.5. Multi Factor Authentication - MFA

- Users have access to your account and can possibly change configurations or delete resources in your AWS account
- **You want to protect your Root Accounts and IAM users**
- MFA = password you know + security device you own
- Main benefit of MFA:
  - If a password is stolen or hacked, the account is not compromised

### 3.6. MFA devices options in AWS

- Virtual MFA device:
  - Google AuthenLcator (phone only).
  - Authy (multi-device).
  - Support for multiple tokens on a single device.
- Universal 2nd Factor (U2F) Security Key:
  - YubiKey by Yubico (3rd party)
    - Support for multiple tokens on a single device. Support for multiple root and IAM users using a single security key
- Hardware Key Fob MFA Device:
  - Provided by Gemalto (3rd party)
- Hardware Key Fob MFA Device for AWS GovCloud (US):
  - Provided by SurePassID (3rd party)

### 3.7. How can users access AWS ?

- To access AWS, you have three options:
  - AWS Management Console (protected by password + MFA).
  - AWS Command Line Interface (CLI): protected by access keys.
  - AWS Software Developer Kit (SDK) - for code: protected by access keys.
- Access Keys are generated through the AWS Console.
- Users manage their own access keys.
- Access Keys are secret, just like a password. Don't share them.
- Access Key ID ~= username.
- Secret Access Key ~= password.
- Example (Fake) Access Keys.
  - Access key ID: AKIASK4E37PV4983d6C.
  - Secret Access Key: AZPN3zojWozWCndIjhB0Unh8239a1bzbzO5fqqkZq.
  - **Remember: don't share your access keys.**

### 3.8. What's the AWS CLI?

- A tool that enables you to interact with AWS services using commands in your command-line shell
- Direct access to the public APIs of AWS services
- You can develop scripts to manage your resources
- It's open-source https://github.com/aws/aws-cli
- Alternative to using AWS Management Console

### 3.9. What's the AWS SDK?

- AWS Software Development Kit (AWS SDK)
- Language-specific APIs (set of libraries)
- Enables you to access and manage AWS services programmatically
- Embedded within your application
- Supports:
  - SDKs (JavaScript, Python, PHP, .NET, Ruby, Java, Go, Node.js, C++, C#)
  - Mobile SDKs (Android, iOS, …)
  - IoT Device SDKs (Embedded C, Arduino, …)
- Example: AWS CLI is built on AWS SDK for Python

### 3.10. IAM Roles for Services

- Some AWS service will need to perform actions on your behalf.
- To do so, we will assign permissions to AWS services with IAM Roles.
- Common roles:
  - EC2 Instance Roles.
  - Lambda Function Roles.
  - Roles for CloudFormation.

### 3.11. IAM Security Tools

- IAM Credentials Report (account-level)
  - A report that lists all your account's users and the status of their various credentials
- IAM Access Advisor (user-level)
  - Access advisor shows the service permissions granted to a user and when those services were last accessed.
  - You can use this information to revise your policies.

### 3.12. IAM Guidelines & Best Practices

- Don't use the root account except for AWS account setup
- One physical user = One AWS user
- **Assign users to groups** and assign permissions to groups
- Create a **strong password policy**
- Use and enforce the use of **Multi Factor Authentication (MFA)**
- Create and use **Roles** for giving permissions to AWS services
- Use Access Keys for Programmatic Access (CLI / SDK)
- Audit permissions of your account with the IAM Credentials Report
- **Never share IAM users & Access Keys**

### 3.13. Shared Responsibility Model for IAM

- AWS:
  - Infrastructure (global network security)
  - Configuration and vulnerability analysis
  - Compliance validation
- You (Customer):
  - Users, Groups, Roles, Policies management and monitoring
  - Enable MFA on all accounts
  - Rotate all your keys often
  - Use IAM tools to apply appropriate permissions
  - Analyze access patterns & review permissions

### 3.14. IAM Section – Summary

- **Users**: mapped to a physical user, has a password for AWS Console
- **Groups**: contains users only
- **Policies**: JSON document that outlines permissions for users or groups
- **Roles**: for EC2 instances or AWS services
- **Security**: MFA + Password Policy
- **AWS CLI**: manage your AWS services using the command-line
- **AWS SDK**: manage your AWS services using a programming language
- **Access Keys**: access AWS using the CLI or SDK
- **Audit**: IAM Credential Reports & IAM Access Advisor

## 4. EC2 - Elastic Compute Cloud

### 4.1. Amazon EC2

- EC2 is one of the most popular of AWS' offering.
- EC2 = Elastic Compute Cloud = Infrastructure as a Service.
- It mainly consists in the capability of:
  - Renting virtual machines (EC2).
  - Storing data on virtual drives (EBS).
  - Distributing load across machines (ELB).
  - Scaling the services using an auto-scaling group (ASG).
- Knowing EC2 is fundamental to understand how the Cloud works.

### 4.2. EC2 sizing & configuration options

- Operating System (OS): Linux, Windows or Mac OS.
- How much compute power & cores (CPU).
- How much random-access memory (RAM).
- How much storage space:
  - Network-attached (EBS & EFS).
  - hardware (EC2 Instance Store).
- Network card: speed of the card, Public IP address.
- Firewall rules: security group.
- Bootstrap script (configure at first launch): EC2 User Data.

### 4.3. EC2 User Data

- It is possible to bootstrap our instances using an EC2 User data script.
- **bootstrapping** means launching commands when a machine starts.
- That script is **only run once** at the instance first start.
- EC2 user data is used to automate boot tasks such as:
  - Installing updates.
  - Installing software.
  - Downloading common files from the internet.
  - Anything you can think of.
- The EC2 User Data Script runs with the root user.

### 4.4. EC2 Instance Types - Overview

- You can use different types of EC2 instances that are optimised for different use cases (https://aws.amazon.com/ec2/instance-types/).
- AWS has the following naming convention:
  - m5.2xlarge
    - m: instance class
    - 5: generation (AWS improves them over time)
    - 2xlarge: size within the instance class

### 4.5. EC2 Instance Types

#### 4.5.1. General Purpose

- Great for a diversity of workloads such as web servers or code repositories.
- Balance between:
  - Compute.
  - Memory.
  - Networking.
- In the course, we will be using the t2.micro which is a General Purpose EC2 instance.

#### 4.5.2. Compute Optimized

- Great for compute-intensive tasks that require high performance processors:
  - Batch processing workloads.
  - Media transcoding.
  - High performance web servers.
  - High performance computing (HPC).
  - Scientific modeling & machine learning.
  - Dedicated gaming servers.

#### 4.5.3. Memory Optimized

- Fast performance for workloads that process large data sets in memory
- Use cases:
- High performance, relational/non-relational databases
- Distributed web scale cache stores
- In-memory databases optimized for BI (business intelligence)
- Applications performing real-time processing of big unstructured data

#### 4.5.4. Storage Optimized

- Great for storage-intensive tasks that require high, sequential read and write
  access to large data sets on local storage
- Use cases:
- High frequency online transaction processing (OLTP) systems
- Relational & NoSQL databases
- Cache for in-memory databases (for example, Redis)
- Data warehousing applications
- Distributed file systems

### 4.6. EC2 Instance Types: example

| Instance    | vCPU | Mem (GiB) | Storage          | Network performance | EBS Banwidth () |
| ----------- | ---- | --------- | ---------------- | ------------------- | --------------- |
| t2.micro    | 1    | 1         | EBS-Only         | Low to Moderate     |                 |
| t2.xlarge   | 4    | 16        | EBS-Only         | Moderate            |                 |
| c5d.4xlarge | 16   | 32        | 1 x 400 NVMe SSD | Up to 10 Gbps       | 4,750           |
| r5.16xlarge | 64   | 512       | EBS-Only         | 20 Gbps             | 13,600          |
| m5.8xlarge  | 32   | 128       | EBS-Only         | 10 Gbps             | 6,800           |

- t2.micro is part of the AWS free tier (up to 750 hours per month)

### 4.7. Introduction to Security Groups

- Security Groups are the fundamental of network security in AWS
- They control how traffic is allowed into or out of our EC2 Instances.
- Security groups only contain **allow** rules
- Security groups rules can reference by IP or by security group

### 4.8. Security Groups Deeper Dive

- Security groups are acting as a "firewall" on EC2 instances
- They regulate:
  - Access to Ports
  - Authorised IP ranges IPv4 and IPv6
  - Control of inbound network (from other to the instance)
  - Control of outbound network (from the instance to other)

### 4.9. Security Groups Good to know

- Can be attached to multiple instances
- Locked down to a region / VPC combination
- Does live "outside" the EC2 – if traffic is blocked the EC2 instance won't see it
- It's good to maintain one separate security group for SSH access
- If your application is not accessible (time out), then it's a security group issue
- If your application gives a "connection refused" error, then it's an application error or it's not launched
- All inbound traffic is **blocked** by default
- All outbound traffic is **authorised** by default

### 4.10. Classic Ports to know

- 22 = SSH (Secure Shell) - log into a Linux instance
- 21 = FTP (File Transfer Protocol) – upload files into a file share
- 22 = SFTP (Secure File Transfer Protocol) – upload files using SSH
- 80 = HTTP – access unsecured websites
- 443 = HTTPS – access secured websites
- 3389 = RDP (Remote Desktop Protocol) – log into a Windows instance

### 4.11. How to SSH into your EC2 Instance

- Windows
- We'll learn how to SSH into your EC2 instance using Windows
- Configure pem file
  ![Permission Propertie Aws PemFile](/Images/PermissionPropertieAwsPemFile.png)
- Command
  - ssh -i D:\MY_PENFILE.pem ec2-user@PUBLIC_IP

### 4.12. EC2 Instance Connect

- Connect to your EC2 instance within your browser
- No need to use your key file that was downloaded
- The "magic" is that a temporary key is uploaded onto EC2 by AWS
- **Works only out-of-the-box with Amazon Linux 2**
- Need to make sure the port 22 is still opened!

### 4.13. EC2 Instances Purchasing Options

- On-Demand Instances: short workload, predictable pricing
- Reserved: (MINIMUM 1 year)
  - Reserved Instances: long workloads
  - Convertible Reserved Instances: long workloads with flexible instances
  - Scheduled Reserved Instances: example – every Thursday between 3 and 6 pm
- Spot Instances: short workloads, cheap, can lose instances (less reliable)
- Dedicated Hosts: book an entire physical server, control instance placement
- Dedicated Instances: no other customers will share your hardware

#### 4.13.1. EC2 On Demand

- Pay for what you use:
  - Linux or Windows - billing per second, after the first minute
  - All other operating systems - billing per hour
- Has the highest cost but no upfront payment
- No long-term commitment
- Recommended for **short-term** and **un-interrupted workloads**, where you can't predict how the application will behave

#### 4.13.2. EC2 Reserved Instances

- Up to 72% discount compared to On-demand
- Reservation period: 1 year = + discount | 3 years = +++ discount
- Purchasing options: no upfront | partial upfront = + | All upfront = ++ discount
- Reserve a specific instance type
- Recommended for steady-state usage applications (think database)
- Convertible Reserved Instance
  - Can change the EC2 instance type
  - Up to 45% discount
- Scheduled Reserved Instances
  - Launch within time window you reserve
  - When you require a fraction of day / week / month
  - Commitment for 1 year only

#### 4.13.3. EC2 Spot Instances

- Can get a discount of up to 90% compared to On-demand
- Instances that you can "lose" at any point of time if your max price is less than the
  current spot price
- The MOST cost-efficient instances in AWS
- Useful for workloads that are resilient to failure
  - Batch jobs
  - Data analysis
  - Image processing
  - Any distributed workloads
  - Workloads with a flexible start and end time
- Not suitable for critical jobs or databases

#### 4.13.4. EC2 Dedicated Hosts

- An Amazon EC2 Dedicated Host is a physical server with EC2 instance capacity fully dedicated to your use. Dedicated Hosts can help you address compliance requirements and reduce costs by allowing you to use your existing server-bound software licenses.
- Allocated for your account for a 3-year period reservation
- More expensive
- Useful for software that have complicated licensing model (BYOL – Bring Your Own License)
- Or for companies that have strong regulatory or compliance needs

#### 4.13.5. EC2 Dedicated Instances

- Instances running on hardware that's dedicated to you
- May share hardware with other instances in same account
- No control over instance placement (can move hardware after Stop / Start)

### 4.14. Which purchasing option is right for me? (correlation with Hotel)

- On demand: coming and staying in resort whenever we like, we pay the full price
- Reserved: like planning ahead and if we plan to stay for a long time, we may get a good discount.
- Spot instances: the hotel allows people to bid for the empty rooms and the highest bidder keeps the rooms. You can get kicked out at any time
- Dedicated Hosts: We book an entire building of the resort

### 4.15. Shared Responsibility Model for EC2

- AWS:
  - Infrastructure (global network security)
  - Isolation on physical hosts
  - Replacing faulty hardware
  - Compliance validation
- You:
  - Security Groups rules
  - Operating-system patches and updates
  - Software and utilities installed on the EC2 instance
  - IAM Roles assigned to EC2ASDASD\_\_& IAM user access management
  - Data security on your instance

### 4.16. EC2 Section – Summary

- EC2 Instance: AMI (OS) + Instance Size (CPU + RAM) + Storage + security groups + EC2 User Data.
- Security Groups: Firewall attached to the EC2 instance.
- EC2 User Data: Script launched at the first start of an instance.
- SSH: start a terminal into our EC2 Instances (port 22).
- EC2 Instance Role: link to IAM roles.
- Purchasing Options: On-Demand, Spot, Reserved (Standard + Convertible + Scheduled), Dedicated Host, Dedicated Instance.

## 5. EC2 Instance Storage

### 5.1. What's an EBS Volume?

- An **EBS (Elastic Block Store) Volume** is a **network drive** you can attach to your instances while they run.
- It allows your instances to persist data, even after their termination.
- **They can only be mounted to one instance at a time (at the CCP level)**.
- They are bound to a specific **availability zone (AZ)**.
- Analogy: Think of them as a "network USB stick".

### 5.2. EBS Volume

- It's a network drive (i.e. not a physical drive).
  - It uses the network to communicate the instance, which means there might be a bit of latency.
  - It can be detached from an EC2 instance and attached to another one quickly.
- It's locked to an Availability Zone (AZ).
  - An EBS Volume in us-east-1a cannot be attached to us-east-1b.
  - To move a volume across, you first need to **snapshot** it.
- Have a provisioned capacity (size in GBs, and IOPS).
  - You get billed for all the provisioned capacity.
  - You can increase the capacity of the drive over time.

### 5.3. EBS – Delete on Termination attribute

- Controls the EBS behaviour when an EC2 instance terminates
  - By default, the root EBS volume is deleted (attribute enabled)
  - By default, any other attached EBS volume is not deleted (attribute disabled)
- This can be controlled by the AWS console / AWS CLI
- **Use case: preserve root volume when instance is terminated**

### 5.4. EBS Snapshots

- Make a backup (snapshot) of your EBS volume at a point in time.
- Not necessary to detach volume to do snapshot, **but recommended**.
- Can copy snapshots across AZ or Region.

### 5.5. AMI Overview

- AMI = Amazon Machine Image.
- AMI are a **customization** of an EC2 instance.
  - You add your own software, configuration, operating system, monitoring...
  - Faster boot / configuration time because all your software is pre-packaged
- AMI are built for a **specific region** (and can be copied across regions).
- You can launch EC2 instances from:
  - A Public AMI: AWS provided
  - Your own AMI: you make and maintain them yourself
  - An AWS Marketplace AMI: an AMI someone else made (and potentially sells)

### 5.6. AMI Process (from an EC2 instance)

- Start an EC2 instance and customize it.
- Stop the instance (for data integrity).
- Build an AMI – this will also create EBS snapshots.
- Launch instances from other AMIs.

### 5.7. EC2 Image Builder

- Used to automate the creation of Virtual Machines or container images.
- => Automate the creation, maintain, validate and test **EC2 AMIs**.
- Can be run on a schedule (weekly, whenever packages are updated, etc...).
- Free service (only pay for the underlying resources).
- Steps:
  1. EC2 Image Builder
     1. Create
  2. Builder EC2 Instance
     1. Create
  3. New AMI
  4. Test EC2 Instance

### 5.8. EC2 Instance Store

- EBS volumes are **network drives** with good but "limited" performance.
- **If you need a high-performance hardware disk, use EC2 Instance Store**.
- Better I/O performance.
- EC2 Instance Store lose their storage if they're stopped (ephemeral).
- Good for buffer / cache / scratch data / temporary content.
- Risk of data loss if hardware fails.
- Backups and Replication are your responsibility.

### 5.9. EFS – Elastic File System

- Managed NFS (network file system) that **can be mounted on 100s of EC2**.
- EFS works with **Linux** EC2 instances in **multi-AZ**.
- Highly available, scalable, expensive (3x gp2), pay per use, no capacity planning.

### 5.10. EFS Infrequent Access (EFS-IA)

- **Storage class** that is cost-optimized for files not accessed every day.
- Up to 92% lower cost compared to EFS Standard.
- EFS will automatically move your files to EFS-IA based on the last time they were accessed.
- Enable EFS-IA with a Lifecycle Policy.
- Example: move files that are not accessed for 60 days to EFS-IA.
- Transparent to the applications accessing EFS.

### 5.11. Shared Responsibility Model for EC2 Storage

- AWS
  - Infrastructure.
  - Replication for data for EBS volumes & EFS drives.
  - Replacing faulty hardware.
  - Ensuring their employees cannot access your data.
- You
  - Setting up backup / snapshot procedures.
  - Setting up data encryption.
  - Responsibility of any data on the drives.
  - Understanding the risk of using EC2 Instance Store.

### 5.12. Amazon FSx – Overview

- Launch 3rd party high-performance file systems on AWS.
- Fully managed service.
- Products:
  - FSx for Lustre
  - FSx for Windows File Server
  - FSx for NetApp ONTAP

#### 5.12.1. Amazon FSx for Windows File Server

- A fully managed, highly reliable, and scalable Windows native shared file system.
- Built on Windows File Server.
- Supports SMB protocol & Windows NTFS.
- Integrated with Microsoft Active Directory.
- Can be accessed from AWS or your on-premise infrastructure.

#### 5.12.2. Amazon FSx for Lustre

- A fully managed, high-performance, scalable file storage for **High Performance Computing (HPC)**.
- The name Lustre is derived from "Linux" and "cluster".
- Machine Learning, Analytics, Video Processing, Financial Modeling, ...
- Scales up to 100s GB/s, millions of IOPS, sub-ms latencies.

### 5.13. EC2 Instance Storage Summary

- EBS volumes:
  - Network drives attached to one EC2 instance at a time
  - Mapped to an Availability Zones
  - Can use EBS Snapshots for backups / transferring EBS volumes across AZ
- AMI: create ready-to-use EC2 instances with our customizations
- EC2 Image Builder: automatically build, test and distribute AMIs
- EC2 Instance Store:
  - High performance hardware disk attached to our EC2 instance
  - Lost if our instance is stopped / terminated
- EFS: network file system, can be attached to 100s of instances in a region
- EFS-IA: cost-optimized storage class for infrequent accessed files
- FSx for Windows: Network File System for Windows servers
- FSx for Lustre: High Performance Computing Linux file system

## 6. Elastic Load Balancing & Auto Scaling Groups Section
