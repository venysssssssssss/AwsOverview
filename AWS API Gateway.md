# AWS API Gateway<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Integrations High Level](#2-integrations-high-level)
- [3. AWS Service Integration Kinesis Data Streams example](#3-aws-service-integration-kinesis-data-streams-example)
- [4. API Gateway - Endpoint Types](#4-api-gateway---endpoint-types)
- [5. API Gateway – Security](#5-api-gateway--security)
- [6. API Gateway – Deployment Stages](#6-api-gateway--deployment-stages)
  - [6.1. API Gateway Stages v1 and v2 API breaking change](#61-api-gateway-stages-v1-and-v2-api-breaking-change)
- [7. API Gateway – Stage Variables](#7-api-gateway--stage-variables)
  - [7.1. API Gateway Stage Variables \& Lambda Aliases](#71-api-gateway-stage-variables--lambda-aliases)
- [8. API Gateway – Canary Deployment](#8-api-gateway--canary-deployment)
- [9. API Gateway - Integration Types (Methods)](#9-api-gateway---integration-types-methods)
- [10. Mapping Templates (AWS \& HTTP Integration)](#10-mapping-templates-aws--http-integration)
  - [10.1. Mapping Example: JSON to XML with SOAP](#101-mapping-example-json-to-xml-with-soap)
  - [10.2. Mapping Example: Query String parameters](#102-mapping-example-query-string-parameters)
- [11. AWS API Gateway Swagger / Open API spec](#11-aws-api-gateway-swagger--open-api-spec)
- [12. Caching API responses](#12-caching-api-responses)
  - [12.1. API Gateway Cache Invalidation](#121-api-gateway-cache-invalidation)
- [13. API Gateway – Usage Plans \& API Keys](#13-api-gateway--usage-plans--api-keys)
  - [13.1. API Gateway – Correct Order for API keys](#131-api-gateway--correct-order-for-api-keys)
- [14. API Gateway – Logging \& Tracing](#14-api-gateway--logging--tracing)
  - [14.1. API Gateway – CloudWatch Metrics](#141-api-gateway--cloudwatch-metrics)
- [15. API Gateway Throttling](#15-api-gateway-throttling)
- [16. API Gateway - Errors](#16-api-gateway---errors)
- [17. CORS](#17-cors)

# 1. Introduction

- AWS Lambda + API Gateway: No infrastructure to manage.
- Support for the WebSocket Protocol.
- Handle API versioning (v1, v2...).
- Handle different environments (dev, test, prod...).
- Handle security (Authentication and Authorization).
- Create API keys, handle request throttling.
- Swagger / Open API import to quickly define APIs.
- Transform and validate requests and responses.
- Generate SDK and API specifications.
- Cache API responses.

# 2. Integrations High Level

- Lambda Function:
  - Invoke Lambda function.
  - Easy way to expose REST API backed by AWS Lambda.
- HTTP:
  - Expose HTTP endpoints in the backend.
  - Example: internal HTTP API on premise, Application Load Balancer...
  - Why? Add rate limiting, caching, user authentications, API keys, etc...
- AWS Service
  - Expose any AWS API through the API Gateway.
  - Example: start an AWS Step Function workflow, post a message to SQS.
  - Why? Add authentication, deploy publicly, rate control...

# 3. AWS Service Integration Kinesis Data Streams example

# 4. API Gateway - Endpoint Types

- Edge-Optimized (default): For global clients
  - Requests are routed through the CloudFront Edge locations (improves latency).
  - The API Gateway still lives in only one region.
- Regional:
  - For clients within the same region.
  - Could manually combine with CloudFront (more control over the caching strategies and the distribution).
- Private:
  - Can only be accessed from your VPC using an interface VPC endpoint (ENI).
  - Use a resource policy to define access.

# 5. API Gateway – Security

- **User Authentication through:**
  - IAM Roles (useful for internal applications).
  - Cognito (identity for external users – example mobile users).
  - Custom Authorizer (your own logic).
- **Custom Domain Name HTTPS** security through integration with AWS Certificate Manager (ACM):
  - If using Edge-Optimized endpoint, then the certificate must be in us-east-1.
  - If using Regional endpoint, the certificate must be in the API Gateway region.
  - Must setup CNAME or A-alias record in Route 53.

# 6. API Gateway – Deployment Stages

- Making changes in the API Gateway does not mean they're effective.
- You need to make a "deployment" for them to be in effect.
  - It's a common source of confusion.
- Changes are deployed to "Stages" (as many as you want).
- Use the naming you like for stages (dev, test, prod).
- Each stage has its own configuration parameters.
- Stages can be rolled back as a history of deployments is kept.

## 6.1. API Gateway Stages v1 and v2 API breaking change

# 7. API Gateway – Stage Variables

- Stage variables are like environment variables for API Gateway.
- Use them to change often changing configuration values.
- They can be used in:
  - Lambda function ARN.
  - HTTP Endpoint.
  - Parameter mapping templates.
- Use cases:
  - Configure HTTP endpoints your stages talk to (dev, test, prod...).
  - Pass configuration parameters to AWS Lambda through mapping templates.
- Stage variables are passed to the "context" object in AWS Lambda.

## 7.1. API Gateway Stage Variables & Lambda Aliases

- We create a stage variable to indicate the corresponding Lambda alias.
- Our API gateway will automatically invoke the right Lambda function!

# 8. API Gateway – Canary Deployment

- Possibility to enable canary deployments for any stage (usually prod).
- Choose the % of traffic the canary channel receives.
- Metrics & Logs are separate (for better monitoring).
- Possibility to override stage variables for canary.
- This is blue / green deployment with AWS Lambda & API Gateway.

# 9. API Gateway - Integration Types (Methods)

- Integration Type:
  - **MOCK:**
    - API Gateway returns a response without sending the request to the backend.
  - **HTTP / AWS (Lambda & AWS Services):**
    - You must configure both the integration request and integration response.
    - Setup data mapping using mapping templates for the request & response.
  - **AWS_PROXY (Lambda Proxy):**
    - Incoming request from the client is the input to Lambda.
    - The function is responsible for the logic of request / response.
    - No mapping template, headers, query string parameters... are passed as arguments.
  - **HTTP_PROXY**
    - No mapping template.
    - The HTTP request is passed to the backend.
    - The HTTP response from the backend is forwarded by API Gateway.

# 10. Mapping Templates (AWS & HTTP Integration)

- Mapping templates can be used to modify request / responses.
- Rename / Modify query string parameters.
- Modify body content.
- Add headers.
- Uses Velocity Template Language (VTL): for loop, if etc...
- Filter output results (remove unnecessary data).

## 10.1. Mapping Example: JSON to XML with SOAP

- SOAP API are XML based, whereas REST API are JSON based.
- In this case, API Gateway should:
  - Extract data from the request: either path, payload or header.
  - Build SOAP message based on request data (mapping template).
  - Call SOAP service and receive XML response.
  - Transform XML response to desired format (like JSON), and respond to the user.

## 10.2. Mapping Example: Query String parameters

# 11. AWS API Gateway Swagger / Open API spec

- Common way of defining REST APIs, using API definition as code.
- Import existing Swagger / OpenAPI 3.0 spec to API Gateway:
  - Method.
  - Method Request.
  - Integration Request.
  - Method Response.
  - AWS extensions for API gateway and setup every single option.
- Can export current API as Swagger / OpenAPI spec.
- Swagger can be written in YAML or JSON.
- Using Swagger we can generate SDK for our applications.

# 12. Caching API responses

- Caching reduces the number of calls made to the backend.
- Default TTL (time to live) is 300 seconds (min: 0s, max: 3600s).
- **Caches are defined per stage.**
  - Possible to override cache settings **per method**.
- Cache encryption option.
- Cache capacity between 0.5GB to 237GB.
- Cache is expensive, makes sense in production, may not make sense in dev / test.

## 12.1. API Gateway Cache Invalidation

- Able to flush the entire cache (invalidate it) immediately.
- Clients can invalidate the cache with header: Cache-Control: max-age=0 (with proper IAM authorization).
- If you don't impose an InvalidateCache policy (or choose the Require authorization check box in the console), any client can invalidate the API cache.

# 13. API Gateway – Usage Plans & API Keys

- If you want to make an API available as an offering ($) to your customers.
- Usage Plan:
  - Who can access one or more deployed API stages and methods.
  - How much and how fast they can access them.
  - Uses API keys to identify API clients and meter access.
  - Configure throttling limits and quota limits that are enforced on individual client.
- API Keys:
  - alphanumeric string values to distribute to your customers
  - Ex: WBjHxNtoAb4WPKBC7cGm64CBibIb24b4jt8jJHo9
  - Can use with usage plans to control access
  - Throttling limits are applied to the API keys
  - Quotas limits is the overall number of maximum requests

## 13.1. API Gateway – Correct Order for API keys

- To configure a usage plan:

1. Create one or more APIs, configure the methods to require an API key, and deploy the APIs to stages.
2. Generate or import API keys to distribute to application developers (your customers) who will be using your API.
3. Create the usage plan with the desired throttle and quota limits.
4. Associate API stages and API keys with the usage plan.

- Callers of the API must supply an assigned API key in the x-api-key header in requests to the API.

# 14. API Gateway – Logging & Tracing

- CloudWatch Logs:
  - Enable CloudWatch logging at the Stage level (with Log Level).
  - Can override settings on a per API basis (ex: ERROR, DEBUG, INFO).
  - Log contains information about request / response body.
- X-Ray:
  - Enable tracing to get extra information about requests in API Gateway.
  - X-Ray API Gateway + AWS Lambda gives you the full picture.

## 14.1. API Gateway – CloudWatch Metrics

- Metrics are by stage, Possibility to enable detailed metrics.
- **CacheHitCount & CacheMissCount:** efficiency of the cache.
- **Count:** The total number API requests in a given period.
- **IntegrationLatency:** The time between when API Gateway relays a request to the backend and when it receives a response from the backend.
- **Latency:** The time between when API Gateway receives a request from a client and when it returns a response to the client. The latency includes the integration latency and other API Gateway overhead.
- **4XXError** (client-side) & **5XXError** (server-side).

# 15. API Gateway Throttling

- Account Limit:
  - API Gateway throttles requests at10000 rps across all API.
  - Soft limit that can be increased upon request.
- In case of throttling => **429 Too Many Requests** (retriable error).
- Can set Stage limit & Method limits to improve performance.
- Or you can define Usage Plans to throttle per customer.
- Just like Lambda Concurrency, one API that is overloaded, if not limited, can cause the other APIs to be throttled.

# 16. API Gateway - Errors

- 4xx means Client errors:
  - 400: Bad Request.
  - 403: Access Denied, WAF filtered.
  - 429: Quota exceeded, Throttle.
- 5xx means Server errors:
  - 502: Bad Gateway Exception, usually for an incompatible output returned from a Lambda proxy integration backend and occasionally for out-of-order invocations due to heavy loads.
  - 503: Service Unavailable Exception.
  - 504: Integration Failure – ex Endpoint Request Timed-out Exception API Gateway requests time out after 29 second maximum.

# 17. CORS

- CORS must be enabled when you receive API calls from another domain.
- The OPTIONS pre-flight request must contain the following headers:
  - Access-Control-Allow-Methods
  - Access-Control-Allow-Headers
  - Access-Control-Allow-Origin
- CORS can be enabled through the console.
