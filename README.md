
# 🐾 Pet Shelter Application

### Summary
This project delivers a robust **serverless backend infrastructure** designed for a pet shelter website. It enables the efficient management of pet listings, adoption applications, and crucial user engagement insights. Built entirely on **AWS Amplify** and leveraging a comprehensive suite of **AWS serverless services**, the entire infrastructure is precisely defined using **AWS SAM (Serverless Application Model)** for declarative infrastructure as code.

---

## 🏛️ Architecture Overview

The application is structured around a scalable, event-driven serverless architecture, ensuring high high availability and cost-effectiveness.

![architecture](arch-diagram-week-6.png)

### Core Data Storage (Amazon DynamoDB)

* **`PetsTable`**: Stores comprehensive details for each pet (name, breed, age, status, etc.).
* **`AdoptionsTable`**: Tracks all adoption applications (applicant info, status).
* **`PetsInterestTable`**: Captures user interest and interactions for analytics and reporting purposes.

### API Endpoints (Amazon API Gateway + AWS Lambda)

All frontend-backend interactions are facilitated via a secure RESTful API, powered by API Gateway and AWS Lambda functions.

| Method | Path         | Description              | Access        |
| :----- | :----------- | :----------------------- | :------------ |
| `GET`  | `/pets`      | Get list of available pets | `Public`      |
| `GET`  | `/adoptions` | Get all adoption records | `Authenticated` |
| `GET`  | `/adoptions/{id}` | Get specific adoption record by ID | `Authenticated` |
| `POST` | `/adoptions` | Submit new adoption application | `Public`      |

---

## ✨ Key Features

* ✅ **Efficient Management:** Seamlessly manage pet listings and adoption processes.
* ✅ **Secure Access:** Robust user authentication and authorization via AWS Cognito.
* ✅ **Automated Insights:** Generate and deliver reports automatically via email.
* ✅ **Scalable Design:** Built on an event-driven serverless architecture for high scalability and reliability.

---

## 🚀 Tech Stack

This application utilizes a powerful combination of AWS serverless services for its backend operations:

| Service                | Purpose                                                                 |
| :--------------------- | :---------------------------------------------------------------------- |
| **AWS SAM** | Infrastructure as Code (IaC) for defining the entire serverless application. |
| **AWS Lambda** | Serverless compute for all backend business logic and event processing. |
| **Amazon DynamoDB** | High-performance NoSQL data storage for pets, adoptions, and user interest records. |
| **Amazon API Gateway** | Creates RESTful API endpoints for secure and efficient frontend-backend communication. |
| **Amazon Cognito** | Handles all aspects of user authentication and authorization, including user pools. |
| **AWS Step Functions** | Orchestrates complex, multi-step workflows for automated report generation. |
| **Amazon SNS** | Manages notification delivery, primarily for sending automated email reports to stakeholders. |

---

## 🔐 User Authentication (Amazon Cognito)

* **User Pool**: Manages user signup, login, and account recovery processes with email-based login and strong password policies.
* **User Pool Client**: Configured to enable OAuth flows, facilitating seamless integration with the frontend application.
* **User Pool Domain**: Provides a hosted UI for user authentication, securely linked with protected API routes.

---

## 📊 Automated Reporting Workflow (AWS Step Functions + Amazon SNS)

A sophisticated workflow orchestrates the generation and delivery of insightful reports on pet interest and adoption trends.

1.  **`GenerateReportDataLambda`**: Gathers and aggregates raw data on pet interest and adoption trends.
2.  **`GenerateHTMLLambda`**: Formats the processed data into a user-friendly HTML report.
3.  **`GeneratePresignedUrlLambda`**: Creates a temporary, secure URL for accessing the generated report, ensuring controlled access.
4.  **State Machine (AWS Step Functions)**: Orchestrates the entire multi-step workflow, ensuring each step executes in the correct sequence.
5.  **`TriggerSNS`**: Publishes the generated report's URL to an Amazon SNS topic.
6.  **Email Delivery**: Amazon SNS then sends automated email notifications containing the report URL to relevant stakeholders.
7.  **`CreateReportLambda`**: An API-triggered Lambda function that initiates this entire report generation workflow on demand.
