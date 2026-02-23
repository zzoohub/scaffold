# CLAUDE.md
This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview
<!-- TODO: Describe what this project does, who it's for, and core value proposition -->

## Architecture
<!-- TODO: Describe high-level architecture (e.g., "Axum API + Next.js web client + background workers") -->

### Monorepo Structure
```
├── services/
│   ├── api/              # Backend API
│   └── worker/           # Queue, cron, pub/sub handlers
├── clients/
│   ├── web/              # Next.js frontend
│   └── mobile/           # Expo React Native
├── db/
│   ├── migrations/
│   └── seeds/
├── infra/                # Pulumi IaC
├── tasks/                # todo.md, lessons.md (Claude Code workspace)
├── docs/                 # Product planning (what and how to build)
├── biz/                  # Business operations (how to sell & grow)
└── scripts/
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

## Principles & Constraints
### MUST
1. All changes must use skills, including after plan mode.
2. After implementation, check if sub-agents are needed and run in parallel:
   - **z-security-reviewer**: logic change only (API, auth, data, infra)
   - **z-tester**: testable code change (new/modified functions, branches)
   - **z-verify-app**: any app-affecting change (typecheck → test → browser)
   > Skip all for docs/copy-only changes. Skip browser test if `/chrome` unavailable.
3. Marketing content must reference `docs/product-brief.md` for consistent messaging.
4. All user-facing events must be defined in `biz/analytics/tracking-plan.md` before implementation.
5. Any change to architecture or feature specs must update the relevant docs in `docs/`.
6. When the user corrects you, log the lesson to `tasks/lessons.md` with a rule to prevent recurrence.
7. Read `tasks/lessons.md` at every session start.

### MUST NOT
- (project-specific anti-patterns here)

### Before Completion Checklist
- [ ] Diff behavior between main and your changes when relevant
- [ ] Ask yourself: "Would a staff engineer approve this?"

### Complex Tasks (5+ steps)
1. Write plan to `tasks/todo.md` with checkable items
2. Verify plan with the user before starting implementation
3. Track progress — mark items complete as you go

## Domain Glossary
<!-- TODO: Define domain-specific terms (e.g., "Workspace", "Member", "Plan") -->

## Build & Dev Commands
All commands are in `justfile`. Run `just --list` to see available recipes.

## API
### API Workflow (MUST FOLLOW)
- Schema changes: **z-database-design** → **z-api-design** (plan)
- Default: **z-axum-hexagonal** + **z-postgresql** (queries)
<!-- NOTE: If using FastAPI, replace z-axum-hexagonal with appropriate sub-agent -->

### API Conventions
<!-- TODO: Define API conventions (e.g., error format, auth strategy, pagination style) -->

## Worker
<!--> TODO: If worker is needed -->

## Web
### Web Workflow (MUST FOLLOW)
- UI components: **z-ui-engineer** Agent
- Default: **z-nextjs** (implementation) → **vercel-react-best-practices** (review)

### FSD Import Rules
- `app(routing) → views → widgets → features → entities → shared` (never import upward)

### Web Conventions
- **i18n**: Use `next-intl` for all UI text (Korean/English).
- **Responsive**: Support all screen sizes.
- **Dark mode**: Support light and dark themes.
<!-- TODO: Add project-specific UI conventions -->

## Mobile
### Mobile Workflow
- UI components: **z-ui-engineer** Agent
- Default: **z-react-native**
<!-- NOTE: Add or replace sub-agent as needed -->

### Mobile Conventions
- **Navigation**: Expo Router (file-based routing).
- **i18n**: Share translation keys with web where possible.
- **Dark mode**: Support light and dark themes.
<!-- TODO: Add project-specific mobile conventions -->
