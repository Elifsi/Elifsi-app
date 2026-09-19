# supabase/functions/ — Supabase Edge Functions

This directory contains server-side TypeScript Edge Functions running on the Deno runtime for privileged operations and external service integrations.

## Status

🔮 **No Edge Functions yet — will be implemented as production integrations commence.**

---

## Planned Edge Functions

1. **`payment-initiate/`**:
   - Implements the Payment Service abstraction dispatching to provider adapters (`eSewa`, `Khalti`, `Fonepay`, and optional dedicated `Card Gateway`).
   - Handles card payments through provider checkout where facilitated (e.g. Khalti Checkout) without unnecessary duplication, while remaining extensible for dedicated card processors.
   - Calculates immutable order totals server-side (never trusts client-provided amounts).
   - Generates and signs gateway session tokens, QR payloads, or redirect URLs with nonces.

2. **`payment-verify/`**:
   - Webhook and callback receiver for payment gateway notifications.
   - Cryptographically verifies gateway signatures and IP addresses.
   - Updates `payments` table status and triggers order status advancement.

3. **`payout-execute/`**:
   - Executes partner payout batches to banks / partner wallets.
   - Restricted to automated settlement schedules or platform admin authorization.
   - Records provider reference numbers and reconciliation tokens.

4. **`ai-concierge-chat/`**:
   - Server-side tool execution loop for conversational catalog querying and order drafting.
   - Protects upstream AI provider API keys (Gemini, Anthropic, OpenRouter).

5. **`push-notify/`**:
   - Sends critical order notifications to mobile clients via FCM (Android) and APNs (iOS).

---

## Edge Function Security Rules

- **Secrets Management**: Secrets are injected via `supabase secrets set <KEY>=<VALUE>`. They are NEVER checked into Git.
- **Service Role Key Usage**: The `SUPABASE_SERVICE_ROLE_KEY` is only used within Edge Functions when cross-table or administrative operations are required.
- **Payload Validation**: All incoming requests are validated against strict Zod or JSON schemas before processing.
- **Idempotency**: Payment and payout webhooks must implement idempotency keys to prevent double-charging or double-disbursement.
