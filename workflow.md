# Workflow

## 0. Validate

Write one hypothesis. Build the minimum test in hours. Ship to real users.
Move on when you see organic pull. Kill after 2–3 pivots with no signal.

---

## 1. Plan

```
product-manager (sub-agent)
  → product-brief                   → docs/prd/product-brief.md
  → prd-craft                       → docs/prd/prd.md
                                    → docs/prd/features/*.md
                                    → TASKS.md

plan-ceo-review                     → "방향이 맞는가?" (PRD 단위, 1회)
```

---

## 2. Design → Architecture

```
ux-designer                         → docs/ux/
software-architecture               → docs/design-doc.md
database-design                     → docs/database-design.md
plan-eng-review                     → "아키텍처가 견고한가?" (설계 후, 1회)
data-analyst (parallel)             → biz/analytics/
```

### Checklist
- [ ] UX: IA + 주요 화면 스펙 (`docs/ux/screens/`)
- [ ] Architecture: design-doc + database-design
- [ ] plan-eng-review 통과
- [ ] Tracking plan: events, funnels, dashboards, kill criteria

---

## 3. Build (task-based loop)

Each task from TASKS.md = one session. Repeat until all tasks checked.

```
TASKS.md → next unchecked task
  ↓
Read docs/prd/prd.md + docs/prd/features/{feature}.md
  ↓
TDD (write tests → fail → implement → pass)
  ↓
reviewer + verifier (task당 1회, 병렬)
  ├─ reviewer: security (OWASP) + code quality (2-pass)
  └─ verifier: browser QA (browse binary) + E2E tests
  ↓
Fix if needed → re-run → all pass
  ↓
TASKS.md 체크 → docs/ 동기화 → commit → push
```

Unit + integration tests via TDD. E2E via verifier per task.
Exception: manually test payment flows before launch.

---

## 4. Deploy

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

## 5. Launch → Marketing

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
- Feed insights back into step 1, 3, or 7

---

## 6. Measure → Decide

Check dashboard every morning (5 min). First Kill/Keep/Scale call at week 2.

Use **data-analyst** agent → `biz/analytics/reports/`.

**Week 1:** Observe. Respond to feedback. Don't optimize yet.
**Week 2:** First Kill/Keep/Scale evaluation.
**After:** Re-evaluate weekly until decision is clear.

Fix in this order: **Retention → Activation → Acquisition**

- **Kill** → Post-mortem, archive, move on same day.
- **Keep** → Back to step 1 or 3. Fix the biggest problem.
- **Scale** → Continue to step 7.

---

## 7. Grow → Optimize

Only after Scale. Don't grow a leaky product.

Use **growth-optimizer** agent → `biz/growth/`.

### Checklist
- [ ] ICE-scored experiment backlog
- [ ] Referral / viral loop design
- [ ] Churn prevention: cancel flow, save offers, dunning
- [ ] CRO: per-page/flow optimization
- [ ] E2E for critical paths (signup → Aha Moment → payment)
- [ ] E2E integrated into CI pipeline
