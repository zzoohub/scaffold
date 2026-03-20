# Workflow — Ship

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

Use **marketer** agent → `biz/marketing/strategy.md`, `biz/marketing/launch/`, `biz/marketing/pricing.md`, `biz/marketing/competitors.md`
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
- Feed insights back into Plan (step 1), Build (step 4), or Grow (step 8)

---

## 7. Measure → Decide

Check dashboard every morning (5 min). First Kill/Keep/Scale call at week 2.

Use **data-analyst** agent → `biz/analytics/reports/`.

**Week 1:** Observe. Respond to feedback. Don't optimize yet.
**Week 2:** First Kill/Keep/Scale evaluation.
**After:** Re-evaluate weekly until decision is clear.

Fix in this order: **Retention → Activation → Acquisition**

- **Kill** → Post-mortem, archive, move on same day.
- **Keep** → Back to Plan (step 1) or Build (step 4). Fix the biggest problem.
- **Scale** → Continue to Grow (step 8).

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
