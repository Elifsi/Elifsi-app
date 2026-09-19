# apps/web/partner — Production Partner Web Portal & Dashboard

## Status

🔮 **Planned — Implementation not yet initialized.**

## Technology Stack

- **Framework**: Next.js (App Router)
- **Language**: TypeScript
- **UI & Styling**: React, Tailwind CSS, Data Tables, Charts (Recharts / Tremor)
- **Backend SDK**: `@supabase/ssr` / `@supabase/supabase-js`
- **Authentication**: Supabase Auth with Partner Role Verification & Staff RBAC

## Core Responsibilities

- Desktop & tablet-optimized web dashboard for business owners and store managers.
- Multi-location business profile and outlet management.
- Comprehensive catalog management: bulk product CSV import/export, pricing matrix, menu builders, modifier groups, and inventory counts.
- Live order dispatch console with audio chime alerts for busy counter environments.
- Financial management: sales analytics, commission breakdowns, settlement history, downloadable invoices, and bank payout requests.
- Staff team management: invite members, configure permission tiers (e.g. Owner, Store Manager, Cashier, Analyst).

> **Security Reminder**: Never expose `SUPABASE_SERVICE_ROLE_KEY` in client bundles. Access control to partner data is strictly enforced by PostgreSQL Row Level Security based on authenticated partner user IDs.
