# Workflow

## 0. Validate

Write one hypothesis. Build the minimum test in hours. Ship to real users.
Move on when you see organic pull. Kill after 2–3 pivots with no signal.

---

## 1. Plan

```
product-manager (Mode A)
  → product-brief                   → docs/prd/product-brief.md
  → prd-craft                       → docs/prd/prd.md + docs/prd/features/*.md

plan-ceo-review                     → "Is this the right direction?" (once per PRD)
```

---

## 2. Design → Architecture

```
ux-designer                         → docs/ux/
software-architecture               → docs/arch/
database-design                     → docs/database-design.md
plan-eng-review                     → "Is the architecture solid?" (once, after design)
data-analyst (parallel)             → biz/analytics/
```

### Checklist
- [ ] UX: IA + key screen specs (`docs/ux/screens/`)
- [ ] Architecture: `docs/arch/` + database-design
- [ ] plan-eng-review passed
- [ ] Tracking plan: events, funnels, dashboards, kill criteria

---

## 3. Tasks

After design is finalized, generate the task breakdown.

```
product-manager (Mode E)            → TASKS.md
```

Tasks reflect architecture decisions from design-doc. Each task = one PR-sized session.

---

## 4. Build (task-based loop)

Each task from TASKS.md = one session. Repeat until all tasks checked.

```
TASKS.md → next unchecked task
  ↓
Read docs/prd/prd.md + docs/prd/features/{feature}.md
  ↓
TDD (write tests → fail → implement → pass)
  ↓
reviewer + verifier (once per task, parallel)
  ├─ reviewer: security (OWASP) + code quality (2-pass)
  └─ verifier: browser QA (browse binary) + E2E tests
  ↓
Fix if needed → re-run → all pass
  ↓
check TASKS.md → sync docs/ → commit → push
```

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

Use **marketer** agent → outputs to `biz/marketing/`, `biz/legal/`, `biz/ops/`.

### Checklist
- [ ] Strategy: positioning, audience, channels
- [ ] Pricing: tiers and packaging
- [ ] Launch materials: PH, HN, Reddit drafts
- [ ] SEO: sitemap, meta, OG, robots.txt
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
