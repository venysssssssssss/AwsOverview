# AWS X-Ray<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Visual analysis of our applications](#2-visual-analysis-of-our-applications)
- [3. Advantages](#3-advantages)
- [4. Compatibility](#4-compatibility)
- [5. Leverages Tracing](#5-leverages-tracing)
- [6. How to enable it?](#6-how-to-enable-it)
  - [6.1. Your code](#61-your-code)
- [7. X-Ray magic](#7-x-ray-magic)
- [8. Troubleshooting](#8-troubleshooting)
- [9. X-Ray Instrumentation in your code](#9-x-ray-instrumentation-in-your-code)

# 1. Introduction

- Debugging in Production, the good old way:
  - Test locally.
  - Add log statements everywhere.
  - Re-deploy in production.
- Log formats differ across applications using CloudWatch and analytics is hard.
- Debugging: monolith "easy", distributed services "hard".
- No common views of your entire architecture!

# 2. Visual analysis of our applications

![AWS X-Ray Service Map](/Images/AWSXRayServiceMap.PNG)

# 3. Advantages

- Troubleshooting performance (bottlenecks).
- Understand dependencies in a microservice architecture.
- Pinpoint service issues.
- Review request behavior.
- Find errors and exceptions.
- Are we meeting time SLA?
- Where I am throttled?
- Identify users that are impacted.

# 4. Compatibility

- AWS Lambda.
- Elastic Beanstalk.
- ECS.
- ELB.
- API Gateway.
- EC2 Instances or any application server (even on premise).

# 5. Leverages Tracing

- Tracing is an end to end way to following a "request".
- Each component dealing with the request adds its own "trace".
- Tracing is made of segments (+ sub segments).
- Annotations can be added to traces to provide extra-information
- Ability to trace:
  - Every request.
  - Sample request (as a % for example or a rate per minute).
- X-Ray Security:
  - IAM for authorization.
  - KMS for encryption at rest.

# 6. How to enable it?

## 6.1. Your code

1. Your code (Java, Python, Go, Node.js, .NET) must import the AWS X-Ray SDK:

- Very little code modification needed.
- The application SDK will then capture:
  - Calls to AWS services.
  - HTTP / HTTPS requests.
  - Database Calls (MySQL, PostgreSQL, DynamoDB).
  - Queue calls (SQS).

2. Install the X-Ray daemon or enable X-Ray AWS Integration:

- X-Ray daemon works as a low level UDP packet interceptor (Linux / Windows / Mac...)
- AWS Lambda / other AWS services already run the X-Ray daemon for you
- Each application must have the IAM rights to write data to X-Ray

# 7. X-Ray magic

- X-Ray service collects data from all the different services
- Service map is computed from all the segments and traces
- X-Ray is graphical, so even non technical people can help troubleshoot

# 8. Troubleshooting

- If X-Ray is not working on EC2:
  - Ensure the EC2 IAM Role has the proper permissions.
  - Ensure the EC2 instance is running the X-Ray Daemon.
- To enable on AWS Lambda:
  - Ensure it has an IAM execution role with proper policy (AWSX-RayWriteOnlyAccess).
  - Ensure that X-Ray is imported in the code.

# 9. X-Ray Instrumentation in your code

- Instrumentation means the measure of product's performance, diagnose errors, and to write trace information.
- To instrument your application code, you use the X-Ray SDK.
- Many SDK require only configuration changes.
- You can modify your application code to customize and annotation the data that the SDK sends to X- Ray, using interceptors, filters, handlers, middleware.
