# apps/partner/ — Partner Applications

The **Partner Application** is designed for businesses, service providers, and gig operators participating in the Pocket Concierge platform.

> **Important**: Partner applications are distinct products from consumer applications. They are designed around specific business roles, workflows, and operational needs rather than customer shopping flows.

---

## Supported Partner Types

The platform supports diverse partner domains with tailored workflows:
- **Food & Dining**: Restaurants, cafes, cloud kitchens (menu editing, order prep queue, kitchen display).
- **Retail & Grocery**: Supermarkets, specialty stores (inventory tracking, barcode lookup, dispatch).
- **Hospitality**: Hotels, resorts, serviced apartments (room inventory, check-in schedules, rate calendars).
- **Mobility & Logistics**: Drivers, riders, couriers (trip dispatch, route navigation, live trip completion).
- **Professional Services**: Cleaners, technicians, consultants (appointment scheduling, task logging).

---

## Core Architectural Capabilities

1. **Onboarding & Verification**:
   - Business registration, KYC/legal document upload, tax/VAT details, bank account verification.
   - Multi-stage onboarding status (pending review, verified, suspended, active).

2. **Catalog & Inventory Management**:
   - Product/service catalog creation, categorization, multi-variant options, and modifier groups.
   - Dynamic pricing rules, discount schedules, and real-time item availability toggling.
   - Stock level tracking and low-inventory alerts where applicable.

3. **Order Fulfillment Workflow**:
   - Real-time incoming order sound/push alerts.
   - Accept, reject (with structured reason codes), and estimated preparation time setting.
   - Order lifecycle progression: `acknowledged` → `preparing` → `ready_for_pickup` → `dispatched` → `delivered`.
   - Customer and delivery partner handoff coordination.

4. **Earnings, Settlement & Payouts**:
   - Granular gross revenue, net earnings, platform commission deductions, and adjustments ledger.
   - Scheduled bank settlement and automated payout status tracking.
   - Downloadable financial statements and tax summaries.

5. **Analytics & Operations**:
   - Real-time sales dashboard, top-performing items, peak order hours, customer ratings/feedback.

6. **Team & Staff Access Control**:
   - Role-based access control (RBAC): Owner, Store Manager, Kitchen Staff, Cashier, Driver.
   - Scoped permissions preventing unauthorized access to financial and bank payout records.

---

## Platforms

- **Android**: [`apps/partner/android/`](./android/) (Kotlin + Jetpack Compose)
- **iOS**: [`apps/partner/ios/`](./ios/) (Swift + SwiftUI)
- **Web Dashboard**: [`apps/web/partner/`](../web/partner/) (Next.js + React)
