# .github

# Warehouse Management & B2B Ordering Platform

A unified platform for managing products, inventory, orders, payments, and financial reporting for B2B operations.  
This system provides a single source of truth for warehouse operations and integrates with a third-party payment gateway for transaction processing.

---

## Overview

This project is designed as an end-to-end solution covering:

- Product and inventory management
- B2B ordering workflow
- Role-based business dashboard
- Payment processing via external gateway
- Financial and transaction reporting

The system architecture separates **business logic** from **payment orchestration** to ensure scalability, auditability, and operational correctness.

---

## Core Components

### 1. Customer Portal
The customer-facing interface for B2B buyers.

**Responsibilities:**
- Product catalog browsing
- Order creation and submission
- Account management
- Order history and status tracking
- Secure redirection to payment gateway

---

### 2. Business Dashboard
Internal interface for business operations.

**Modules:**
- Product & inventory management
- Order management
- Financial reports
- Transaction monitoring
- Role & access control

---

### 3. Payment Integration Layer
Integration with a third-party payment provider.

**Responsibilities:**
- Invoice creation
- Payment method handling
- Payment status updates via webhooks
- Settlement reconciliation

> Note: The payment provider is treated strictly as a payment processor.  
> All business data, reporting, and logic remain within this system.

---

## Architecture Principles

### Single Source of Truth
- **Business data** (products, inventory, orders, users, roles) are owned by this system.
- **Payment data** (payment status, settlement, fees) are retrieved from the payment provider and synchronized via API/webhooks.

### Base Unit Normalization
- Internally, inventory and order quantities are normalized to a single base unit.
- All calculations and stock deductions are performed using integer arithmetic.
- No fractional quantities are allowed at any stage.

### Deterministic Operations
- No floating-point arithmetic in inventory or pricing.
- All conversions and calculations are deterministic and reversible.
- This guarantees auditability and prevents stock drift.

---

## Data Flow

1. Buyer creates an order in the Customer Portal.
2. System validates inventory and pricing.
3. System creates an invoice via the payment provider API.
4. Buyer completes payment on the payment provider’s interface.
5. Payment provider sends webhook notification.
6. System updates order and financial records.
7. Data becomes available in the Business Dashboard.

---

## Access Control

The system supports role-based access control, including but not limited to:

- Super Administrator
- Administrator
- Finance
- Operations / Staff

Each role has strictly scoped access to features and data.

---

## Reporting & Reconciliation

All reports are generated from internal data combined with synchronized payment data.

Supported reporting dimensions include:
- Orders
- Revenue
- Outstanding payments
- Settlements
- Transaction history

The system does **not** rely on the payment provider’s dashboard for business reporting.

---

## Technology Stack

### Backend
- **Node.js + ExpressJS**
- RESTful API architecture
- Webhook handling for payment events

### Frontend
- **React + Vite**
- Single codebase for:
  - Customer Portal
  - Business Dashboard

### Database
- **MongoDB (NoSQL)**
- Document-based schema for flexibility and scalability

### Payment
- **Xendit**
- Invoice-based payment flow
- Webhook-driven reconciliation

### Authentication
- **JWT (JSON Web Token)**
- Longer-session based authentication strategy
- Role-based access control on top of JWT

### Infrastructure
- **Managed Database**
- **Docker** for containerization
- **Sentry** for error tracking and monitoring

---

## Non-Goals

- The system does not act as a logistics provider.
- The system does not expose internal base units to end users.
- The system does not delegate business logic to the payment provider.

---

## Key Invariants

These rules must never be violated:

1. No fractional inventory
2. No fractional orders
3. All inventory changes are atomic
4. All payments are reconciled via webhook
5. All business reports are generated internally

---

## License

© Koperasi Konsumen Al Futuhat Rabbaniyah Wailahiyah.  
All rights reserved. This software is proprietary and may not be used, copied, modified, or distributed without explicit written permission from the copyright holder.
