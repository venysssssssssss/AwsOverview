# AWS ELB - Elastic Load Balancing and AWS ASG - Auto Scaling Groups<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Scalability and High Availability](#1-scalability-and-high-availability) - [1.1. Vertical Scalability](#11-vertical-scalability) - [1.2. Horizontal Scalability](#12-horizontal-scalability)
  - [2. High Availability](#2-high-availability)
  - [3. High Availability \& Scalability For EC2](#3-high-availability--scalability-for-ec2)
  - [4. Scalability vs Elasticity (vs Agility)](#4-scalability-vs-elasticity-vs-agility)
  - [5. What is load balancing?](#5-what-is-load-balancing)
    - [5.1. Why use a load balancer?](#51-why-use-a-load-balancer)
    - [5.2. Why use an Elastic Load Balancer (ELB)?](#52-why-use-an-elastic-load-balancer-elb)
  - [6. Health Checks](#6-health-checks)
  - [7. Types of load balancer on AWS](#7-types-of-load-balancer-on-aws)
    - [7.1. Classic Load Balancers (v1)](#71-classic-load-balancers-v1)
    - [7.2. Application Load Balancer (v2)](#72-application-load-balancer-v2)
    - [7.3. Network Load Balancer (v2)](#73-network-load-balancer-v2)
    - [7.4. Network Load Balancer - Target Groups](#74-network-load-balancer---target-groups)
    - [7.5. Gateway Load Balancer](#75-gateway-load-balancer)
    - [7.6. Gateway Load Balancer -Target Groups](#76-gateway-load-balancer--target-groups)
  - [8. Sticky Sessions (Session Affinity)](#8-sticky-sessions-session-affinity)
    - [8.1. Sticky Sessions - Cookie Names](#81-sticky-sessions---cookie-names)
  - [9. Cross-Zone Load Balancing](#9-cross-zone-load-balancing)
  - [10. SSL/TLS - Basics](#10-ssltls---basics)
    - [10.1. Load Balancer - SSL Certificates](#101-load-balancer---ssl-certificates)
  - [11. SSL - Server Name Indication (SNI)](#11-ssl---server-name-indication-sni)
  - [12. Elastic Load Balancers - SSL Certificates](#12-elastic-load-balancers---ssl-certificates)
  - [13. Connection Draining](#13-connection-draining)
  - [14. What's an Auto Scaling Group?](#14-whats-an-auto-scaling-group)
  - [15. Auto Scaling Group Attributes](#15-auto-scaling-group-attributes)
  - [16. Auto Scaling - CloudWatch Alarms \& Scaling](#16-auto-scaling---cloudwatch-alarms--scaling)
  - [17. Auto Scaling Groups - Dynamic Scaling Policies](#17-auto-scaling-groups---dynamic-scaling-policies)
  - [18. Auto Scaling Groups - Predictive Scaling](#18-auto-scaling-groups---predictive-scaling)
  - [19. Good metrics to scale on](#19-good-metrics-to-scale-on)
  - [20. Auto Scaling Groups - Scaling Cooldowns](#20-auto-scaling-groups---scaling-cooldowns)
  - [21. Auto Scaling Groups - Scaling Strategies (Resume)](#21-auto-scaling-groups---scaling-strategies-resume)

# 1. Scalability and High Availability

- Scalability means that an application / system can handle greater loads by adapting.
- There are two kinds of scalability:
  - Vertical Scalability.
  - Horizontal Scalability (= elasticity).
- **Scalability is linked but different to High Availability.**

## 1.1. Vertical Scalability

- Vertical Scalability means increasing the size of the instance.
- For example, your application runs on a t2.micro.
  - Scaling that application vertically means running it on a t2.large.
- Vertical scalability is very common for **NON** distributed systems, such as a database.
- RDS, ElastiCache are services that can scale vertically.
- There's usually a limit to how much you can vertically scale (hardware limit).

## 1.2. Horizontal Scalability

- Horizontal Scalability means increasing the number of instances / systems for your application.
- Horizontal scaling implies distributed systems.
- This is very common for web applications / modern applications.
- It's easy to horizontally scale thanks the cloud offerings such as Amazon EC2.

## 1.3. High Availability

- High Availability usually goes **hand in hand with Horizontal Scaling**.
- High availability means running your application / system in at least 2 data centers (Availability Zones).
- The goal of high availability is to survive a data center loss (disaster).
- The high availability can be passive (for RDS Multi AZ for example).
- The high availability can be active (for horizontal scaling).

## 1.4. High Availability and Scalability for EC2

- **Vertical Scaling:** Increase instance size (= scale up / down).
  - From: t2.nano - 0.5G of RAM, 1 vCPU.
  - To: u-12tb1.metal - 12.3 TB of RAM, 448 vCPUs.
- **Horizontal Scaling:** Increase number of instances (= scale out / in).
  - Auto Scaling Group.
  - Load Balancer.
- **High Availability:** Run instances for the same application across multi AZ.
  - Auto Scaling Group multi AZ.
  - Load Balancer multi AZ.

## 1.5. Scalability vs Elasticity (vs Agility)

- **Scalability:** Ability to accommodate a larger load by making the hardware stronger (scale up), or by adding nodes (scale out).
- **Elasticity:** Once a system is scalable, elasticity means that there will be some "auto-scaling" so that the system can scale based on the load.
  - This is "cloud-friendly": pay-per-use, match demand, optimize costs.
- **Agility:** (not related to scalability - distractor) new IT resources are only a click away, which means that you reduce the time to make those resources available to your developers from weeks to just minutes.

# 2. What is Load Balancing?

- Load balancers are servers that forward internet traffic to multiple servers (EC2 Instances) downstream.

![Load Balance](Images/LoadBalanceDiagram.png)

# 3. Why use a load balancer?

- Spread load across multiple downstream instances.
- Expose a single point of access (DNS) to your application.
- Seamlessly handle failures of downstream instances.
- Do regular health checks to your instances.
- Provide SSL termination (HTTPS) for your websites.
- Enforce stickiness with cookies.
- High availability across zones.
- Separate public traffic from private traffic.

## 3.1. Why use an Elastic Load Balancer (ELB)?

- An ELB (Elastic Load Balancer) is a **managed load balancer**:
  - AWS guarantees that it will be working.
  - AWS takes care of upgrades, maintenance, high availability.
  - AWS provides only a few configuration knobs.
- It costs less to setup your own load balancer but it will be a lot more effort on your end (maintenance, integrations).
- It is integrated with many AWS offerings / services:
  - EC2, EC2 Auto Scaling Groups, Amazon ECS.
  - AWS Certificate Manager (ACM), CloudWatch.
  - Route 53, AWS WAF, AWS Global Accelerator.
- 3 kinds of load balancers offered by AWS:
  - Application Load Balancer (HTTP / HTTPS only) - Layer 7.
  - Network Load Balancer (ultra-high performance, allows for TCP) - Layer 4.
  - Classic Load Balancer (slowly retiring) - Layer 4 & 7.

### 3.1.1. Health Checks

- Health Checks are crucial for Load Balancers.
- They enable the load balancer to know if instances it forwards traffic to are available to reply to requests.
- The health check is done on a port and a route (/health is common).
- If the response is not 200 (OK), then the instance is unhealthy.

### 3.1.2. Types of load balancer on AWS

- AWS has **4 kinds of managed Load Balancers**.
- Classic Load Balancer (v1 - old generation) - 2009 - CLB.
  - HTTP, HTTPS, TCP, SSL (secure TCP).
- Application Load Balancer (v2 - new generation) - 2016 - ALB.
  - HTTP, HTTPS, WebSocket.
- Network Load Balancer (v2 - new generation) - 2017 - NLB.
  - TCP, TLS (secure TCP), UDP.
- Gateway Load Balancer - 2020 - GWLB.
  - Operates at layer 3 (Network layer) - IP Protocol.
- Overall, it is recommended to use the newer generation load balancers as they provide more features.
- Some load balancers can be setup as internal (private) or external (public) ELBs.

#### 3.1.2.1. Classic Load Balancers (v1)

- Supports TCP (Layer 4), HTTP & HTTPS (Layer 7).
- Health checks are TCP or HTTP based.
- Fixed hostname XXX.region.elb.amazonaws.com.

#### 3.1.2.2. Application Load Balancer (v2)

- Application load balancers is Layer 7 (HTTP).
- Load balancing to multiple HTTP applications across machines (target groups).
- Load balancing to multiple applications on the same machine (ex: containers).
- Support for HTTP/2 and WebSocket.
- Support redirects (from HTTP to HTTPS for example).
- Routing tables to different target groups:
  - Routing based on path in URL (example.com/users & example.com/posts)
  - Routing based on hostname in URL (one.example.com & other.example.com)
  - Routing based on Query String, Headers (example.com/users?id=123&order=false)
- ALB are a great fit for micro services & container-based application (example: Docker & Amazon ECS)
- Has a port mapping feature to redirect to a dynamic port in ECS
- In comparison, we'd need multiple Classic Load Balancer per application
- EC2 instances (can be managed by an Auto Scaling Group) - HTTP.
- ECS tasks (managed by ECS itself) - HTTP.
- Lambda functions - HTTP request is translated into a JSON event.
- IP Addresses - must be private IPs.
- ALB can route to multiple target groups.
- Health checks are at the target group level.
- Fixed hostname (XXX.region.elb.amazonaws.com).
- The application servers don't see the IP of the client directly.
  - The true IP of the client is inserted in the header X-Forwarded-For.
  - We can also get Port (X-Forwarded-Port) and proto (X-Forwarded-Proto).

#### 3.1.2.3. Network Load Balancer (v2)

- Network load balancers (Layer 4) allow to:
  - Forward TCP & UDP traffic to your instances.
  - Handle millions of request per seconds.
  - Less latency ~100 ms (vs 400 ms for ALB).
- NLB has one static IP per AZ, and supports assigning Elastic IP (helpful for whitelisting specific IP).
- NLB are used for extreme performance, TCP or UDP traffic.
- Not included in the AWS free tier.

#### 3.1.2.4. Network Load Balancer - Target Groups

- EC2 instances.
- IP Addresses - must be private IPs.
- Application Load Balancer.

#### 3.1.2.5. Gateway Load Balancer

- Deploy, scale, and manage a fleet of 3rd party network virtual appliances in AWS.
- Example: Firewalls, Intrusion Detection and Prevention Systems, Deep Packet Inspection Systems, payload manipulation, ...
- Operates at Layer 3 (Network Layer) - IP Packets.
- Combines the following functions:.
  - **Transparent Network Gateway** - single entry/exit for all traffic.
  - **Load Balancer** - distributes traffic to your virtual appliances.
- Uses the GENEVE protocol on port 6081.

#### 3.1.2.6. Gateway Load Balancer -Target Groups

- EC2 instances
- IP Addresses - must be private IPs

### 3.1.3. Sticky Sessions (Session Affinity)

- It is possible to implement stickiness so that the same client is always redirected to the same instance behind a load balancer.
- This works for Classic Load Balancers & Application Load Balancers.
- The "cookie" used for stickiness has an expiration date you control.
- Use case: make sure the user doesn't lose his session data.
- Enabling stickiness may bring imbalance to the load over the backend EC2 instances.

#### 3.1.3.1. Sticky Sessions - Cookie Names

- Application-based Cookies:
  - Custom cookie:
    - Generated by the target.
    - Can include any custom attributes required by the application.
    - Cookie name must be specified individually for each target group.
    - Don't use AWSALB, AWSALBAPP, or AWSALBTG (reserved for use by the ELB).
  - Application cookie:
    - Generated by the load balancer.
    - Cookie name is AWSALBAPP.
- Duration-based Cookies:
  - Cookie generated by the load balancer.
  - Cookie name is AWSALB for ALB, AWSELB for CLB.

### 3.1.4. Cross-Zone Load Balancing

- Application Load Balancer:
  - Always on (can't be disabled).
  - No charges for inter AZ data.
- Network Load Balancer:
  - Disabled by default.
  - You pay charges ($) for inter AZ data if enabled.
- Classic Load Balancer:
  - Disabled by default.
  - No charges for inter AZ data if enabled.

### 3.1.5. SSL/TLS - Basics

- An SSL Certificate allows traffic between your clients and your load balancer to be encrypted in transit (in-flight encryption).
- **SSL** refers to Secure Sockets Layer, used to encrypt connections.
- **TLS** refers to Transport Layer Security, which is a newer version.
- Nowadays, **TLS certificates are mainly used**, but people still refer as SSL.
- Public SSL certificates are issued by Certificate Authorities (CA).
- Comodo, Symantec, GoDaddy, GlobalSign, Digicert, Letsencrypt, etc...
- SSL certificates have an expiration date (you set) and must be renewed.

#### 3.1.5.1. Load Balancer - SSL Certificates

- The load balancer uses an X.509 certificate (SSL/TLS server certificate).
- You can manage certificates using ACM (AWS Certificate Manager).
- You can create upload your own certificates alternatively.
- HTTPS listener:
  - You must specify a default certificate.
  - You can add an optional list of certs to support multiple domains.
  - Clients can use SNI (Server Name Indication) to specify the hostname they reach.
  - Ability to specify a security policy to support older versions of SSL / TLS (legacy clients).

### 3.1.6. SSL - Server Name Indication (SNI)

- SNI solves the problem of loading multiple SSL certificates onto one web server (to serve multiple websites).
- It's a "newer" protocol, and requires the client to indicate the hostname of the target server in the initial SSL handshake.
- The server will then find the correct certificate, or return the default one.

- Note:
  - Only works for ALB & NLB (newer generation), CloudFront.
  - Does not work for CLB (older gen).

### 3.1.7. Elastic Load Balancers - SSL Certificates

- Classic Load Balancer (v1):
  - Support only one SSL certificate.
  - Must use multiple CLB for multiple hostname with multiple SSL certificates.
- Application Load Balancer (v2):
  - Supports multiple listeners with multiple SSL certificates.
  - Uses Server Name Indication (SNI) to make it work.
- Network Load Balancer (v2):
  - Supports multiple listeners with multiple SSL certificates.
  - Uses Server Name Indication (SNI) to make it work.

### 3.1.8. Connection Draining

- Feature naming:
  - Connection Draining - for CLB.
  - Deregistration Delay - for ALB & NLB.
- Time to complete "in-flight requests" while the instance is de-registering or unhealthy.
- Stops sending new requests to the EC2 instance which is de-registering.
- Between 1 to 3600 seconds (default: 300 seconds).
- Can be disabled (set value to 0).
- Set to a low value if your requests are short.

### 3.1.9. What's an Auto Scaling Group?

- **Auto Scaling in EC2 allows you to have the right number of instances to handle the application load. Auto Scaling in DynamoDB automatically adjusts read and write throughput capacity, in response to dynamically changing request volumes, with zero downtime. These are both examples of horizontal scaling.**
- **For each Auto Scaling Group, there's a Cooldown Period after each scaling activity. In this period, the ASG doesn't launch or terminate EC2 instances. This gives time to metrics to stabilize. The default value for the Cooldown Period is 300 seconds (5 minutes).**
- In real-life, the load on your websites and application can change.
- In the cloud, you can create and get rid of servers very quickly.
- The goal of an Auto Scaling Group (ASG) is to:
  - Scale out (add EC2 instances) to match an increased load.
  - Scale in (remove EC2 instances) to match a decreased load.
  - Ensure we have a minimum and a maximum number of machines running.
  - Automatically register new instances to a load balancer.
  - Re-create an EC2 instance in case a previous one is terminated (ex: if unhealthy) (Replace unhealthy instances).
- Cost Savings: only run at an optimal capacity (principle of the cloud).
- ASG are free (you only pay for the underlying EC2 instances).

### 3.1.10. Auto Scaling Group Attributes

- A Launch Template (older "Launch Configurations" are deprecated):
  - AMI + Instance Type.
  - EC2 User Data.
  - EBS Volumes.
  - Security Groups.
  - SSH Key Pair.
  - IAM Roles for your EC2 Instances.
  - Network + Subnets Information.
  - Load Balancer Information.
- Min Size / Max Size / Initial Capacity.
- Scaling Policies.

### 3.1.11. Auto Scaling - CloudWatch Alarms & Scaling

- It is possible to scale an ASG based on CloudWatch alarms.
- An alarm monitors a metric (such as Average CPU, or a custom metric).
- Metrics such as Average CPU are computed for the overall ASG instances.
- Based on the alarm:
  - We can create scale-out policies (increase the number of instances).
  - We can create scale-in policies (decrease the number of instances).

### 3.1.12. Auto Scaling Groups - Dynamic Scaling Policies

- Target Tracking Scaling:
  - Most simple and easy to set-up.
  - Example: I want the average ASG CPU to stay at around 40%.
- Simple / Step Scaling:
  - When a CloudWatch alarm is triggered (example CPU > 70%), then add 2 units.
  - When a CloudWatch alarm is triggered (example CPU < 30%), then remove 1.
- Scheduled Actions:
  - Anticipate a scaling based on known usage patterns.
  - Example: increase the min capacity to 10 at 5 pm on Fridays.

### 3.1.13. Auto Scaling Groups - Predictive Scaling

- Predictive scaling: continuously forecast load and schedule scaling ahead.

### 3.1.14. Good metrics to scale on

- CPUUtilization: Average CPU utilization across your instances.
- RequestCountPerTarget: to make sure the number of requests per EC2 instances is stable.
- Average Network In / Out (if you're application is network bound).
- Any custom metric (that you push using CloudWatch).

### 3.1.15. Auto Scaling Groups - Scaling Cooldowns

- After a scaling activity happens, you are in the cooldown period (default 300 seconds).
- During the cooldown period, the ASG will not launch or terminate additional instances (to allow for metrics to stabilize).
- Advice: Use a ready-to-use AMI to reduce configuration time in order to be serving request fasters and reduce the cooldown period.

### 3.1.16. Auto Scaling Groups - Scaling Strategies (Resume)

- Manual Scaling: Update the size of an ASG manually
- Dynamic Scaling: Respond to changing demand
  - Simple / Step Scaling
    - When a CloudWatch alarm is triggered (example CPU > 70%), then add 2 units
    - When a CloudWatch alarm is triggered (example CPU < 30%), then remove 1
- Target Tracking Scaling
  - Example: I want the average ASG CPU to stay at around 40%
- Scheduled Scaling
  - Anticipate a scaling based on known usage patterns
  - Example: increase the min. capacity to 10 at 5 pm on Fridays
- Predictive Scaling
  - Uses Machine Learning to predict future traffic ahead of time.
  - Automatically provisions the right number of EC2 instances in advance.
  - Useful when your load has predictable time-based patterns.
