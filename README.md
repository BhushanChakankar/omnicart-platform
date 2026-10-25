# OmniCart Platform

AI-powered, cloud-native, multi-vendor commerce and digital services platform built with independently deployable polyglot microservices.

OmniCart is designed to support everything from a small **kirana/general store** to large retailers, restaurants, marketplaces, logistics providers, and eventually travel and mobility services.

The platform is intentionally **product-category independent**. Customers should be able to discover and purchase any supported product or service from a single ecosystem.

---

## 🚀 Vision

OmniCart aims to evolve into a unified commerce ecosystem combining:

- E-Commerce
- Multi-Vendor Marketplace
- Quick Commerce
- Retail
- Grocery / Kirana
- Restaurant Commerce
- Logistics
- AI-powered commerce
- Real-time delivery tracking
- Travel
- Transportation
- Hospitality
- Mobility Services

The long-term vision is to build a platform capable of supporting business models similar to large global marketplaces and digital ecosystems while allowing independent sellers and businesses to operate through the same platform.

---

# 🏗️ Architecture

OmniCart follows a **microservices-based, event-driven, cloud-native architecture**.

```text
                    ┌───────────────────────┐
                    │      Customer         │
                    │ Web / Android / iOS   │
                    └───────────┬───────────┘
                                │
                                ▼
                    ┌───────────────────────┐
                    │      API Gateway      │
                    │ Auth / Routing / Rate │
                    │ Limiting / Security   │
                    └───────────┬───────────┘
                                │
          ┌─────────────────────┼──────────────────────┐
          │                     │                      │
          ▼                     ▼                      ▼
     Identity Domain       Commerce Domain        Operations
          │                     │                      │
     User Service          Product Service        Admin Service
     Seller Service         Catalog Service        Audit Service
     Auth / RBAC            Inventory Service      Reporting
                            Pricing Service
                            Cart Service
                            Order Service
                            Checkout Service
                            Payment Service
                            Refund Service
                                  │
                                  ▼
                         ┌─────────────────┐
                         │      Kafka      │
                         │   Event Bus     │
                         └────────┬────────┘
                                  │
                 ┌────────────────┼────────────────┐
                 ▼                ▼                ▼
             Delivery           AI            Notification
             Services         Services          Services
                 │
                 ▼
         Delivery Partners
                 │
                 ▼
             Customer
```

---

# 🎯 Core Architecture Principles

## 1. Independent Microservices

Each business capability is implemented as an independently deployable service.

For example:

```text
product-service
inventory-service
order-service
payment-service
delivery-service
```

A change to one service should not require rebuilding or deploying unrelated services.

---

## 2. Polyglot Development

Developers are free to choose the most appropriate technology for their service.

Supported technologies may include:

```text
Java
Spring Boot
Python
FastAPI
Node.js
TypeScript
Go
Ruby
Rust
```

Frontend applications may use:

```text
React
Next.js
Vue.js
Angular
```

The architecture is based on **API and event contracts**, not programming languages.

Example:

```text
Payment Service
    Java + Spring Boot

AI Service
    Python + FastAPI

Search Service
    Python / Java

Customer Web
    Next.js + React

Admin Web
    React

Mobile
    Android / iOS
```

---

# 🔐 Security

Security is a first-class platform capability.

The platform will support:

- OAuth 2.0
- OpenID Connect
- JWT
- Role-Based Access Control
- Multi-Factor Authentication
- Password policies
- Account verification
- API rate limiting
- Input validation
- Secure headers
- Encryption in transit
- Encryption at rest
- Audit logging
- Secrets management
- Dependency scanning
- Container security scanning

No credentials or secrets should ever be committed to Git.

Secrets will be managed using:

```text
AWS Secrets Manager
```

---

# 👥 Roles

The platform will support role-based authentication and authorization.

Initial roles:

```text
CUSTOMER
SELLER
SELLER_ADMIN
STORE_MANAGER
DELIVERY_PARTNER
DELIVERY_MANAGER
SUPPORT_AGENT
FINANCE
OPERATIONS
ADMIN
SUPER_ADMIN
```

Authorization will be enforced at both the gateway and individual service levels.

---

# 🧩 Core Microservices

## Identity & User Domain

```text
identity-service
user-service
seller-service
```

Responsibilities include:

- Registration
- Login
- Authentication
- Authorization
- User profiles
- Seller onboarding
- Seller management
- RBAC
- MFA
- Account management

---

## Product & Catalog Domain

```text
product-service
catalog-service
inventory-service
pricing-service
search-service
```

Responsibilities include:

- Products
- SKUs
- Product variants
- Categories
- Brands
- Product attributes
- Inventory
- Warehouses
- Store inventory
- Pricing
- Discounts
- Search
- Filters
- Sorting

---

## Customer Commerce Domain

```text
cart-service
wishlist-service
review-service
rating-service
```

Responsibilities include:

- Shopping cart
- Wishlist
- Product reviews
- Seller reviews
- Ratings
- Saved products
- Back-in-stock notifications
- Price-drop notifications

---

# 🛒 Order & Checkout Domain

```text
order-service
checkout-service
payment-service
refund-service
promotion-service
tax-service
```

Responsibilities include:

- Checkout
- Order creation
- Order lifecycle
- Payment
- Refunds
- Promotions
- Coupons
- Tax calculation
- Cancellation
- Partial cancellation
- Partial refund
- Order history

---

# 💳 Payment Architecture

The payment platform will be provider-independent.

Potential providers:

```text
Razorpay
Stripe
PhonePe
UPI Providers
Card Processors
Future Payment Providers
```

Supported payment methods may include:

```text
UPI
Credit Card
Debit Card
Net Banking
Wallets
Apple Pay
Google Pay
International Cards
INR
USD
Future currencies
```

Payment-provider-specific implementation will remain inside the payment service.

Business services should never depend directly on a specific payment provider.

---

# 🔄 Saga Pattern

Distributed transactions will use the **Saga Pattern**.

Example order workflow:

```text
Create Order
      │
      ▼
Reserve Inventory
      │
      ▼
Initiate Payment
      │
      ▼
Payment Successful
      │
      ▼
Confirm Order
```

If payment fails:

```text
Payment Failed
      │
      ▼
Release Inventory
      │
      ▼
Cancel Order
```

Saga workflows will be implemented using Kafka events and appropriate orchestration/choreography patterns.

---

# 📨 Kafka Event Architecture

Kafka will be used for asynchronous communication between services.

Example event topics:

```text
order.events
payment.events
inventory.events
delivery.events
product.events
seller.events
notification.events
refund.events
```

Events should contain:

```text
eventId
eventType
eventVersion
timestamp
correlationId
causationId
aggregateId
producer
schemaVersion
```

Events must be:

- Idempotent
- Versioned
- Traceable
- Backward compatible

---

# 🚚 Delivery Platform

Delivery will be an independent business domain.

```text
delivery-service
delivery-partner-service
tracking-service
location-service
```

The platform can support:

```text
Seller Delivery
OmniCart Delivery
Third-Party Logistics
External Delivery Partners
```

Delivery selection can consider:

- Distance
- Cost
- Availability
- SLA
- Delivery time
- Vehicle type
- Provider reliability
- Product type

---

# ⚡ Quick Commerce

The platform will support multiple fulfillment modes:

```text
STANDARD
EXPRESS
SAME_DAY
SCHEDULED
QUICK_COMMERCE
```

Example:

```text
Milk
→ Delivery within 10–30 minutes

Electronics
→ Delivery within 1–2 days

Custom Product
→ Delivery within several days
```

The estimated delivery date/time should be shown to the customer before purchase.

---

# 📍 Real-Time Delivery Tracking

The delivery platform will support real-time tracking.

Potential mapping infrastructure:

```text
Google Maps
```

Tracking capabilities:

- Delivery partner location
- Customer location
- Seller location
- Route
- Distance
- ETA
- Geofencing
- Live tracking

Example:

```text
Delivery App
      │
      ▼
Location Service
      │
      ▼
Kafka
      │
      ▼
Tracking Service
      │
      ▼
WebSocket / SSE
      │
      ▼
Customer Application
```

---

# 🤖 AI Platform

AI capabilities will be implemented as independent services.

Potential AI services:

```text
ai-service
recommendation-service
fraud-detection-service
```

Future AI capabilities:

- AI product search
- AI recommendations
- Personalized shopping
- Product categorization
- Product description generation
- Demand forecasting
- Inventory prediction
- Dynamic pricing recommendations
- Fraud detection
- Fake review detection
- Review moderation
- Customer support assistant
- Seller AI assistant
- Delivery ETA prediction

Example:

```text
Customer:

"I need a birthday gift for a 5 year old boy
under ₹2,000."

        │
        ▼

AI Search
        │
        ▼

Catalog
        │
        ▼

Product Search
        │
        ▼

Recommendation Engine
        │
        ▼

Personalized Results
```

---

# 🏢 Seller Platform

OmniCart should support different types of sellers:

```text
Kirana Store
General Store
Supermarket
Restaurant
Retailer
Brand
Manufacturer
Distributor
Enterprise Seller
Individual Seller
```

A seller can have:

```text
Multiple Stores
Warehouses
Products
Inventory
Employees
Delivery Configuration
Payment Configuration
Business Rules
```

---

# 🏬 Multi-Store Inventory

A seller may operate multiple locations.

```text
Seller
 │
 ├── Store A
 ├── Store B
 ├── Store C
 └── Warehouse
```

Inventory is location-aware.

This allows the platform to select the closest available inventory for faster delivery.

---

# 📦 Order Fulfillment

The platform will support:

```text
Single Seller Order
Multi-Seller Order
Multi-Warehouse Order
Split Shipment
Partial Fulfillment
Partial Cancellation
Partial Refund
```

Example:

```text
Order #1001

Seller A
 ├── Product A
 └── Product B

Seller B
 └── Product C
```

The customer can still see the purchase as one unified order.

---

# ❌ Cancellation & Refund

The platform will support cancellation based on order state.

Possible scenarios:

```text
Before Payment
After Payment
Before Packing
After Packing
Before Dispatch
After Dispatch
Before Delivery
After Delivery
```

The system should calculate:

```text
Can Cancel?
Cancellation Fee?
Refund Amount?
Inventory Release?
Delivery Cancellation?
Seller Compensation?
Payment Refund?
```

Refunds must be idempotent.

---

# 🔔 Notification Service

Central notification service:

```text
notification-service
```

Supported channels:

```text
Email
SMS
Push Notification
WhatsApp
In-App Notifications
```

Example events:

```text
Order Placed
Payment Successful
Payment Failed
Order Packed
Delivery Assigned
Out For Delivery
Delivery Nearby
Order Delivered
Refund Initiated
Refund Completed
```

---

# 🛠️ Admin Platform

A centralized Admin Dashboard will provide operational control.

Admin capabilities:

```text
Users
Sellers
Products
Orders
Payments
Refunds
Delivery
Delivery Partners
Reviews
Complaints
Promotions
Fraud
Reports
Audit
System Health
```

---

# 📊 Operations Dashboard

Operations teams should be able to monitor:

```text
Live Orders
Failed Orders
Delayed Orders
Payment Failures
Inventory Failures
Checkout Failures
Delivery Failures
Refund Failures
Kafka Consumer Lag
Service Health
Infrastructure Health
```

---

# 📝 Audit Service

Important business and administrative operations must be auditable.

Example:

```text
Actor:
ADMIN

Action:
Changed seller commission

Before:
10%

After:
12%

Timestamp:
2026-10-25T10:30:00Z

Reason:
Business policy update
```

---

# 🔎 Search

Search should support:

```text
Product Search
Seller Search
Category Search
Brand Search
Autocomplete
Filters
Sorting
Location-based Search
```

Potential implementation:

```text
OpenSearch / Elasticsearch
```

---

# 🗄️ Database Architecture

Primary database:

```text
PostgreSQL
```

Principle:

> Each microservice owns its data.

Example:

```text
User Service
    → User Database

Product Service
    → Product Database

Inventory Service
    → Inventory Database

Order Service
    → Order Database

Payment Service
    → Payment Database
```

Services must never directly query another service's database.

---

# ⚡ Redis

Redis may be used for:

```text
Caching
Rate Limiting
Idempotency
Short-lived data
Distributed coordination where appropriate
```

Redis must not become the primary source of truth for transactional business data.

---

# 🪣 AWS S3

S3 will be used for object storage.

Potential data:

```text
Product Images
Seller Documents
Invoices
Reports
Exports
Media
AI Generated Assets
```

Production buckets must use appropriate:

- Encryption
- IAM policies
- Private access
- Lifecycle policies
- Access controls

---

# ☁️ AWS Infrastructure

Potential AWS services:

```text
EKS
EC2
Lambda
S3
RDS PostgreSQL
ElastiCache Redis
MSK Kafka
Secrets Manager
ECR
ALB
WAF
Route 53
CloudFront
IAM
CloudWatch
```

Infrastructure should be managed through Infrastructure as Code where practical.

---

# ☸️ Kubernetes

Production services will run on Kubernetes.

Each service should have its own deployment configuration.

Example:

```text
payment-service
    ├── Deployment
    ├── Service
    ├── ConfigMap
    ├── HPA
    └── Health Checks
```

Production deployments should support:

```text
Rolling Updates
Horizontal Pod Autoscaling
Readiness Probes
Liveness Probes
Startup Probes
Resource Requests
Resource Limits
Pod Disruption Budgets
```

---

# 🔍 Service Discovery

For Kubernetes:

```text
Kubernetes Service Discovery
```

should be preferred.

Example:

```text
http://inventory-service
http://order-service
http://payment-service
```

Eureka may be supported for:

```text
Local Development
VM-based Deployments
Legacy Spring Cloud Environments
```

Kubernetes-native service discovery should be the default production approach.

---

# 🛡️ Resilience

The platform should support:

```text
Timeout
Retry
Circuit Breaker
Bulkhead
Rate Limiting
Fallback
Dead Letter Queue
Idempotency
```

For Spring Boot services, **Resilience4j** will be preferred for new implementations.

---

# 📈 Observability

The platform will provide:

```text
Metrics
Logs
Traces
Dashboards
Alerts
```

Technology stack:

```text
Prometheus
Grafana
Loki
OpenTelemetry
```

Potential metrics:

```text
API Latency
Error Rate
Request Rate
Order Rate
Payment Success Rate
Payment Failure Rate
Kafka Consumer Lag
Database Connections
CPU
Memory
Pod Restarts
Checkout Failures
Refund Failures
Delivery Failures
```

---

# 🔗 Distributed Tracing

Requests should carry:

```text
traceId
spanId
correlationId
```

Example:

```text
Customer
   ↓
API Gateway
   ↓
Checkout
   ↓
Order
   ↓
Inventory
   ↓
Payment
   ↓
Kafka
   ↓
Notification
```

The complete transaction should be traceable across services.

---

# 🐳 Docker

Every independently deployable service must be containerized.

Example:

```text
services/
├── payment-service/
│   └── Dockerfile
│
├── inventory-service/
│   └── Dockerfile
│
└── ai-service/
    └── Dockerfile
```

Container images should be immutable and versioned.

---

# 🔄 CI/CD

Jenkins will automate:

```text
Build
Unit Tests
Integration Tests
Contract Tests
Code Coverage
SonarQube
Security Scanning
Docker Build
Container Scanning
Image Push
Kubernetes Deployment
Smoke Tests
```

Pipeline:

```text
Developer
    ↓
Pull Request
    ↓
Jenkins
    ↓
Build
    ↓
Unit Tests
    ↓
Integration Tests
    ↓
Coverage >= 80%
    ↓
SonarQube
    ↓
Security Scan
    ↓
Docker Build
    ↓
AWS ECR
    ↓
Kubernetes
    ↓
Smoke Tests
```

---

# 🧪 Testing Standards

Testing is mandatory.

Required where applicable:

```text
Unit Tests
Integration Tests
Contract Tests
End-to-End Tests
Performance Tests
Security Tests
```

Java:

```text
JUnit
Mockito
Testcontainers
```

Every service must maintain a minimum target of:

```text
80% Code Coverage
```

Coverage alone does not guarantee quality; meaningful test scenarios are required.

---

# 📖 API Documentation

Every REST service must provide OpenAPI documentation.

Example:

```text
GET    /api/v1/products
POST   /api/v1/products
GET    /api/v1/products/{id}
PUT    /api/v1/products/{id}
DELETE /api/v1/products/{id}
```

Swagger UI will be available for local development and testing.

---

# 📨 Event Documentation

Kafka events will be documented using AsyncAPI.

Example:

```text
order.created
order.confirmed
order.cancelled

payment.initiated
payment.succeeded
payment.failed

inventory.reserved
inventory.released
```

Events must be versioned and backward compatible.

---

# 📱 Applications

The repository will contain multiple client applications.

```text
applications/
├── customer-web/
├── seller-web/
├── admin-web/
├── operations-web/
├── mobile-web/
├── android/
└── ios/
```

Possible technologies:

```text
Next.js
React
Vue.js
Angular
Android
iOS
React Native
Flutter
```

Frontend applications communicate through the API Gateway and never directly access internal microservices.

---

# 🔮 Future Domains

The architecture is designed to allow future domains to be added independently.

Potential future services:

```text
hotel-service
flight-service
railway-service
bus-service
car-rental-service
bike-rental-service
ride-booking-service
```

These domains should have their own bounded contexts and workflows.

---

# 📁 Repository Structure

```text
omnicart-platform/
│
├── README.md
├── CONTRIBUTING.md
├── SECURITY.md
├── CODEOWNERS
│
├── applications/
│   ├── customer-web/
│   ├── seller-web/
│   ├── admin-web/
│   ├── operations-web/
│   ├── mobile-web/
│   ├── android/
│   └── ios/
│
├── services/
│   ├── api-gateway/
│   ├── identity-service/
│   ├── user-service/
│   ├── seller-service/
│   ├── catalog-service/
│   ├── product-service/
│   ├── inventory-service/
│   ├── pricing-service/
│   ├── cart-service/
│   ├── wishlist-service/
│   ├── review-service/
│   ├── rating-service/
│   ├── search-service/
│   ├── checkout-service/
│   ├── order-service/
│   ├── payment-service/
│   ├── refund-service/
│   ├── promotion-service/
│   ├── tax-service/
│   ├── delivery-service/
│   ├── delivery-partner-service/
│   ├── tracking-service/
│   ├── location-service/
│   ├── notification-service/
│   ├── recommendation-service/
│   ├── ai-service/
│   ├── fraud-detection-service/
│   ├── audit-service/
│   └── reporting-service/
│
├── infrastructure/
│   ├── aws/
│   ├── kafka/
│   ├── postgres/
│   ├── redis/
│   └── monitoring/
│
├── platform/
│   ├── docker/
│   ├── kubernetes/
│   ├── helm/
│   ├── terraform/
│   └── jenkins/
│
├── docs/
│   ├── architecture/
│   ├── api/
│   ├── events/
│   └── adr/
│
├── tests/
│   ├── integration/
│   ├── contract/
│   ├── e2e/
│   └── performance/
│
└── scripts/
    ├── local-start.sh
    ├── local-stop.sh
    ├── test-all.sh
    └── changed-services.sh
```

---

# 🗺️ Development Roadmap

## Phase 1 — Repository & Platform Foundation

- [ ] Repository structure
- [ ] Branch protection
- [ ] CODEOWNERS
- [ ] Contribution guidelines
- [ ] Docker standards
- [ ] Local development environment
- [ ] Jenkins
- [ ] Kubernetes
- [ ] AWS foundation
- [ ] PostgreSQL
- [ ] Kafka
- [ ] Redis
- [ ] AWS Secrets Manager
- [ ] Prometheus
- [ ] Grafana
- [ ] Loki
- [ ] OpenTelemetry

---

## Phase 2 — Identity & Security

- [ ] Identity Service
- [ ] User Service
- [ ] Authentication
- [ ] Authorization
- [ ] RBAC
- [ ] OAuth2
- [ ] OpenID Connect
- [ ] JWT
- [ ] MFA
- [ ] API Gateway
- [ ] Rate Limiting
- [ ] Audit Logging

---

## Phase 3 — Marketplace

- [ ] Seller Service
- [ ] Catalog Service
- [ ] Product Service
- [ ] Inventory Service
- [ ] Pricing Service
- [ ] Search Service
- [ ] Review Service
- [ ] Rating Service

---

## Phase 4 — Commerce

- [ ] Cart Service
- [ ] Wishlist Service
- [ ] Checkout Service
- [ ] Order Service
- [ ] Payment Service
- [ ] Refund Service
- [ ] Promotion Service
- [ ] Tax Service
- [ ] Notification Service

---

## Phase 5 — Delivery

- [ ] Delivery Service
- [ ] Delivery Partner Service
- [ ] Tracking Service
- [ ] Location Service
- [ ] Delivery ETA
- [ ] Maps Integration
- [ ] Real-Time Tracking
- [ ] External Delivery Providers

---

## Phase 6 — AI

- [ ] AI Service
- [ ] AI Search
- [ ] Recommendation Engine
- [ ] Seller AI Assistant
- [ ] Product Classification
- [ ] Review Moderation
- [ ] Fraud Detection
- [ ] Demand Forecasting
- [ ] Inventory Prediction
- [ ] ETA Prediction

---

## Phase 7 — Applications

- [ ] Customer Web
- [ ] Seller Web
- [ ] Admin Web
- [ ] Operations Dashboard
- [ ] Mobile Web
- [ ] Android
- [ ] iOS

---

## Phase 8 — Future Services

- [ ] Hotel Service
- [ ] Flight Service
- [ ] Railway Service
- [ ] Bus Service
- [ ] Car Rental Service
- [ ] Bike Rental Service
- [ ] Ride Booking Service
- [ ] Hospitality Services

---

# ✅ Definition of Done

A feature is considered complete only when applicable requirements are satisfied:

```text
[ ] Code implemented
[ ] Unit tests implemented
[ ] Minimum 80% coverage
[ ] Integration tests
[ ] Contract tests
[ ] API documentation
[ ] Event documentation
[ ] Input validation
[ ] Error handling
[ ] Idempotency
[ ] Security review
[ ] SonarQube passed
[ ] Dependency/security scan
[ ] Docker image
[ ] Kubernetes deployment
[ ] Health checks
[ ] Metrics
[ ] Structured logging
[ ] Distributed tracing
[ ] Documentation updated
[ ] Pull Request reviewed
[ ] Jenkins pipeline passed
```

---

# 🤝 Contribution Model

Developers can work on different services independently.

Example:

```text
Developer A
→ payment-service

Developer B
→ inventory-service

Developer C
→ customer-web

Developer D
→ ai-service

Developer E
→ delivery-service
```

All development happens inside the same repository while services remain independently deployable.

Changes should be submitted through Pull Requests.

Direct pushes to `main` should be restricted.

---

# 🧭 Engineering Rules

1. Services must own their data.
2. Services must not directly access another service's database.
3. Business logic must remain inside the appropriate bounded context.
4. APIs must be contract-driven.
5. Kafka events must be versioned.
6. Critical operations must be idempotent.
7. Secrets must never be committed.
8. Production services must be observable.
9. Automated tests are mandatory.
10. Minimum target code coverage is 80%.
11. Production deployments must be automated.
12. Breaking API/event changes require versioning.
13. Services must be independently deployable.
14. Frontend applications must communicate through the API Gateway.
15. Infrastructure must be reproducible through Infrastructure as Code.
16. Security must be considered during design, development and deployment.

---

# 🌐 Long-Term Goal

OmniCart is designed to become more than an e-commerce application.

The long-term platform vision is:

```text
                         OMNICART
                            │
          ┌─────────────────┼─────────────────┐
          │                 │                 │
       Commerce          Logistics          Services
          │                 │                 │
     ┌────┼────┐       ┌────┼────┐       ┌────┼────┐
     │    │    │       │    │    │       │    │    │
   Retail Food B2B   Delivery ETA Tracking Hotel Flight Transport
     │
  Grocery
  Electronics
  Fashion
  Cosmetics
  Industrial
  Any Product
                            │
                            ▼
                           AI
                            │
                            ▼
                  Web / Android / iOS
                            │
                            ▼
                AWS / Kubernetes / Kafka
```

---

## ⭐ Project Status

**Status:** Initial Architecture / Foundation

This repository is currently being established as the central platform repository.

The architecture and services will evolve incrementally through documented Architecture Decision Records (ADRs).

---

## 📄 License

License to be determined.

---

## 🔒 Security

If you discover a security vulnerability, do not disclose sensitive details publicly through an issue. Follow the security reporting process defined in `SECURITY.md`.
