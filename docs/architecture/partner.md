# Partner Architecture

The Partner application enables merchants, restaurants, service providers, and gig operators to manage their business, catalog, orders, and finances on the Pocket Concierge platform.

---

## 1. Platform Clients

| Client | Directory | Technology | Build / Package |
|---|---|---|---|
| **Android** | `apps/partner/android/` | Kotlin, Jetpack Compose, Foreground Services | Gradle |
| **iOS** | `apps/partner/ios/` | Swift, SwiftUI, Critical Alerts | Xcode |
| **Web Portal** | `apps/web/partner/` | Next.js, React, Tailwind, Data Tables, Charts | npm |

---

## 2. Distinct Partner Workflows

The platform treats different partner domains with specialized operational models:

```
┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐
│   RESTAURANT    │  │ RETAIL/GROCERY  │  │   HOSPITALITY   │  │ MOBILITY/RIDER  │
│ Order prep time │  │ Barcode scan    │  │ Room inventory  │  │ Dispatch queue  │
│ Kitchen display │  │ Stock counts    │  │ Booking calendar│  │ Route navigation│
│ Station routing │  │ Pick & pack     │  │ Check-in/out    │  │ Fare settlement │
└─────────────────┘  └─────────────────┘  └─────────────────┘  └─────────────────┘
```

---

## 3. Core Functional Capabilities

### A. Partner Onboarding & Business Profile
- Submission of business details, trade licenses, VAT/PAN certificates, bank accounts.
- Multi-step verification state machine: `submitted` → `under_review` → `verified` / `rejected` → `active`.
- Store hours, operating days, holiday schedules, and temporary "busy" or "closed" toggles.

### B. Catalog, Menu & Inventory Control
- Hierarchical category and product creation.
- Support for complex product variants (sizes, colors) and modifier groups (toppings, add-ons, required vs. optional selections).
- Stock level tracking with automated out-of-stock toggling.

### C. Live Order Dispatch & Kitchen Display
- High-priority real-time order arrival via WebSocket subscriptions (Supabase Realtime) and device push notifications.
- Order review actions:
  - **Accept**: Sets estimated prep time (e.g., 15 mins).
  - **Reject**: Requires a structured reason (out of ingredients, store overloaded).
- Status advancement: `acknowledged` → `preparing` → `ready_for_pickup` → `dispatched` → `delivered`.
- Bluetooth / LAN ESC/POS thermal receipt and kitchen order ticket (KOT) printing.

### D. Financials, Earnings & Ledger
- Per-order earnings breakdown:
  - Gross Order Value
  - Platform Commission / Fee
  - Taxes / VAT withheld
  - Net Partner Earning
- Financial balance ledger and settlement history.
- Bank payout status tracking and downloadable tax invoices.

### E. Team & Staff Role-Based Access Control (RBAC)
- Staff invitation by email/phone.
- Granular permission levels:
  - **Owner**: Full access including bank details, staff management, and business settings.
  - **Manager**: Menu pricing, operating hours, order management, performance analytics.
  - **Kitchen / Cashier**: Active order view, order state transitions, receipt printing (no financial access).
  - **Driver / Courier**: Assigned order details, customer contact, and delivery completion.

---

## 4. Multi-Tenant Data Isolation

- Partner isolation is enforced via database policies using `partner_id` foreign keys.
- Authenticated requests resolve the caller's authorized partners via `partner_members`.
- Under no circumstances can Partner A read orders, customer details, or financials of Partner B.
