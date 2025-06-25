Project Overview

This project establishes the backend infrastructure for a pet shelter website, enabling efficient management of pet listings, adoption applications, and user interactions. The architecture leverages AWS Amplify for frontend hosting and a comprehensive suite of AWS serverless services for the backend, defined entirely through a SAM template.

Core Technologies Utilized
- AWS Serverless Application Model (SAM): Defines the entire serverless application, ensuring infrastructure as code (IaC) principles.
- AWS Lambda: Event-driven, serverless compute functions handling all backend logic.
- Amazon DynamoDB: Fully managed NoSQL databases for highly available and scalable data storage.
- Amazon API Gateway: Creates secure, scalable, and performant RESTful APIs for frontend interaction.
- Amazon Cognito: Manages user authentication and authorization, providing secure access to protected API endpoints.
- AWS Step Functions: Orchestrates complex, multi-step workflows for automated tasks like report generation.
- Amazon SNS (Simple Notification Service): Facilitates sending notifications, specifically for automated email reports.

Backend Architecture Highlights
1. Data Persistence (DynamoDB)
The backend features three core DynamoDB tables, each designed for specific data entities:

PetsTable: Stores comprehensive details about available pets (e.g., name, breed, age, status).

AdoptionsTable: Manages records of adoption applications, including applicant details and application status.

PetsInterestTable: A new addition to track user interest in specific pets, enabling analytics and reporting on pet popularity.

2. RESTful API Endpoints (API Gateway & Lambda)
A central API Gateway (PetsAPI) exposes various endpoints, secured with CORS for cross-origin requests and integrated with Lambda functions for backend logic:

GET /pets: Retrieves a list of all available pets.

GET /adoptions: Fetches all adoption records (authenticated access).

GET /adoptions/{id}: Retrieves a specific adoption record by ID (authenticated access).

POST /adoptions: Allows users to submit new adoption applications.
Each endpoint is backed by a dedicated Python 3.10 Lambda function (e.g., GetPetsLambda, GetAdoptionsLambda, CreateAdoptionLambda), demonstrating a microservices approach.

3. User Authentication and Authorization (Cognito)
AWS Cognito is integrated to provide robust user management:

CognitoUserPool: Configured to manage user registration, sign-in, and account recovery with email as the primary username attribute and strong password policies.

CognitoUserPoolClient: Enables client applications (e.g., the Amplify-hosted frontend) to interact with the user pool, supporting OAuth flows and specific scopes.

CognitoUserPoolDomain: Provides a hosted UI for user sign-up/sign-in, integrated with the API Gateway for authorizing protected routes (e.g., /adoptions and /adoptions/{id}).

4. Automated Reporting Workflow (Step Functions & SNS)
A sophisticated automated reporting system is implemented using AWS Step Functions:

GenerateReportDataLambda: Gathers data on pet interest and adoption trends from PetsTable and PetsInterestTable.

GenerateHTMLLambda: Transforms the gathered data into a formatted HTML report.

GeneratePresignedUrlLambda: Creates a secure, temporary pre-signed URL for accessing the generated HTML report (presumably stored in S3, though the S3 bucket is not explicitly defined in this snippet, it's a typical pattern).

StateMachine: Orchestrates these Lambda functions in a sequential workflow, ensuring the report generation process is reliable and automated.

TriggerSNS: Publishes the pre-signed URL to an SNS topic (EmailReport), which then sends an email notification to a specified address, providing stakeholders with direct access to the report.

CreateReportLambda: Acts as an API trigger to initiate the Step Functions state machine, allowing users to request a report generation on demand.
