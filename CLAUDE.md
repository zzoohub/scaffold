# CLAUDE.md
This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview
<!-- Describe what this project does, who it's for, and core value proposition -->

## Rules (STRICTLY ENFORCED — NO EXCEPTIONS)

1. **TDD**: NEVER write implementation before tests. Tests first → red → implement → green. If you catch yourself writing implementation first, STOP and write the test first.
2. **Review once per task**: Run **reviewer** + **verifier** in parallel after all TDD passes. Fix → re-run → all pass, then commit. When in doubt, run. Skip only for cosmetic changes (styling, typos, docs).
3. **Docs sync**: Changes to requirements, scope, architecture, data model, UX/UI → update `docs/`.

<!-- Add project test runner here (e.g., nextest, vitest, pytest) -->

---

## Development Loop

Each session = one task from `TASKS.md`.

### Step 1: Pick a task
Open `TASKS.md`. Find the next unchecked task. Read `docs/prd/features/{feature}.md` for context.

### Step 2: TDD
Write ALL tests first → confirm FAIL → implement → confirm ALL PASS.

### Step 3: Review
- **reviewer** — security (OWASP) + code quality (2-pass: CRITICAL / INFORMATIONAL)
- **verifier** — browser QA (browse binary) + E2E tests

### Step 4: Done
Check off task in `TASKS.md` → commit → push.

---

## Architecture
<!-- Describe high-level architecture (e.g., "Axum API + Next.js web client + background workers") -->

### Monorepo Structure
```
├── apps/
│   ├── api/              # Backend API → Cloud Run
│   ├── worker/           # Background jobs → Cloud Run Jobs / CF Worker
│   ├── web/              # Web client → CF Pages / Vercel
│   └── mobile/           # Expo React Native
├── db/
│   ├── migrations/
│   ├── rollbacks/
│   └── seeds/
├── e2e/                  # End-to-end tests
├── docs/
│   ├── prd/
│   │   ├── product-brief.md    # Strategic one-pager
│   │   ├── prd.md              # Vision + tech stack + dev order
│   │   └── features/           # Feature specs (requirements, journeys)
│   ├── ux/
│   │   ├── ux-design.md        # UX overview (IA, navigation, global patterns)
│   │   └── screens/            # Per-screen UX specs
│   ├── arch/
│   │   ├── context.md            # Problem, ASRs, domain model
│   │   ├── design.md             # Patterns, components, data, deployment
│   │   └── decisions.md          # ADRs, risk register, tech debt
│   └── database-design.md      # DB schema design
├── biz/                  # Business operations (how to sell & grow)
├── TASKS.md              # Progress tracking (all features)
└── justfile              # dev, test, deploy commands
```

## Environment

| Service | Dev                         | Prod |
|---------|-----------------------------|------|
| Web     | `localhost:3000`            | TBD  |
| Mobile  | Expo Dev Client (`19000`)   | TBD  |
| API     | `localhost:8080`            | TBD  |
| Worker  | `localhost:8081~`           | TBD  |
| DB      | Docker Compose (`5432`)     | TBD  |
| Redis   | Docker Compose (`6379`)     | TBD  |

## Build & Dev Commands
All commands in `justfile`. Run `just --list` to see all recipes.

## API
### API Workflow (MUST FOLLOW)
- Schema changes: **database-design** → **rest-api-design** (plan)
- Default: **axum-hexagonal** + **postgresql** (queries)
<!-- If using FastAPI, replace axum-hexagonal with appropriate sub-agent -->

### API Conventions
<!-- Define API conventions (e.g., error format, auth strategy, pagination style) -->

## Worker
<!-- If worker is needed -->

## Web
### Web Workflow (MUST FOLLOW)
- Design system: **design-system**
- UI: **frontend-design**
- Web source code:
  - If **TanStack Start (SolidJS)** → **solidjs**
  - If **Next.js** → **vercel-composition-patterns** (composition) → **vercel-react-best-practices** (optimization)
  

### FSD Import Rules
- `app(routing) → views → widgets → features → entities → shared` (never import upward)

### Web Conventions
- **Type safety**: Enforce the strictest TypeScript compiler options.
- **I18n**: All user-facing text must support en and ko
- **Responsive**: Support all screen sizes.
- **Dark mode**: Support light and dark themes.
<!-- Add project-specific UI conventions -->

## Mobile
### Mobile Workflow
- Design system: **design-system**
- Mobile source code: **expo-app-design:building-native-ui** (implementation) → **vercel-react-native-skills** (improve)
<!-- Add or replace sub-agent as needed -->

### Mobile Conventions
- **Type safety**: Enforce the strictest TypeScript compiler options.
- **I18n**: All user-facing text must support en and ko.
- **Dark mode**: Support light and dark themes.
<!-- Add project-specific mobile conventions -->
