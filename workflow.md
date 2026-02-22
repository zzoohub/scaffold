# Workflow

## 0. Validate — Before Building Anything

```
"Products that work are obvious even when ugly and poorly marketed.
 Products that don't work stay broken no matter how polished."

→ Define the ONE core hypothesis
→ Build the minimum thing that tests it (hours, not days)
→ Put it in front of real users before investing in design/polish
→ Look for organic pull (users asking for more, sharing unprompted)
→ If no pull → pivot the hypothesis, don't polish the execution
```

## 1. Product → Design → UI

```
z-product-brief
  → z-prd-craft                    # PRD → docs/prd.md
  → z-ux-designer (agent)          # UX spec → docs/ux-design.md
    + Google Stitch (MCP)           # Visual mockups (parallel)
  → z-ui-engineer (agent)          # Component implementation
  → UX check by human
```

## 2. Architecture → Backend → Frontend

```
z-software-architect               # Design doc → docs/design-doc.md
  → z-database-architect           # DB design → docs/database-design.md
  → z-api-design                   # API endpoints (plan)
  → Backend implementation         # z-axum-hexagonal / z-fastapi-hexagonal
  → Frontend implementation        # z-nextjs
  → (auto, parallel)
      z-security-reviewer           # Security audit → fix
      z-tester                      # Test changed code → fix
```

## 3. Pre-Deployment

### GCP
- [ ] Cloud Run services (API / Worker)
- [ ] Secret Manager — API keys, DB credentials, third-party tokens
- [ ] IAM service accounts

### Cloudflare
- [ ] Domain + DNS
- [ ] SSL/TLS
- [ ] Workers / Pages (if used)

### Database
- [ ] Provision PostgreSQL (Neon)
- [ ] Run migrations
- [ ] Seed initial data (if needed)

### Observability
- [ ] Sentry — API + Worker + Web + Mobile
- [ ] PostHog — Web + Mobile
- [ ] Verify Aha Moment event tracking (see `biz/analytics/tracking-plan.md`)

### EAS (Mobile)
- [ ] EAS project init
- [ ] Build profiles (development / preview / production)
- [ ] EAS Update (OTA) channel setup
- [ ] App Store / Play Store credentials

### CI/CD (GitHub Actions)
- [ ] Workflows per service
- [ ] Dockerfile per service
- [ ] GitHub Secrets — GCP SA key, Sentry DSN, PostHog key, EAS token

### Pulumi
- [ ] Stack init (staging / production)
- [ ] State backend configured

## 4. Launch → Marketing

```
z-marketer (agent)
  → biz/marketing/strategy.md            # Positioning, audience, channels, messaging
  → biz/marketing/pricing.md              # Pricing strategy, tiers, packaging
  → biz/marketing/competitors.md          # Competitor analysis
  → biz/marketing/launch/                 # PH, HN, Reddit drafts + checklist
  → biz/marketing/content/calendar.md     # Editorial calendar + content pillars
  → biz/marketing/content/free-tools.md   # Free tool strategy
  → biz/marketing/content/                # Email sequences, social posts, blog
  → biz/marketing/assets/                 # OG image, screenshots, demo
  → biz/legal/                            # Terms of Service, Privacy Policy
  → biz/ops/                              # FAQ, runbook
  → Launch execution (human)
```

## 5. Measure → Decide

```
z-data-analyst (agent)
  → biz/analytics/tracking-plan.md  # Event tracking + Aha Moment definition
  → biz/analytics/funnels.md        # Funnel definitions + retention cohorts
  → biz/analytics/dashboards.md     # CC monitoring + retention + growth dashboards
  → biz/analytics/kill-criteria.md  # CC calculation + Kill/Keep/Scale
  → biz/analytics/health-score.md   # Customer health score model
  → biz/analytics/reports/          # Weekly reports, deep-dives, A/B results
  → Kill / Keep / Scale decision    # → human

  Improvement priority (always in this order):
  1. Retention (does a plateau exist? raise it)
  2. Activation (are users reaching Aha Moment? remove friction)
  3. Acquisition (only after 1 & 2 are healthy)
```

## 6. Grow → Optimize

```
z-growth-optimizer (agent)
  → biz/growth/experiments.md          # ICE-scored experiment backlog + results
  → biz/growth/referral-program.md     # Referral + viral loop design
  → biz/growth/churn-prevention.md     # Cancel flow, save offers, intervention
  → biz/growth/dunning.md              # Payment failure recovery
  → biz/growth/cro/                    # Per-page/flow CRO analyses
  → Experiment results → z-data-analyst # Handoff for statistical analysis
```

## 7. Operate → Iterate

```
  → biz/ops/faq.md                  # FAQ from user questions
  → biz/ops/feedback-log.md         # Feedback collection
  → biz/ops/runbook.md              # Incident response
  → Feedback-driven improvements    # → back to step 0, 1, or 2
```
