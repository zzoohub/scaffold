# Tech Stack

Cloudflare-first. Unified billing, single CLI, one dashboard.

---

## Platform

| Layer | Default | Escape Hatch |
|---|---|---|
| Compute | Workers + CF Containers | GCP Cloud Run (GPU) |
| Frontend | TanStack Start (React or SolidJS) | — |
| DB (Global) | Neon + Hyperdrive | — |
| DB (Korea) | Supabase + Hyperdrive | — |
| CDN / DNS | Cloudflare | — |

---

## Backend

| Framework | Runtime | When |
|---|---|---|
| **Hono (TS)** | Workers | Default |
| **Axum (Rust)** | CF Containers / Cloud Run | CPU-intensive |
| FastAPI (Python) | CF Containers / Cloud Run | Python-only ML libs |

### TS Ecosystem
Hono · Drizzle ORM · Zod · Hono RPC · native `fetch`

### Rust Ecosystem
Axum · SQLx · serde · Tokio · reqwest · validator

---

## Frontend

| Framework | When |
|---|---|
| **TanStack Start (React)** | Default |
| **TanStack Start (SolidJS)** | Bundle size / fine-grained reactivity |

---

## Mobile

React Native (Expo) · Expo Router · TanStack Query · EAS Build/Submit/Update

---

## Auth

| Layer | Choice |
|---|---|
| Authentication | **Better Auth** (TS-native, self-hosted) |
| Social (Global) | Google OAuth2 |
| Social (Korea) | Kakao + Naver + Google |
| B2B | Email magic link |
| Authorization | RBAC (default) · ABAC · ReBAC |

Token: short-lived access (15min) + httpOnly refresh (90d web / 1y mobile) + rotation 필수.

Rust backend without TS frontend → **Clerk** or direct OAuth2 + JWT.

---

## Container Runtime

| Runtime | When |
|---|---|
| **CF Containers** | Default (sleep/wake, unified billing) |
| **Cloud Run** | GPU, GCP services |

---

## Supporting Services

| Role | Choice |
|---|---|
| Error Tracking | Sentry |
| Observability | Axiom |
| Uptime | BetterStack |
| Analytics | PostHog |
| Payments (Global) | Stripe |
| Payments (Korea) | Toss Payments |
| CI/CD | GitHub Actions + Workers Builds |

---

## Monorepo

**Bun workspaces**. No Turborepo/Nx.

```
project/
├── packages/
│   ├── api/       # Hono (Workers)
│   ├── web/       # TanStack Start
│   ├── shared/    # Types, utils, validation
│   └── mobile/    # Expo (optional)
```

Hono RPC로 backend↔frontend 타입 공유.

---

## Region

| Audience | Strategy |
|---|---|
| Global (Virginia) | Workers + Neon |
| Korea | Workers + Supabase via Hyperdrive |
