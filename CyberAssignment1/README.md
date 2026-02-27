# Secure Architecture & Threat Model  
## Online Payment Processing Application

---

# Task 1: System Overview

This project models and secures an **Internet-facing Online Payment Processing Application**.

The system enables:
- Users to make online payments
- Merchants to receive payments
- Admins to monitor and manage transactions
- Integration with a third-party payment gateway
- Communication with a core banking system

The system is assumed to be:
- Internet-facing
- Cloud-agnostic
- Targeted by both external and insider threats

---

# 2. System Components

## 2.1 Frontend
- Public web application (users + merchants)
- Admin portal (separate interface)

## 2.2 Backend Services
- REST API backend
- Authentication service
- Payment processing service
- Transaction management service

## 2.3 Data Storage
- User database
- Merchant database
- Transaction database
- Audit log storage

## 2.4 External Integrations
- Payment gateway provider
- Core banking system

---

# 3. Actors

| Actor | Description |
|-------|-------------|
| User | Makes payments |
| Merchant | Receives payments |
| Admin | Monitors and manages system |
| Attacker (External) | Internet-based threat actor |
| Insider | Malicious employee |
| Payment Gateway | External financial processor |
| Bank System | Core banking infrastructure |

---

# 4. Assets

## High-Value Assets
- Payment card data
- Transaction records
- User credentials
- Merchant account data
- Banking integration secrets
- API keys
- System configuration

## Security Properties (CIA)

| Asset | Confidentiality | Integrity | Availability |
|--------|----------------|-----------|--------------|
| Card Data | Critical | Critical | Medium |
| Transactions | High | Critical | High |
| Credentials | Critical | High | High |
| Logs | Medium | High | Medium |

---

# 5. Data Types Handled

- Personally Identifiable Information (PII)
- Payment card data
- Transaction amounts
- Authentication credentials
- Audit logs
- API tokens

---
## High-Level Architecture Diagram

![High Level Architecture](./HighLevelDiagram.png)

---
# 7. Trust Boundaries

1. Internet Boundary (User → Frontend)
2. Frontend → Backend boundary
3. Backend → Database boundary
4. Backend → Payment Gateway boundary
5. Admin access boundary
6. Internal service-to-service boundary

Each boundary represents a change in trust level and must be secured.

---


# Task 2: Asset Identification and Security Objectives

## 1. Asset Identification

## 1.1 Asset Inventory Table

| Asset ID | Asset Name | Description | Asset Type | Sensitivity Level |
|-----------|------------|-------------|------------|-------------------|
| A1 | User Credentials | Passwords, MFA secrets, tokens | Authentication Data | Critical |
| A2 | Payment Card Data | Card number, CVV, expiry | Financial Data | Critical |
| A3 | Transaction Records | Payment amounts, timestamps, merchant ID | Financial Data | High |
| A4 | Personal Data | Name, email, phone number | PII | High |
| A5 | Merchant Account Data | Bank account details, payout info | Financial Data | Critical |
| A6 | API Keys & Secrets | Gateway credentials, service tokens | Secrets | Critical |
| A7 | Business Logic | Payment validation, transaction workflows | Intellectual Property | High |
| A8 | Audit Logs | Admin actions, transaction logs | Security Evidence | High |
| A9 | System Availability | Uptime of services and APIs | Operational Asset | Critical |

---

## 2. Security Objectives

The system must ensure the following security properties:

### Confidentiality
Sensitive information must not be disclosed to unauthorized parties.

### Integrity
Data must not be altered without authorization.

### Availability
System services must remain operational and accessible.

### Accountability
Actions must be traceable to a specific identity.

---

## 3. Mapping Assets to Security Objectives

| Asset | Confidentiality | Integrity | Availability | Accountability |
|--------|----------------|-----------|--------------|----------------|
| User Credentials | Critical | High | High | High |
| Payment Card Data | Critical | Critical | Medium | High |
| Transaction Records | High | Critical | High | High |
| Personal Data | High | High | Medium | Medium |
| Merchant Account Data | Critical | Critical | High | High |
| API Keys & Secrets | Critical | High | High | High |
| Business Logic | Medium | Critical | High | Medium |
| Audit Logs | Medium | Critical | High | Critical |
| System Availability | Low | Medium | Critical | Medium |

---




