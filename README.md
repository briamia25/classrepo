Pet Shelter Application:
This project implements the backend infrastructure for a pet shelter website, supporting the seamless management of pet listings, adoption applications, and user engagement. The architecture leverages AWS Amplify for frontend hosting and a robust suite of AWS serverless services for the backend, all defined using AWS Serverless Application Model (SAM) template. 

Core Technologies
AWS Serverless Application Model (SAM) – Defines the serverless infrastructure as code for consistent and repeatable deployments.

AWS Lambda – Handles backend logic using event-driven serverless compute functions.

Amazon DynamoDB – Provides highly available, scalable NoSQL data storage.

Amazon API Gateway – Powers secure, scalable RESTful APIs for frontend-backend integration.

Amazon Cognito – Manages user authentication and authorization to protect API endpoints.

AWS Step Functions – Orchestrates complex workflows like automated report generation.

Amazon SNS (Simple Notification Service) – Delivers notifications such as email reports.

Backend Architecture
Data Persistence (DynamoDB)
The backend uses three dedicated DynamoDB tables:

PetsTable – Stores details of pets (e.g., name, breed, age, status).

AdoptionsTable – Records adoption applications, including applicant info and status.

PetsInterestTable – Tracks user interest in specific pets for analytics and reporting.

RESTful API (API Gateway + Lambda)
A centralized API Gateway (PetsAPI) exposes secure, CORS-enabled endpoints, each backed by Lambda functions:

GET /pets – Retrieve all available pets.

GET /adoptions – Fetch all adoption records.

GET /adoptions/{id} – Retrieve a specific adoption record (requires authentication).

POST /adoptions – Submit a new adoption application.

User Authentication (Cognito)
Integrated AWS Cognito handles user management and API access control:

User Pool – Supports registration, login, and account recovery with email as the primary ID and strong password policies.

User Pool Client – Connects frontend apps via OAuth flows and scopes.

User Pool Domain – Provides a hosted UI for sign-up/sign-in and protects routes like /adoptions/{id}.

Automated Reporting (Step Functions + SNS)
A fully automated reporting workflow generates adoption and interest reports:

GenerateReportDataLambda – Aggregates data from PetsTable and PetsInterestTable.

GenerateHTMLLambda – Converts data into a formatted HTML report.

GeneratePresignedUrlLambda – Produces a secure, temporary URL to access the report.

State Machine – Coordinates the reporting steps in sequence for reliability.

TriggerSNS – Sends the report URL via email through an SNS topic (EmailReport).

CreateReportLambda – API-triggered Lambda to start the report generation workflow on demand.



