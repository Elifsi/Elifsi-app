# Development Guide

This guide covers local environment setup, standards, and workflows for contributing across the repository.

---

## 1. Prerequisites

- **Node.js**: >= 22.22.2
- **npm**: >= 10.0.0
- **Supabase CLI**: >= 1.150.0 (optional for local backend development)
- **Android Studio / Kotlin**: (for upcoming `apps/consumer/android/` and `apps/partner/android/`)
- **Xcode / Swift**: (for upcoming `apps/consumer/ios/` and `apps/partner/ios/`)

---

## 2. Working with the Prototype

The interactive prototype lives in `prototype/web/`. All prototype commands must run inside that directory:

```bash
cd prototype/web
npm install
npm run dev           # Run Next.js on port 3000
npm run chat-server   # Run WebSocket relay on port 8787
npm run dev:all       # Run both concurrently
```

### Pre-PR Verification for Prototype Changes

```bash
cd prototype/web
npm run lint          # ESLint check
npx tsc --noEmit      # TypeScript typecheck
npm run test          # Vitest unit & integration tests
npm run build         # Next.js production build
```

---

## 3. Working with Supabase (Local Backend)

```bash
# Start local Supabase containers (Docker required)
supabase start

# Apply pending migrations
supabase db push

# Reset local database and re-seed from supabase/seed.sql
supabase db reset

# Serve Edge Functions locally
supabase functions serve
```

---

## 4. Code Standards & Git Hygiene

- **Domain Scoping**: Keep modules small and domain-isolated.
- **Security Boundaries**: Never commit private keys, service-role tokens, or API credentials.
- **Testing**: Write unit and integration tests alongside code.
- **Branching**: Branch from `main`, open focused PRs with clear descriptions.
