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
ux-designer                         → docs/ux/ux-design.md + docs/ux/screens/*.md
  + Stitch (parallel)               → visual mockups

software-architecture               → docs/design-doc.md
database-design                     → docs/database-design.md

plan-eng-review                     → "아키텍처가 견고한가?" (설계 후, 1회)

data-analyst (parallel)
  → biz/analytics/tracking-plan.md       # events + Aha Moment
  → biz/analytics/funnels.md             # funnel stages + target rates
  → biz/analytics/dashboards.md          # PostHog dashboard specs
  → biz/analytics/kill-criteria.md       # Kill / Keep / Scale criteria
```

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

Direct CLI deploy per service. No IaC layer — keep it simple.

### Deploy commands

All in `justfile`. Example recipes:

```
just deploy-api          # gcloud run deploy
just deploy-worker       # wrangler deploy (CF Worker)
just deploy-web          # vercel deploy --prod (or git push)
just deploy-mobile       # eas build + eas submit
just db-migrate          # neon CLI or psql
```

### Per-service setup

| Service | Platform | Deploy | Secrets |
|---------|----------|--------|---------|
| API | Cloud Run | `gcloud run deploy` | `gcloud run services update --set-env-vars` |
| Worker | CF Workers | `wrangler deploy` | `wrangler secret put` |
| Web | Vercel / CF Pages | git push (auto) | Vercel dashboard / `wrangler pages secret put` |
| DB | Neon | Dashboard / `neon` CLI | Connection string in `.env` |
| Mobile | EAS | `eas build` + `eas submit` | `eas secret:push` |

### DB
- Run migrations before deploy
- Seed data (if needed)
- Neon branching for staging

### Secrets
- Local: `.env` (gitignored)
- Production: each platform's secret management (see table above)
- Shared secrets across services: document in `.env.example`

### Mobile (if applicable)
- EAS project + build profiles (`eas.json`)
- EAS Update (OTA) channel
- App Store / Play Store credentials

### First deploy checklist
- [ ] Staging → smoke test → Production
- [ ] PostHog events firing
- [ ] Sentry connected (API + Worker + Web + Mobile)
- [ ] Aha Moment event works end-to-end

---

## 5. Launch → Marketing

```
marketer
  → biz/marketing/strategy.md            # positioning, audience, channels
  → biz/marketing/pricing.md             # tiers and packaging
  → biz/marketing/competitors.md         # competitive landscape
  → biz/marketing/launch/                # PH, HN, Reddit drafts + checklist
  → biz/marketing/content/               # email sequences, content calendar
  → biz/marketing/assets/                # OG image, screenshots, demo
  → biz/marketing/seo/                   # technical SEO (sitemap, meta, OG, robots.txt)
  → biz/legal/                           # ToS, Privacy Policy
  → biz/ops/                             # FAQ, runbook
```

### Launch day
- Post per `biz/marketing/launch/checklist.md`
- Respond to all comments within 2 hours
- Log feedback → `biz/ops/feedback-log.md`

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

```
data-analyst
  → biz/analytics/reports/               # weekly reports
  → biz/analytics/health-score.md        # customer health score
  → Kill / Keep / Scale recommendation
```

**Week 1:** Observe. Respond to feedback. Don't optimize yet.
**Week 2:** First evaluation per `biz/analytics/kill-criteria.md`
**After:** Re-evaluate weekly until decision is clear.

Fix in this order: **Retention → Activation → Acquisition**

- **Kill** → Post-mortem, archive, move on same day.
- **Keep** → Back to step 1 or 3. Fix the biggest problem.
- **Scale** → Continue to step 7.

---

## 7. Grow → Optimize

Only after Scale. Don't grow a leaky product.

```
growth-optimizer
  → biz/growth/experiments.md            # ICE-scored backlog + results
  → biz/growth/referral-program.md       # referral + viral loops
  → biz/growth/churn-prevention.md       # cancel flow, save offers
  → biz/growth/dunning.md               # payment failure recovery
  → biz/growth/cro/                      # per-page/flow CRO
  → biz/growth/seo/                      # content SEO (keywords, blog, landing pages)
  → Experiment results → data-analyst  # statistical analysis handoff
```

### Now add E2E tests
Product survived — protect core flows from regression.
- [ ] E2E for critical paths (signup → Aha Moment → payment)
- [ ] Integrated into CI pipeline
- [ ] Flaky tests: fix or delete within 48 hours
