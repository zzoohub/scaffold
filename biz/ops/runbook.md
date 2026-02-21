# Operations Runbook

## Monitoring

| Service | Tool | Alert Channel |
|---------|------|--------------|
| Errors | Sentry | Email / Slack |
| Uptime | BetterUptime | Email / SMS |
| Analytics | PostHog | Dashboard |
| Payments | Stripe/Lemon Squeezy | Email |

## Incident Response

### Severity Levels
- **P0 (Critical):** Service down, payments broken, data loss → Fix immediately
- **P1 (High):** Major feature broken, significant UX issue → Fix within 4h
- **P2 (Medium):** Minor bug, cosmetic issue → Fix within 24h

### Response Process
1. Acknowledge alert
2. Check Sentry for error details
3. Identify scope (all users? specific flow?)
4. If P0: rollback deployment first, investigate second
5. Fix → test → deploy
6. Post-mortem if P0/P1 (add to `analytics/reports/`)

## Routine Maintenance
- [ ] Weekly: review Sentry errors (suppress noise, fix recurring)
- [ ] Weekly: check dependency updates (Dependabot / Renovate)
- [ ] Monthly: review server costs, optimize if needed
- [ ] Monthly: backup verification
