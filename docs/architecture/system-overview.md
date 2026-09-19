# System Architecture Overview

Pocket Concierge is an AI-first super-app platform connecting consumers and service partners across multiple verticals (dining, grocery, retail, hospitality, rides, and on-demand services).

---

## 1. Top-Level Platform Topology

```
┌─────────────────────────────────────────────────────────────┐
│                    CLIENT APPLICATIONS                      │
│                                                             │
│   ┌─────────────────────┐         ┌─────────────────────┐   │
│   │    CONSUMER APPS    │         │    PARTNER APPS     │   │
│   │  Android (Kotlin)   │         │  Android (Kotlin)   │   │
│   │  iOS (Swift)        │         │  iOS (Swift)        │   │
│   │  Web (Next.js)      │         │  Web (Next.js)      │   │
│   └──────────┬──────────┘         └──────────┬──────────┘   │
└──────────────┼───────────────────────────────┼──────────────┘
               │                               │
               │  HTTPS / WSS (Supabase SDKs)  │
               ▼                               ▼
┌─────────────────────────────────────────────────────────────┐
│                 CENTRALIZED SUPABASE BACKEND                │
│                                                             │
│  ┌───────────────────────────────────────────────────────┐  │
│  │                    Supabase Auth                      │  │
│  │    (Consumer & Partner identity, JWT claims, RBAC)    │  │
│  └───────────────────────────┬───────────────────────────┘  │
│                              │                              │
│  ┌───────────────────────────┴───────────────────────────┐  │
│  │               PostgreSQL Database + RLS               │  │
│  │  - User Profiles            - Partner Outlets         │  │
│  │  - Catalog & Items          - Inventory & Pricing     │  │
│  │  - Orders & Items           - Realtime State          │  │
│  │  - Payment Records          - Financial Ledger        │  │
│  │  - Partner Earnings         - Settlements & Payouts   │  │
│  └───────────────────────────┬───────────────────────────┘  │
│                              │                              │
│  ┌───────────────────────────┴───────────────────────────┐  │
│  │                 Supabase Edge Functions               │  │
│  │       (Server-side trusted operations & secrets)      │  │
│  │  - payment-initiate / verify (eSewa, Khalti, Fonepay) │  │
│  │  - payout-execute (Bank settlement & disbursements)   │  │
│  │  - ai-concierge (LLM tool-calling loop)               │  │
│  │  - push-notify (FCM / APNs critical dispatch)         │  │
│  └───────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

---

## 2. Core Architectural Pillars

### A. Consumer vs. Partner Separation
- **Consumer Applications**: Optimized for discovery, AI interaction, multi-vendor cart building, checkout, and live order tracking.
- **Partner Applications**: Tailored for business operations: inventory management, incoming order queues, kitchen display systems, dispatching, financial ledger tracking, and staff role management.
- They are separate products with distinct UI flows, permissions, and operational constraints.

### B. Centralized Shared Backend
- A single, centralized Supabase infrastructure powers all clients.
- Data separation and multi-tenant security are guaranteed at the database layer via **PostgreSQL Row Level Security (RLS)** rather than siloed backend code.
- Client applications access the database directly via Supabase PostgREST clients using public `anon` credentials, scoped by JWT authentication tokens.

### C. Server-Side Financial Authority
- **Zero Client Financial Trust**: Prices, totals, discounts, taxes, fees, and partner earnings are computed and validated exclusively on the server.
- **Auditable Ledger**: Every monetary event (consumer charge, platform fee deduction, partner earning credit, refund, payout) produces an immutable ledger record.
- **Payment Abstraction**: The platform abstracts payment processing behind a unified server-side adapter layer supporting eSewa, Khalti, Fonepay, and Card gateways.
- **Independent Payout Subsystem**: Partner payouts are decoupled from consumer checkout transactions through an auditable settlement lifecycle.

### D. Prototype vs. Production Delineation
- [`prototype/web/`](../../prototype/web/) is the working Next.js interactive prototype and visual/functional reference.
- [`apps/`](../../apps/) contains the production clients (Consumer Android/iOS/Web and Partner Android/iOS/Web).
- The prototype is kept separate and fully functional so team members can experience the end-to-end design without requiring production infrastructure.
