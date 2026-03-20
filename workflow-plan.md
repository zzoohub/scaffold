# Workflow — Plan

## 0. Validate

Write one hypothesis. Build the minimum test in hours. Ship to real users.
Move on when you see organic pull. Kill after 2–3 pivots with no signal.

---

## 1. Plan

```
product-manager (Mode A = Full Planning)
  → product-brief                   → docs/prd/product-brief.md
  → prd-craft                       → docs/prd/prd.md + docs/prd/features/*.md

plan-ceo-review                     → "Is this the right direction?" (once per PRD)
```

---

## 2. Design → Architecture

```
ux-designer                           → docs/ux/
  ↓
architect (Mode A = Full Architecture)→ docs/arch/ + docs/arch/database.md + db/migrations/
  ↓
data-analyst                          → biz/analytics/
  ↓
plan-eng-review                       → "Is the architecture solid?" (once, after design)
```

### Checklist
- [ ] UX: IA + key screen specs (`docs/ux/screens/`)
- [ ] Architecture: `docs/arch/context.md`, `system.md`, `decisions.md`
- [ ] Database: `docs/arch/database.md` + `db/migrations/`
- [ ] plan-eng-review passed
- [ ] Tracking plan: events, funnels, dashboards, kill criteria

---

## 3. Tasks

After design is finalized, generate the task breakdown.

```
task-manager (Mode A = Generate Tasks) → tasks/board.md + tasks/features/*.md
```

Output structure:
```
tasks/
├── board.md              # Phase-based status table (orchestrator reads this)
├── features/
│   ├── <feature>.md      # Full task details per feature
│   └── ...
```

- `tasks/board.md` tracks state (phase, status, assignee, touches). Changes often.
- `tasks/features/*.md` hold context (depends_on, acceptance, context). Change rarely.
- Within a phase: all tasks run in parallel. Between phases: sequential.
- Each task has a `touches` field listing files it modifies (prevents agent conflicts).
