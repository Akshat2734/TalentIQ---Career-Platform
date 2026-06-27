# 🚀 TalentIQ --- Distributed Career & Recruitment Platform

> Enterprise-grade microservices recruitment ecosystem built with
> **Next.js, Express.js, PostgreSQL, Kafka, Redis, Razorpay, Docker, and
> Nginx**.

![Next.js](https://img.shields.io/badge/Next.js-16-black?logo=nextdotjs)
![React](https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=black)
![Node.js](https://img.shields.io/badge/Node.js-Backend-339933?logo=nodedotjs&logoColor=white)
![Express](https://img.shields.io/badge/Express-Microservices-black?logo=express)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Neon-4169E1?logo=postgresql&logoColor=white)
![Kafka](https://img.shields.io/badge/Apache_Kafka-Event_Driven-231F20?logo=apachekafka)
![Docker](https://img.shields.io/badge/Docker-Containerized-2496ED?logo=docker&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-Ready-326CE5?logo=kubernetes&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-Cache-DC382D?logo=redis&logoColor=white)
![Razorpay](https://img.shields.io/badge/Razorpay-Payments-0C2451)

------------------------------------------------------------------------

# 📖 Overview

TalentIQ is a distributed career and recruitment platform composed of
independently deployable microservices. It provides AI-powered ATS
resume analysis, premium subscriptions, job application management,
asynchronous email delivery through Kafka, and dynamic user profile
management.

------------------------------------------------------------------------

## 🌟 Core Features
* **AI-Powered ATS Resume Analysis:** Users can upload their resumes in PDF format to receive instant, granular ATS compatibility scores, detailed category breakdowns, and actionable feedback on strengths and weaknesses.
* **Premium Subscriptions:** Integrated with Razorpay, allowing users to purchase 30-day premium access using secure HMAC SHA-256 signature verification.
* **Dynamic Skill Graphing:** Employs a many-to-many database relationship handling to allow users to dynamically add, view, and delete professional skills connected to their profiles.
* **Job Application Engine:** Seamlessly connects job seekers to active listings, tracking application states, mapping resumes, and verifying active subscription tiers during the application process.

## 🚀 Scalability & Backend Features
* **Microservices Architecture:** The backend is modularized into independently deployable nodes (`auth`, `job`, `payment`, `user`, and `utils`), ensuring isolated failure domains and targeted horizontal scaling.
* **Event-Driven Messaging:** Utilizes Apache Kafka (hosted via Aiven) to decouple heavy operations. The `send-mail` topic enables non-blocking SMTP email dispatching through background consumer workers.
* **Serverless Database:** Backed by Neon Database's serverless PostgreSQL infrastructure, providing instant connection pooling and auto-scaling capabilities tailored for stateless environments.
* **Atomic Database Transactions:** Multi-step insertions (like checking if a user exists, creating a skill, and linking the user to the skill) are wrapped in `BEGIN`, `COMMIT`, and `ROLLBACK` SQL transactions to guarantee data integrity.

# 🏗 High-Level Architecture

``` mermaid
graph TD
Client[Next.js Frontend] --> Gateway[API Gateway]

Gateway --> Auth[Auth Service]
Gateway --> User[User Service]
Gateway --> Job[Job Service]
Gateway --> Payment[Payment Service]
Gateway --> Utils[Utils Service]

Auth --> DB[(Neon PostgreSQL)]
User --> DB
Job --> DB
Payment --> DB

Auth --> Kafka[(Apache Kafka)]
Job --> Kafka
Kafka --> Worker[Mail Consumer]
Worker --> SMTP[Nodemailer SMTP]

Payment --> Razorpay[Razorpay]
User --> Cloudinary[Resume Storage]
```

# 🔄 Payment Lifecycle

``` mermaid
sequenceDiagram
actor User
participant Frontend
participant Payment
participant Razorpay
participant DB

User->>Frontend: Buy Premium
Frontend->>Payment: Create Order
Payment->>Razorpay: Create Order
Razorpay-->>Frontend: Order ID
User->>Razorpay: Complete Payment
Frontend->>Payment: Verify Signature
Payment->>Payment: Validate HMAC SHA256
Payment->>DB: Update Subscription +30 Days
Payment-->>Frontend: Success
```

# 📧 Kafka Email Flow

``` mermaid
flowchart LR
Auth-->Kafka
Job-->Kafka
Kafka-->Consumer
Consumer-->Nodemailer
Nodemailer-->Email[User Inbox]
```


# 🌐 Deployment Topology

``` mermaid
graph LR
Browser-->Nginx
Nginx-->Auth
Nginx-->User
Nginx-->Job
Nginx-->Payment
Nginx-->Utils
Auth-->Neon
User-->Neon
Job-->Neon
Payment-->Neon
Auth-->Kafka
Job-->Kafka
Kafka-->Worker
Worker-->SMTP
```

# ✨ Features

-   AI ATS Resume Analysis
-   JWT Authentication
-   Role-based access
-   Premium subscriptions
-   Razorpay payment verification
-   Dynamic skills management
-   Job application engine
-   Kafka event-driven emails
-   Atomic SQL transactions
-   Neon Serverless PostgreSQL
-   Dockerized microservices
-   Kubernetes-ready deployment

# 📂 Repository Structure

``` text
talentiq/
├── frontend/
├── auth-service/
├── user-service/
├── job-service/
├── payment-service/
├── utils-service/
├── docker-compose.yml
└── README.md
```

# 🚀 Quick Start

``` bash
git clone <repository>
cd talentiq
docker compose up --build
```

# 📊 Monitoring

-   Prometheus
-   Grafana
-   Kafka Metrics
-   API Metrics
-   PostgreSQL Monitoring

# 🔒 Security

-   JWT Authentication
-   bcrypt Password Hashing
-   HMAC SHA-256 Payment Verification
-   SQL Transactions
-   Input Validation
-   Environment Variables

# ☸️ Kubernetes Ready

Supports: - Horizontal Pod Autoscaler - Ingress - ConfigMaps - Secrets -
Rolling Updates - Readiness/Liveness Probes

# 📈 Future Roadmap

-   Resume recommendation engine
-   Interview scheduling
-   Elasticsearch job search
-   OpenTelemetry tracing
-   Helm charts
-   Multi-region deployment

