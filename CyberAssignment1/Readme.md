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
