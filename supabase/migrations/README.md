# supabase/migrations/ — Database Schema Migrations

This directory contains version-controlled, forward-only SQL migrations managing the database schema, indexes, and Row Level Security (RLS) policies for the Pocket Concierge platform.

## Status

🔮 **No migrations yet — schema will be created when backend implementation starts.**

---

## Planned Domain Migrations

When implementation begins, migrations will be created incrementally for the following domains:

1. **Identity, Auth & Roles**:
   - `profiles`: Consumer profile metadata linked to `auth.users(id)`.
   - `partner_profiles`: Business registration, KYC status, business category, contact details.
   - `partner_members`: Association table granting staff roles (`owner`, `manager`, `staff`, `cashier`, `driver`) to authenticated users.

2. **Catalog & Inventory**:
   - `categories`, `catalog_items`, `item_variants`, `item_modifiers`.
   - Partner catalog partitioning and stock availability flags.

3. **Orders & Lifecycle**:
   - `orders`: Unique order identifiers, consumer ID, partner ID, fulfillment status, delivery address, timestamps.
   - `order_items`: Snapshot of items, selected variants, unit price, quantity at time of purchase.
   - `order_status_logs`: Auditable state transitions (`created`, `acknowledged`, `in_preparation`, `ready`, `dispatched`, `completed`, `cancelled`).

4. **Payments & Financial Ledger**:
   - `payments`: Consumer payment attempts, gateway provider (`esewa`, `khalti`, `fonepay`, `card`), gateway transaction ID, status (`pending`, `completed`, `failed`, `refunded`).
   - `transactions`: Immutable transaction ledger recording every monetary movement.
   - `refunds`: Refund tracking linked to original payment and order.

5. **Partner Settlement & Payouts**:
   - `partner_earnings`: Granular record per fulfilled order calculating gross order value, platform fee/commission, taxes, and net payable.
   - `settlements`: Settlement batch records summarizing net earnings across a settlement cycle.
   - `payouts`: Bank / wallet payout records containing payout reference, destination bank account, transfer status, and payout provider response.

6. **Communication & Location**:
   - `conversations`, `messages`: Direct communication records.
   - `driver_locations`: Real-time geospatial coordinates for active delivery and mobility trips.

---

## Migration Rules

- **Timestamped naming**: `YYYYMMDDHHMMSS_description.sql`.
- **Forward-only**: Never modify committed migration files.
- **Mandatory RLS**: Every single table created MUST run:
  ```sql
  ALTER TABLE public.<table_name> ENABLE ROW LEVEL SECURITY;
  ```
  with explicit policies for `SELECT`, `INSERT`, `UPDATE`, and `DELETE`.
- **Tenant & User Isolation**:
  - Consumers can only read/update their own profile, orders, and addresses.
  - Partners can only read/update their own catalog, orders, and business earnings.
  - Financial payout records are strictly restricted to partner owners/managers and platform administrators.
