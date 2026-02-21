# Kill / Keep / Scale Criteria

## Carrying Capacity

The single number that determines your product's long-term user ceiling.

```
CC = Daily Organic Inflow ÷ Daily Churn Rate
```

- **Inflow**: organic new users + resurrections + referrals. Exclude paid.
- **Churn Rate**: % of MAU that permanently leaves per day.
  - "Permanent" = hasn't returned in 90 days.
  - Churn rate ≠ inverse of retention. Retention can improve while churn stays flat.

### How Fast Can You Know?
- **Inflow**: measurable within 1 week of launch
- **Churn Rate**: measurable within 1-2 months
- **CC prediction**: reliable within 2-3 months

### Max CC (Your Ceiling)
```
Max CC = Addressable Market × Online Penetration Rate × Your Market Share
```
- Addressable Market: total potential users for this problem
- Online Penetration: % who would solve this problem online
- Market Share: your realistic share vs competitors

If Max CC is too small to justify continued investment, kill early.

### CC Improvement
CC only improves through **product changes** that affect Inflow or Churn:
- Lower churn → higher CC (most effective lever)
- Increase organic inflow (SEO, viral loops, word of mouth) → higher CC
- Paid ads do NOT improve CC. They temporarily inflate MAU above CC, which falls back when ads stop.

---

## Decision Framework

Evaluate every product **2 weeks after launch**. Repeat weekly until decision is clear.

### 🟢 SCALE — Double down
All of these must be true:
- Retention curve has a **plateau** (PMF exists)
- Plateau height >20%
- Signup → Aha Moment activation rate >30%
- At least 1 organic signup per day (not from launch spike)
- CC trending upward on 7d trailing
- Positive qualitative signals (users asking for features, not just "cool")

### 🟡 KEEP — Iterate, re-evaluate in 2 weeks
At least 3 of these:
- Retention curve shows early signs of plateau
- Activation rate >20%
- Some users returning without prompting
- Clear feedback on what to fix
- CC is flat (not declining)

### 🔴 KILL — Stop development
Any of these:
- No retention plateau after 4+ weeks
- Activation rate <10%
- CC declining on 30d trailing
- No organic signups after launch spike dies (1 week)
- Feedback is lukewarm ("interesting" but no urgency)
- Usage frequency <3x/month (plateau nearly impossible below this)

---

## Kill Protocol
1. Stop development immediately
2. Write post-mortem → `biz/analytics/reports/`
   - What was the CC? What limited it?
   - Was there a retention plateau? At what height?
   - What was the Aha Moment? Did users reach it?
   - What would you do differently?
3. Leave running at zero cost (serverless) or shut down
4. Archive code (don't delete — parts may be reusable)
5. Move to next idea the same day

## Emotional Override Protection
- Make the decision based on numbers, not feelings
- "I spent 2 days on this" is not a reason to keep it (dev cost ≈ 0)
- "But one user really loved it" — look at the cohort, not individuals
- "If I just add X feature it'll work" — if CC is declining, adding features won't help
- Set a calendar reminder for the decision date BEFORE launch
- Review CC, not MAU. MAU can be inflated temporarily. CC cannot.
