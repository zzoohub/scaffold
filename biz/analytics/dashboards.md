# PostHog Dashboard Setup

## Dashboard 1: CC Monitor (check every morning, 5 min)

**This is the most important dashboard.** Everything else is detail.

### Carrying Capacity
```
CC = Daily Inflow ÷ Daily Churn Rate

- Inflow: new signups + resurrections (organic only, exclude paid)
- Churn Rate: % of MAU that churns per day
  (Churn definition: user who hasn't returned in 90 days)
```

Track these as two lines:
- **CC 7-day trailing average** — sensitive, leading indicator
- **CC 30-day trailing average** — stable, confirms direction
- When 7d rises and 30d follows → you're on the right track
- When 7d drops but 30d holds → investigate but don't panic

### Daily Pulse
- New signups (today vs yesterday vs 7-day avg)
- DAU
- Aha Moment activation rate (users who completed core action within Y days)
- Error count from Sentry

## Dashboard 2: Retention (check weekly, most important weekly view)

### Cohort Retention Table
- D1, D7, D14, D30 retention by signup week cohort
- **Look for Plateau** — the week where retention curve flattens
  - No plateau = no PMF. Stop marketing. Fix the product.
  - Plateau at 20% = viable but limited
  - Plateau at 40%+ = strong business potential
- Compare cohorts over time: are newer cohorts retaining better?

### Retention by Segment
- By acquisition channel (which channels bring users that actually stay?)
- By plan type
- By whether user hit Aha Moment or not

### WAU/MAU Ratio
- Healthy: >25%
- Track trend, not snapshot

## Dashboard 3: Growth Engine (check weekly)

### Funnel Conversion
- Primary funnel: Visit → Signup → Aha Moment → D7 Return → Paid
- Identify the biggest drop-off stage
- **Improve from the bottom up**: Retention → Activation → Acquisition

### Inflow Breakdown
- Organic new users
- Resurrected users (churned users who returned)
- Referral users (trackable invites)
- Paid users (ads — track separately, these don't affect CC)

### Viral Metrics (if referral exists)
- Viral K = invites sent × accept rate per user
- Amplification Factor = 1 ÷ (1 - K)

## Dashboard 4: Revenue (check weekly)
- MRR
- New paid conversions
- Churn events (cancellations)
- Trial → Paid conversion rate

## Session Replay
- Enable for all users initially
- After scale: sample 20% of sessions
- Always record: sessions with errors, sessions where user hit paywall
- Weekly review: watch 10-20 sessions, focus on confused users and users who churned right before Aha Moment
