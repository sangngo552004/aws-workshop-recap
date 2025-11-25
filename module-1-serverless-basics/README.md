# Module 1: Serverless Microservice Foundations

> **Focus:** Building a synchronous RESTful API on top of an Event-Driven Serverless architecture using AWS Lambda, API Gateway, and DynamoDB.

## 🔗 Original Workshop Source
This module is based on the [Serverless patterns AWS Workshop](https://catalog.workshops.aws/serverless-patterns/en-US/module1)..
*Original content © Amazon Web Services, Inc. or its affiliates.*

---

## 🏛️ Architecture Analysis

### The "Sync-over-Async" Pattern
Although this module delivers a standard **Synchronous Request/Response** experience to the end-user (REST API), the underlying infrastructure operates on a purely **Event-Driven Architecture (EDA)** mechanism.

* **Protocol Transformation:** API Gateway acts as the entry point, converting raw HTTP Requests into **JSON Event Objects**.
* **Decoupled Communication:** AWS Lambda does not "listen" to a port like traditional servers. It is strictly triggered by events pushed from the Gateway.

![Architecture Diagram](https://static.us-east-1.prod.workshops.aws/public/6fe885da-bf19-4578-9c35-456ee183a803/static/module1/traditional_webdev_complex.png)

## 🔑 Key Technical Concepts

### 1. Compute: Lambda Lifecycle & Cold Starts
Understanding the **Execution Environment** lifecycle is crucial for performance:
* **Cold Start:** When the function is first triggered, AWS provisions a container and initializes the runtime. This introduces latency.
* **Warm Start:** Subsequent requests reuse the frozen container.
* **Optimization:** Database connections are initialized *outside* the handler function to leverage the warm execution context for future invocations.

### 2. Security: IAM Execution Role
Implemented **Least Privilege Principle** by creating a specific Execution Role.
* The Lambda function is granted permission *only* to `PutItem` on the specific DynamoDB table.
* This differs from **Resource-based Policies**, which control *who* (API Gateway) can invoke the function.

### 3. Data Persistence
* Utilized **Amazon DynamoDB** (Serverless NoSQL) for millisecond-latency data access.
* Schema design focuses on key-value access patterns suitable for high-scale microservices.

## 🛠️ Implementation Summary

In this module, I performed the following engineering tasks:

1.  **Database Provisioning:** Created a DynamoDB table with defined Partition Keys.
2.  **Function Development:** Developed a Node.js/Python Lambda function to process incoming JSON events and persist data.
3.  **IAM Configuration:** manually attached IAM Policies to the Lambda Execution Role to authorize DB write access.
4.  **API Deployment:** Configured Amazon API Gateway (REST API type), defined Resources/Methods, and deployed the Stage to a public endpoint.
5.  **Integration Testing:** Verified the end-to-end data flow from HTTP Client → API Gateway → Lambda → DynamoDB.

## 📝 Key Takeaway
> "Serverless is not just about running code without servers; it is about handling **events**. Even a simple API call is an event transformation process, requiring a shift in mindset from connection-based to event-based programming."
