# Event-Announcement-System
A serverless event announcement system built with AWS services and managed entirely through Terraform. This project allows users to subscribe to event notifications via email and create new events that automatically notify all subscribers.

# Architecture

┌─────────────────────────────────────────────────────────────────┐
│                         End Users                                │
│                    (Web Browsers / Email)                        │
└────────────┬────────────────────────────────────┬───────────────┘
             │                                    │
             │ HTTPS                              │ Email
             ▼                                    ▼
    ┌─────────────────┐                  ┌──────────────┐
    │   CloudFront    │                  │  Amazon SNS  │
    │      (CDN)      │                  │   (Topic)    │
    └────────┬────────┘                  └──────▲───────┘
             │                                   │
             │ Origin                            │ Publish
             ▼                                   │
    ┌─────────────────┐                         │
    │   Amazon S3     │                         │
    │  (Static Site)  │                         │
    │  - index.html   │                         │
    │  - styles.css   │                         │
    │  - events.json  │◄────────────┐           │
    └─────────────────┘              │           │
                                     │ Update    │
             ┌───────────────────────┼───────────┤
             │                       │           │
             │ API Calls             │           │
             ▼                       │           │
    ┌─────────────────┐              │           │
    │  API Gateway    │              │           │
    │   (REST API)    │              │           │
    │  - /subscribe   │──────┐       │           │
    │  - /create-event│──┐   │       │           │
    └─────────────────┘  │   │       │           │
                         │   │       │           │
                         │   │       │           │
             ┌───────────┘   └───────┼───────────┘
             │                       │
             ▼                       ▼
    ┌──────────────────┐    ┌──────────────────┐
    │  Lambda Function │    │  Lambda Function │
    │   (Subscribe)    │    │  (Create Event)  │
    │                  │    │                  │
    │ - Subscribe user │    │ - Update S3      │
    │   to SNS topic   │    │ - Notify via SNS │
    └──────────────────┘    └──────────────────┘

    All managed by Terraform Infrastructure as Code

# Project Structure
.
├── terraform/
│   ├── main.tf                  # Main Terraform configuration
│   ├── variables.tf             # Terraform variables
│   ├── outputs.tf               # Terraform outputs
│   └── terraform.tfvars.example # Example variables file
├── lambda/
│   ├── subscribe.py            # Lambda function for subscriptions
│   └── create_event.py         # Lambda function for event creation
├── frontend/
│   ├── index.html              # Website frontend
│   ├── styles.css              # Website styling
│   └── events.json             # Event data storage
├── scripts/
│   └── update-frontend.sh      # Quick frontend updates
├── deploy.sh                   # Automated deployment script
└── README.md                   # Complete documentation
