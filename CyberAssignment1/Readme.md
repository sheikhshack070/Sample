Online Payment Processing System

Task 1 – System Definition and Architecture

1. System Overview

This system enables customers to pay merchants online. It supports core payment workflows such as authorization, capture, refunds, dispute handling, and payment status tracking. It also integrates with a payment processor and a core banking system to support settlement and reconciliation.

The architecture is layered, cloud-agnostic, and assumes internet-facing exposure. The design considers both:

External threats (internet-based attacks)

Insider threats (misuse of administrative privileges)

Because the system processes financial transactions and sensitive data, it prioritizes the following security objectives:

Confidentiality – Protection of sensitive customer and merchant data

Integrity – Accurate and tamper-resistant transaction processing

Availability – Continuous payment operations

Accountability – Auditable financial and administrative actions

2. Application Components
2.1 Frontend Layer

The frontend layer consists of three web applications:

Customer Web Application

Checkout

Order history

Payment status tracking

Saved payment methods (tokenized)

Merchant Portal

Transaction viewing

Refund requests

Settlement reports

API key management

Admin Portal

Refund approvals

Dispute handling

Monitoring dashboards

Account management

All frontend applications communicate with backend services over HTTPS.

2.2 Backend Services

API Gateway

Central entry point for all external requests

Authentication and authorization

Rate limiting

Request validation

API Backend (Business Logic Service)

Order management

Transaction lifecycle handling

Validation rules

Business workflows

Payment Service

Payment orchestration

Token handling

Communication with payment processor

Authorization, capture, refund execution

Webhook Handler

Receives callbacks from payment processor

Validates webhook signatures

Updates transaction state

Admin Service

Privileged operations

Refund approvals

Dispute management

Financial adjustments

Notification Service (Optional)

Email/SMS notifications

Receipts

Status updates

3. Data Storage (Private Zone)

All databases are located in a restricted internal network zone.
Databases are not directly accessible from the internet.

User Database

Customer profiles

Hashed credentials

Contact details

Merchant Database

Merchant profiles

Payout configuration

API credentials metadata

Transaction / Billing Database

Orders

Payments

Refund records

Settlement details

Token Vault / Secrets Store

Payment tokens

Cryptographic secrets

Audit Log Store (Immutable)

Security events

Administrative actions

Transaction lifecycle changes

4. External Integrations

The system integrates with:

Payment Processor

Authorization

Capture

Refund processing

Risk checks

Webhook callbacks

Core Banking System

Settlement

Payouts

Reconciliation

Email/SMS Provider (Optional)

Fraud / Risk Service (Optional)

All integrations occur over encrypted channels (TLS).

5. Users and Roles
Customer

Initiate payments

View transaction history

Manage profile

Merchant

View transactions

Initiate refunds

View settlement reports

Manage API credentials

Admin

Approve refunds

Handle disputes

Monitor transactions

Manage accounts

Support Agent (Limited Admin)

Read-only transaction lookup

Controlled support actions

System Integrations (Machine-to-Machine)

Merchant API clients

Payment processor webhook sender

6. Data Types Handled

Personally Identifiable Information (PII)

Merchant business data

Payment transaction data

Tokenized payment identifiers

Authentication tokens

Audit logs

Security event data

Raw card data is not stored. Tokenization is used whenever possible.

7. Trust Boundaries

The system contains the following trust boundaries:

Internet → Frontend Applications

Public exposure

High attack surface

Frontend → API Gateway

Authenticated boundary

Token/session validation

API Gateway → Backend Services

Internal service boundary

Role enforcement

Backend Services → Data Storage (Private Zone)

Sensitive data boundary

Strict access control

Backend Services → Payment Processor

External financial boundary

Encrypted and signed communication

Payment Processor → Webhook Handler

Inbound trust validation

Signature verification required

Backend Services → Core Banking System

Settlement boundary

High integrity requirement

Admin Portal → Admin Service

Privileged access boundary

Strong authentication and logging required
