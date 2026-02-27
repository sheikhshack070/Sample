# Secure Architecture & Threat Model  
## Online Payment Processing Application

---

# 1. System Overview

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

Admin Portal → Admin Service

Privileged access boundary

Strong authentication and logging required
