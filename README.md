# Pocket Concierge — Platform Monorepo

**An open-source, AI-first super-app platform** connecting consumers and local businesses across dining, grocery, retail, hospitality, mobility, and on-demand services.

---

## Platform Scope & Status

The platform encompasses **Consumer Applications**, **Partner/Merchant Applications**, **Web Applications**, **Payment Infrastructure**, **Partner Payout Systems**, and a **Centralized Shared Backend**.

| Component | Platform / Tech | Status | Location |
|---|---|---|---|
| **Web Prototype** | Next.js 16 · React · TypeScript · Tailwind | ✅ **Working** — Visual, UX & functional reference | [`prototype/web/`](./prototype/web/) |
| **Consumer Android** | Kotlin · Jetpack Compose | 🔮 Planned | [`apps/consumer/android/`](./apps/consumer/android/) |
| **Consumer iOS** | Swift · SwiftUI | 🔮 Planned | [`apps/consumer/ios/`](./apps/consumer/ios/) |
| **Consumer Web** | Next.js · React · TypeScript | 🔮 Planned | [`apps/web/consumer/`](./apps/web/consumer/) |
| **Partner Android** | Kotlin · Jetpack Compose | 🔮 Planned | [`apps/partner/android/`](./apps/partner/android/) |
| **Partner iOS** | Swift · SwiftUI | 🔮 Planned | [`apps/partner/ios/`](./apps/partner/ios/) |
| **Partner Web Portal** | Next.js · React · TypeScript | 🔮 Planned | [`apps/web/partner/`](./apps/web/partner/) |
| **Shared Backend** | Supabase (PostgreSQL, Auth, RLS, Edge Fns) | 🔮 Planned (Configured) | [`supabase/`](./supabase/) |

> **Crucial Distinction**:
> - [`prototype/web/`](./prototype/web/) is the **current working reference prototype**. It implements the complete multi-category catalog, voice AI concierge, E2E encrypted chat, WebRTC calling, and ride/order simulation in the browser.
> - [`apps/`](./apps/) contains the **future production client applications** for both consumers and business partners.

---

## Target System Architecture

```
CONSUMER APPS                       PARTNER APPS
Android (Kotlin + Compose)          Android (Kotlin + Compose)
iOS (Swift + SwiftUI)               iOS (Swift + SwiftUI)
Web (Next.js + React)               Web Portal (Next.js + React)
        │                                   │
        └─────────────────┬─────────────────┘
                          │
                          ▼
            CENTRALIZED SUPABASE BACKEND (supabase/)
            ├── Supabase Auth (Consumer & Partner RBAC)
            ├── PostgreSQL with Row Level Security (RLS)
            ├── Supabase Realtime (Order Tracking & Messaging)
            └── Supabase Edge Functions (Deno / TypeScript)
                    ├── Payment Abstraction (eSewa, Khalti, Fonepay, Card)
                    ├── Partner Settlement & Payout Engine
                    └── AI Concierge Reasoning Loop
```

---

## Repository Structure

```
Elifsi-app/
│
├── .github/
│   ├── workflows/             # CI workflow (lint, typecheck, tests, build)
│   ├── ISSUE_TEMPLATE/        # Bug report and feature request templates
│   ├── PULL_REQUEST_TEMPLATE.md
│   └── CODEOWNERS             # @Elifsi default review ownership
│
├── prototype/
│   └── web/                   # Existing Next.js working prototype (reference)
│
├── apps/
│   ├── consumer/
│   │   ├── android/           # Kotlin + Jetpack Compose consumer app
│   │   └── ios/               # Swift + SwiftUI consumer app
│   │
│   ├── partner/
│   │   ├── android/           # Kotlin + Jetpack Compose partner/POS/rider app
│   │   └── ios/               # Swift + SwiftUI partner/KDS app
│   │
│   └── web/
│       ├── consumer/          # Production consumer web portal
│       └── partner/           # Production partner merchant dashboard
│
├── supabase/
│   ├── migrations/            # Version-controlled database migrations
│   ├── functions/             # Server-side Edge Functions
│   ├── seed.sql               # Local development seed data
│   └── config.toml            # Local Supabase CLI configuration
│
├── docs/
│   ├── architecture/          # System, Consumer, Partner, Payments, Payouts
│   ├── product/               # Product requirements (Consumer & Partner)
│   ├── development/           # Local development workflows
│   └── roadmap/               # Phased delivery roadmap
│
├── scripts/                   # Repository automation scripts
├── .env.example               # Root environment variable documentation
├── .gitignore
├── README.md
├── ARCHITECTURE.md
├── CONTRIBUTING.md
├── SECURITY.md
├── CODE_OF_CONDUCT.md
└── LICENSE
```

---

## Quick Start — Running the Prototype

The interactive prototype is fully functional and ready to run locally:

```bash
cd prototype/web
npm install
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

### Optional AI Provider Setup
To run with live LLM tool-calling (Gemini, Claude, or OpenRouter):
1. Copy `.env.example` to `prototype/web/.env.local`
2. Add your `GEMINI_API_KEY`, `ANTHROPIC_API_KEY`, or `OPENROUTER_API_KEY`
3. Restart `npm run dev`

### Encrypted Chat & WebRTC Calls
```bash
cd prototype/web
npm run dev:all    # Starts Next.js (port 3000) and WebSocket relay (port 8787)
```
Open two browser windows at `http://localhost:3000/chat` to pair identities.

### Prototype Test Suite
```bash
cd prototype/web
npm run lint         # ESLint check
npx tsc --noEmit     # TypeScript typecheck
npm run test         # Vitest unit + integration tests (75 passing tests)
npm run build        # Production Next.js Turbopack build
```

---

## Payment & Partner Payout Subsystems

The platform integrates an extensible financial architecture:

1. **Payment Abstraction Layer**:
   - Consumers pay via **eSewa**, **Khalti**, **Fonepay**, or **Card Gateways**.
   - Client applications never handle secret keys or calculate final charges.
   - Payments are initiated, verified, and settled server-side via Supabase Edge Functions.

2. **Partner Payout & Settlement Engine**:
   - Financial separation between consumer collection and partner disbursement.
   - Server-side accounting: Gross Order Value minus platform commission/tax equals net partner entitlement.
   - Payout batches disbursed to partner bank accounts / wallets with full audit logs.

For detailed specifications, see:
- [Payment Architecture](docs/architecture/payments.md)
- [Partner Payout & Financial Architecture](docs/architecture/payouts.md)

---

## Documentation

Full architectural, product, and engineering documentation is available in [`docs/`](./docs/):
- [`docs/architecture/system-overview.md`](./docs/architecture/system-overview.md)
- [`docs/architecture/consumer.md`](./docs/architecture/consumer.md)
- [`docs/architecture/partner.md`](./docs/architecture/partner.md)
- [`docs/architecture/payments.md`](./docs/architecture/payments.md)
- [`docs/architecture/payouts.md`](./docs/architecture/payouts.md)
- [`docs/product/consumer/README.md`](./docs/product/consumer/README.md)
- [`docs/product/partner/README.md`](./docs/product/partner/README.md)
- [`docs/development/README.md`](./docs/development/README.md)
- [`docs/roadmap/README.md`](./docs/roadmap/README.md)

---

## Contributing & Security

- Contributions: See [`CONTRIBUTING.md`](./CONTRIBUTING.md)
- Security Policy: See [`SECURITY.md`](./SECURITY.md)
- Code of Conduct: See [`CODE_OF_CONDUCT.md`](./CODE_OF_CONDUCT.md)

---

## License

[MIT](./LICENSE) © 2026 Elifsi Technologies Private Limited
