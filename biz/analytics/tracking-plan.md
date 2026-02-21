# Event Tracking Plan

## Naming Convention
<!-- format: snake_case, verb_noun pattern -->
<!-- e.g., user_signed_up, feature_used, payment_completed -->

## Identity
<!-- How users are identified across sessions -->
<!-- PostHog: posthog.identify(userId, { email, plan, ... }) -->

---

## Aha Moment

The single most important metric for this product. Every team decision should serve this.

> **"[ACTION X] within [Y days] of signup, [Z times]"**

### Definition
- **X (Core Action):**
  <!-- The action that delivers the product's core value. Not "login" or "view page" — the moment the user first feels the value. -->
- **Y (Time Window):** days
  <!-- Before the first impression fades. Typically 3-14 days. -->
- **Z (Frequency):** times
  <!-- Usually 1 is not enough. Test 2-10. -->

### How to Find It
1. Segment retained users (D30+) vs churned users
2. Compare which actions retained users did that churned users didn't
3. For each candidate action, calculate:
   - **RPV** = A ÷ (A+B) — must be >95%
   - **Intersection** = A ÷ (A+B+C) — maximize this
   - Where: A = did action + retained, B = didn't do action + retained, C = did action + churned
4. Sweep Z (frequency) and Y (time window) to find the combination that maximizes Intersection

### Activation Goal
Make more users reach Aha Moment, faster and easier.
- Current activation rate: ___%
- Target activation rate: ___%
- Biggest friction point before Aha Moment: ___

---

## Core Events

### Acquisition
| Event | Trigger | Properties |
|-------|---------|-----------|
| `page_viewed` | Any page load | `path`, `referrer`, `utm_source`, `utm_medium`, `utm_campaign` |
| `cta_clicked` | Marketing CTA click | `cta_name`, `page` |

### Activation
| Event | Trigger | Properties |
|-------|---------|-----------|
| `user_signed_up` | Account created | `method` (email/google/github) |
| `onboarding_started` | Onboarding begins | |
| `onboarding_completed` | Onboarding finishes | `duration_seconds` |
| `core_action_completed` | Aha Moment action | `action_type` |

### Engagement
| Event | Trigger | Properties |
|-------|---------|-----------|
| `feature_used` | Any feature usage | `feature_name` |
| `session_started` | App opened / tab focused | `returning` (bool) |

### Revenue
| Event | Trigger | Properties |
|-------|---------|-----------|
| `checkout_started` | Pricing page / upgrade click | `plan` |
| `payment_completed` | Successful payment | `plan`, `amount`, `currency`, `interval` |
| `subscription_cancelled` | User cancels | `reason`, `plan`, `tenure_days` |

### Retention
| Event | Trigger | Properties |
|-------|---------|-----------|
| `user_returned` | Return after 24h+ absence | `days_since_last_visit` |

### Referral
| Event | Trigger | Properties |
|-------|---------|-----------|
| `referral_sent` | User shares invite | `channel` (link/email/social) |
| `referral_accepted` | Invited user signs up | `referrer_id` |

## User Properties (set on identify)
| Property | Type | Description |
|----------|------|-------------|
| `plan` | string | Current plan (free/pro/team) |
| `signed_up_at` | datetime | Registration date |
| `activation_date` | datetime | When Aha Moment first completed |
| `referral_source` | string | How this user was acquired |

## Feature-Specific Events
<!-- Add per-feature events here as features are built -->
