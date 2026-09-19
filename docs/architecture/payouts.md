# Partner Payout & Financial Architecture

Payments and partner payouts are strictly separated subsystems in the Pocket Concierge platform.

When a consumer pays for an order, the funds are collected into the platform's escrow/clearing accounts. The platform accounts for partner earnings, deducts platform commission, handles adjustments, and subsequently settles net funds to partners according to defined settlement cycles.

---

## 1. Payout Subsystem Topology

```
                  ┌────────────────────────────────────────┐
                  │            Consumer Payment            │
                  │   (eSewa / Khalti / Fonepay / Card)    │
                  └───────────────────┬────────────────────┘
                                      │ Verified by Edge Function
                                      ▼
                  ┌────────────────────────────────────────┐
                  │       Payment & Ledger Subsystem       │
                  │       (Records immutable debit)        │
                  └───────────────────┬────────────────────┘
                                      │
                                      ▼
                  ┌────────────────────────────────────────┐
                  │            Partner Earnings            │
                  │  Gross Order - Platform Fee = Net Due  │
                  └───────────────────┬────────────────────┘
                                      │ Scheduled Settlement Cycle
                                      ▼
                  ┌────────────────────────────────────────┐
                  │           Settlement Engine            │
                  │   (Aggregates earnings & adjustments)  │
                  └───────────────────┬────────────────────┘
                                      │ Generates payout batch
                                      ▼
                  ┌────────────────────────────────────────┐
                  │             Partner Payout             │
                  │      (Disbursement & Bank Transfer)    │
                  └───────────────────┬────────────────────┘
                                      │
                                      ▼
                  ┌────────────────────────────────────────┐
                  │    Bank / IPS / Payout Rail Adapter    │
                  │ (NCHL-IPS, ConnectIPS, Direct Bank API)│
                  └────────────────────────────────────────┘
```

---

## 2. Core Financial Data Entities

To maintain strict auditability, the system separates financial concepts into dedicated entities rather than combining them into a generic "payment" table:

| Entity | Purpose | Key Attributes |
|---|---|---|
| **`orders`** | Business transaction describing purchased items | `id`, `consumer_id`, `partner_id`, `subtotal`, `status` |
| **`payments`** | Consumer payment transaction attempt | `id`, `order_id`, `provider`, `gateway_ref`, `amount`, `status` |
| **`transactions`** | Immutable double-entry financial ledger | `id`, `account_id`, `type` (`credit`/`debit`), `amount`, `reference_id` |
| **`partner_earnings`** | Earning record calculated per completed order | `order_id`, `gross_amount`, `platform_fee`, `tax`, `net_payable` |
| **`refunds`** | Partial or full reversal of a payment | `payment_id`, `amount`, `reason`, `processed_at`, `status` |
| **`settlements`** | Periodic aggregation of unpaid partner earnings | `partner_id`, `cycle_start`, `cycle_end`, `total_amount`, `status` |
| **`payouts`** | Actual disbursement transaction sent to partner bank | `settlement_id`, `bank_account_id`, `payout_ref`, `status` |

---

## 3. Financial Calculation Principles

1. **Server-Side Exclusivity**:
   - Financial calculations (commission deductions, VAT withholding, net earning calculations) occur exclusively in PostgreSQL stored procedures or Supabase Edge Functions.
   - Client applications never compute partner earnings or apply fee percentages locally.
2. **Auditability & Immutability**:
   - Financial ledger entries cannot be modified or deleted (`UPDATE` and `DELETE` privileges are revoked; corrections require compensating journal entries).
3. **Dispute & Refund Accounting**:
   - When a refund is granted, the settlement engine deducts the refunded amount from the partner's pending settlement balance or generates a negative adjustment entry.
4. **Disbursement Decoupling**:
   - Settlement batches can be reviewed by finance administrators prior to execution or automated based on partner trust tier.
   - Payout gateway failures (e.g. incorrect account number, bank network downtime) mark the payout as `failed` and return the balance to `unsettled` without modifying historical order records.
