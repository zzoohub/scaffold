# Funnel Definitions

## Primary Funnel: Visitor → Paying Customer

```
Landing Page Visit
  → Sign Up
    → Aha Moment (core action completed)
      → Return Visit (D7)
        → Upgrade to Paid
```

### Stage Definitions

| Stage | Event | Target Rate | Notes |
|-------|-------|-------------|-------|
| Visit → Signup | `user_signed_up` | >5% | Below 3% = messaging problem |
| Signup → Aha Moment | `core_action_completed` | >40% | Below 25% = activation friction too high |
| Aha Moment → D7 Return | `user_returned` (7d) | >30% | Below 15% = PMF issue |
| D7 Return → Paid | `payment_completed` | >5% | Below 2% = pricing/value mismatch |

---

## Improvement Priority (AARRR in Reverse)

**Always fix from the bottom of the funnel upward.** Acquiring more users into a leaky funnel wastes everything.

### Priority Order:
1. **Retention** — Does the product have a retention plateau? If not, stop everything else and fix this.
2. **Activation** — Are users reaching the Aha Moment? If <40%, this is your #1 job.
3. **Acquisition** — Only invest in growth channels AFTER retention plateau exists and activation is healthy.
4. **Revenue** — Monetize after retention and activation are solid.
5. **Referral** — Design viral loops only when the product retains well (otherwise viral = more users who churn).

### How to Read the Funnel
- Focus on the stage with the **biggest absolute drop-off**
- One stage at a time — don't optimize everything in parallel
- For each drop-off, ask: "Is this a messaging problem, a UX problem, or a value problem?"

---

## Retention Cohort Analysis

### Setup
- Group users by signup week
- Track D1, D7, D14, D30, D60, D90 return rates
- Plot as retention curves per cohort

### What to Look For
- **Plateau exists?** → PMF confirmed. The plateau height determines your ceiling.
- **Plateau height improving across cohorts?** → Product improvements are working.
- **No plateau?** → Stop marketing. Fix core product value.

### Retention Benchmarks

| Plateau Level | What It Means |
|--------------|---------------|
| <10% | No PMF. Pivot or kill. |
| 10-20% | Borderline. Might work with major improvements. |
| 20-40% | Viable business. Push to raise it. |
| 40%+ | Strong. Scale aggressively. |

### Key Insight
Activation rate and retention plateau are connected but separate. High activation + low retention = the Aha Moment might be wrong (correlation, not causation). Low activation + high retention = the product works but onboarding is broken.

---

## Funnel Analysis Method

When analyzing a drop-off:
1. Graph the funnel by screen/step (not just stage)
2. If a single step shows >30% drop, zoom in to button-level granularity
3. Goal: make the curve a smooth downward slope, no cliffs
4. Check: "How long does it take?" — a 65% conversion over 30 days is worse than 35% in one session
