# Event Announcement System

A serverless event announcement platform built with **AWS services** and managed entirely through **Terraform Infrastructure as Code (IaC)**.

The system allows users to subscribe to event notifications using their email addresses. Administrators can create new events through a web interface, automatically updating the event list and notifying all subscribed users through Amazon SNS.

## 🚀 Project Overview

This project demonstrates how to build an automated, serverless notification system using AWS cloud services.

**Key Features:**
- 📧 Email subscription through Amazon SNS.
- 📢 Automated email announcements when new events are created.
- 🌐 Static website hosting using Amazon S3.
- ⚡ Content delivery through Amazon CloudFront.
- 🔗 REST API integration using Amazon API Gateway.
- 🛠️ Automated backend processing using AWS Lambda.
- 🏗️ Infrastructure provisioning and management using Terraform.

## 🏗️ Architecture

The application uses a serverless architecture to handle user subscriptions, event creation, and email notifications.

### Architecture Diagram

```text
                    ┌─────────────────────┐
                    │      End Users      │
                    │  Web Browser/Email  │
                    └──────────┬──────────┘
                               │
                 ┌─────────────┴─────────────┐
                 │                           │
                 ▼                           ▼
          ┌──────────────┐            ┌──────────────┐
          │  CloudFront  │            │  Amazon SNS  │
          │     (CDN)    │            │    (Topic)   │
          └──────┬───────┘            └──────▲───────┘
                 │                           │
                 ▼                           │
          ┌──────────────┐                   │
          │  Amazon S3   │                   │
          │ Static Website│                  │
          └──────────────┘                   │
                                             │
                 ┌───────────────────────────┤
                 │                           │
                 ▼                           │
          ┌──────────────┐                   │
          │ API Gateway  │                   │
          │   REST API   │                   │
          └──────┬───────┘                   │
                 │                           │
          ┌──────┴──────────┐                │
          │                 │                │
          ▼                 ▼                │
   ┌──────────────┐  ┌──────────────┐        │
   │   Lambda     │  │    Lambda    │        │
   │  Subscribe   │  │ Create Event │────────┘
   └──────┬───────┘  └──────┬───────┘
          │                 │
          ▼                 ▼
   ┌──────────────┐  ┌──────────────┐
   │ Subscribe    │  │ Update Event │
   │ Email to SNS │  │ Data in S3   │
   └──────────────┘  └──────────────┘

       Infrastructure Managed by Terraform
```

### AWS Services Used

| AWS Service | Purpose |
|---|---|
| Amazon S3 | Hosts the static website and stores event data |
| Amazon CloudFront | Delivers website content through a CDN |
| Amazon API Gateway | Provides REST API endpoints |
| AWS Lambda | Handles subscriptions and event creation |
| Amazon SNS | Manages email subscriptions and event notifications |
| AWS IAM | Controls permissions between AWS services |

## 🔄 Application Workflow

### 1. User Subscription
- Users visit the website through Amazon CloudFront.
- Users enter their email addresses to subscribe.
- API Gateway receives the subscription request.
- The Subscribe Lambda function registers the email address with the Amazon SNS topic.
- Users confirm their email subscription through the SNS confirmation email.

### 2. Event Creation
- An administrator creates a new event through the website.
- API Gateway forwards the request to the Create Event Lambda function.
- Lambda updates the `events.json` file stored in Amazon S3.
- Lambda publishes an announcement message to the Amazon SNS topic.
- Amazon SNS distributes the notification to confirmed subscribers.

### 3. Website Updates
- The frontend retrieves event information from the S3-hosted website.
- Newly created events are reflected in the event listing.
- CloudFront delivers the static website content to users.

## 📁 Project Structure

```text
Event-Announcement-System/
│
├── terraform/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   └── terraform.tfvars.example
│
├── lambda/
│   ├── subscribe.py
│   └── create_event.py
│
├── frontend/
│   ├── index.html
│   ├── styles.css
│   └── events.json
│
├── scripts/
│   └── update-frontend.sh
│
├── deploy.sh
│
└── README.md
```

## 🛠️ Technologies

- **Cloud Platform:** Amazon Web Services (AWS)
- **Infrastructure as Code:** Terraform
- **Backend:** Python / AWS Lambda
- **Frontend:** HTML, CSS, JavaScript
- **API:** Amazon API Gateway (REST API)
- **Notifications:** Amazon SNS
- **Storage:** Amazon S3
- **Content Delivery:** Amazon CloudFront

## ⚙️ Deployment

### Prerequisites

Before deploying the project, ensure you have:

- An AWS account
- AWS CLI installed and configured
- Terraform installed
- Python installed
- Appropriate AWS IAM permissions

### Step 1: Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/Event-Announcement-System.git

cd Event-Announcement-System
```

### Step 2: Configure Terraform Variables

Navigate to the Terraform directory and create your variables file.

```bash
cd terraform

cp terraform.tfvars.example terraform.tfvars
```

Update the variables according to your AWS environment.

### Step 3: Initialize Terraform

```bash
terraform init
```

### Step 4: Review Infrastructure Changes

```bash
terraform plan
```

### Step 5: Deploy AWS Infrastructure

```bash
terraform apply
```

Review and approve the deployment when prompted.

### Step 6: Deploy the Frontend

Follow the deployment script instructions:

```bash
cd ..

chmod +x deploy.sh

./deploy.sh
```

Once deployment is complete, access the website using the CloudFront URL generated by Terraform.

## 🔐 Security Considerations

- Use IAM roles with least-privilege permissions for Lambda functions.
- Restrict access to administrative event creation endpoints.
- Keep AWS credentials outside the source code.
- Avoid committing `terraform.tfvars` and sensitive configuration files to GitHub.
- Use HTTPS through Amazon CloudFront.
- Configure appropriate API authorization and validation.

## 💡 Key Learning Outcomes

Through this project, I gained practical experience in:

- Designing serverless architectures on AWS.
- Automating infrastructure provisioning with Terraform.
- Integrating AWS Lambda with API Gateway.
- Implementing email subscriptions and notifications using Amazon SNS.
- Hosting static websites using Amazon S3 and CloudFront.
- Managing AWS infrastructure through Infrastructure as Code (IaC).

## 📌 Future Improvements

- Implement administrator authentication using Amazon Cognito.
- Add DynamoDB for structured event management.
- Introduce Amazon EventBridge for scheduled event announcements.
- Implement CI/CD deployment using GitHub Actions.

## 👨‍💻 Author

**Bryan Lim**

Cloud & Infrastructure Projects | AWS | Terraform | Networking | Cloud Security

