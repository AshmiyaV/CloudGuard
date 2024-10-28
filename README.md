# CloudGuard
User Account Management System with Serverless Verification

## Description

This project is a **User Account Management System** built as a cloud-native web application that combines a set of RESTful APIs for account management with serverless infrastructure on Google Cloud. Users can create and manage accounts securely with token-based authentication. Upon creating an account, the system verifies the user’s email through an automated serverless function powered by Google Cloud Functions, Pub/Sub, and Cloud SQL, all managed with Terraform for scalable infrastructure setup.

## Features

- RESTful APIs for user account management
- Token-based authentication (Basic Auth)
- Email verification using a serverless function triggered by Google Cloud Pub/Sub
- Infrastructure as Code (IaC) with Terraform for automated deployment
- Continuous Integration (CI) using GitHub Actions to enforce code quality on pull requests

---

## Prerequisites

- **Java (JDK 17)**
- **Spring Boot** (version 3.2.2)
- **MySQL** database
- **Google Cloud SDK** (for GCP deployment)
- **Terraform** (for infrastructure setup)

---

## Sections

### 1. Web Application

This web application provides RESTful APIs for managing user accounts, including endpoints for creating, updating, and retrieving user details. The application uses token-based authentication to secure access.

#### Steps to Set Up and Run

1. **Clone the repository**: `git clone <repository_url>`
2. **Build the project**: Run `./mvnw install` (for Maven)
3. **Run the application**: Use `./mvnw spring-boot:run`

#### API Endpoints

1. **Create User**
   - **Endpoint**: `/v1/user`
   - **Method**: POST
   - **Request Payload**:
     ```json
     {
       "email": "user@example.com",
       "password": "your_password",
       "first_name": "John",
       "last_name": "Doe"
     }
     ```
   - **Response**: `200 OK`

2. **Update User**
   - **Endpoint**: `/v1/user/self`
   - **Method**: PUT
   - **Request Payload**: Fields that can be updated (firstName, lastName, password)
   - **Response**: `204 NO CONTENT`

3. **Get User**
   - **Endpoint**: `/v1/user/self`
   - **Method**: GET
   - **Response**: `200 OK`

   *Note*: Handle `400 Bad Request` and `401 Unauthorized` errors as required.

#### Authentication Requirements

- The user must provide a Basic Authentication token to access the authenticated endpoints.
- Only token-based authentication is supported; session-based authentication is not supported.

#### Continuous Integration (CI) with GitHub Actions

- A GitHub Actions workflow is included to perform code checks on each pull request.
- A branch protection rule is enforced to prevent merging a pull request if the workflow fails.

---

### 2. Serverless Email Verification with Google Cloud Functions

This module contains the infrastructure and code for serverless email verification using Google Cloud Functions, Pub/Sub, and Cloud SQL.

#### Steps for Setup

1. **Create & Setup GitHub Repository**
   - Create a new private repository named `serverless` and fork it for development.

2. **Cloud Function Implementation**
   - The Cloud Function is invoked by Pub/Sub when a new user account is created.
   - The function sends a verification email with a link that expires in 2 minutes. The function also logs email details in a Cloud SQL instance.

3. **Pub/Sub Setup**
   - Create a topic named `verify_email` with a 7-day retention period.
   - Set up a subscription for the Cloud Function.

4. **Web Application Updates**
   - Update the application to publish a message to the `verify_email` Pub/Sub topic with user information required for email verification.

5. **IAM Configuration**
   - Assign a non-admin IAM role that allows publishing messages to the Pub/Sub topic.

---

### 3. Infrastructure as Code (IaC) with Terraform

This section includes Terraform scripts for creating and managing GCP resources, including Cloud Functions, Pub/Sub topics, Cloud SQL, and IAM roles.

#### Steps for Local Setup

1. **Clone the repository**: `git clone <tf-gcp-infra-repo-url>`
2. **Add Google Cloud SDK to the environment variables path**
3. **Authenticate with GCP**: `gcloud auth login`

#### Terraform Commands

- **Initialize Provider**: `terraform init`
- **Generate Execution Plan**: `terraform plan`
- **Apply Configuration**: `terraform apply`
- **Destroy Resources**: `terraform destroy`
- **Format Configuration**: `terraform fmt`

#### Terraform Files

- **variables.tf**: Contains all configuration variables.
- **main.tf**: Defines the networking resources, including VPC, subnets, and routes.

#### Enabled APIs for GCP

- **Compute Engine**

---

This README combines all components for a comprehensive overview and setup instructions for the **User Account Management System with Serverless Verification**.
