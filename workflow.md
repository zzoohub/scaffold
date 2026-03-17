# Workflow

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

- `board.md` tracks state (phase, status, assignee, touches). Changes often.
- `features/*.md` hold context (depends_on, acceptance, context). Change rarely.
- Within a phase: all tasks run in parallel. Between phases: sequential.
- Each task has a `touches` field listing files it modifies (prevents agent conflicts).

---

## 4. Build (phase-based loop)

Process one phase at a time. Within each phase, tasks run in parallel.

```
board.md → pick phase with remaining backlog tasks
  ↓
Create team — route tasks to domain agents by `touches` path:
  apps/api/, apps/worker/, db/ → backend-developer
  apps/web/                    → frontend-developer
  apps/mobile/                 → mobile-developer
  apps/desktop/                → desktop-developer
  ↓
Each teammate (same repo, no worktree):
  Read tasks/features/{feature}.md for task context
  ↓
  TDD (write tests → fail → implement → pass)
  ↓
  reviewer + verifier (once per task, parallel)
    ├─ reviewer: security (OWASP) + code quality (2-pass)
    └─ verifier: qa skill (browser) + E2E tests
  ↓
  Fix if needed → re-run → all pass
  ↓
  Update board.md status → done → commit → push

Side duties (not routed as build tasks):
  docs/  → plan agents (product-manager, architect, ux-designer) update on plan change
  tasks/ → developers update board.md per task; task-manager maintains structure
```

Phase N+1 starts after all Phase N tasks are done.
Same repo — `touches` field is the only file conflict guard. No worktree isolation.
Unit + integration tests via TDD. E2E via verifier per task.
Exception: manually test payment flows before launch.

---

## 5. Deploy

Direct CLI deploy per service (`wrangler`, `gcloud`, `vercel`, `eas`). Deploy recipes in `justfile`.

### Setup checklist
- [ ] Each service deploy command in `justfile`
- [ ] Secrets configured per platform (`.env.example` documents what's needed)
- [ ] DB migrations run before deploy
- [ ] Mobile: EAS project + build profiles

### First deploy checklist
- [ ] Staging → smoke test → Production
- [ ] PostHog events firing
- [ ] Sentry connected
- [ ] Aha Moment event works end-to-end

---

## 6. Launch → Marketing

Use **marketer** agent → `biz/marketing/strategy.md`, `launch/`, `pricing.md`, `competitors.md`
Use **content-marketer** agent → `biz/marketing/content/`, `biz/marketing/assets/`

### Checklist
- [ ] Strategy: positioning, audience, channels (marketer)
- [ ] Pricing: tiers and packaging (marketer)
- [ ] Launch materials: PH, HN, Reddit drafts (marketer)
- [ ] Content backlog: social, email, blog (content-marketer)
- [ ] SEO: sitemap, meta, OG, robots.txt (content-marketer)
- [ ] Legal: ToS, Privacy Policy
- [ ] Assets: OG image, screenshots, demo

### Launch day
- [ ] Post per `biz/marketing/launch/checklist.md`
- [ ] Respond to all comments within 2 hours
- [ ] Log feedback → `biz/ops/feedback-log.md`

---

## Operate (ongoing from launch)

Runs in parallel — not a sequential step.

- Daily: check feedback → `biz/ops/feedback-log.md`
- Same question 3+ times → update FAQ
- Incidents → follow `biz/ops/runbook.md`
- Ongoing content: use **content-marketer** → `biz/marketing/content/`
- Feed insights back into step 1, 4, or 8

---

## 7. Measure → Decide

Check dashboard every morning (5 min). First Kill/Keep/Scale call at week 2.

Use **data-analyst** agent → `biz/analytics/reports/`.

**Week 1:** Observe. Respond to feedback. Don't optimize yet.
**Week 2:** First Kill/Keep/Scale evaluation.
**After:** Re-evaluate weekly until decision is clear.

Fix in this order: **Retention → Activation → Acquisition**

- **Kill** → Post-mortem, archive, move on same day.
- **Keep** → Back to step 1 or 4. Fix the biggest problem.
- **Scale** → Continue to step 8.

---

## 8. Grow → Optimize

Only after Scale. Don't grow a leaky product.

Use **growth-optimizer** agent → `biz/growth/`.

### Checklist
- [ ] ICE-scored experiment backlog
- [ ] Referral / viral loop design
- [ ] Churn prevention: cancel flow, save offers, dunning
- [ ] CRO: per-page/flow optimization
- [ ] E2E for critical paths (signup → Aha Moment → payment)
- [ ] E2E integrated into CI pipeline
