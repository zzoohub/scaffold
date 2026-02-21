# Pre-Deployment Checklist

## Database
- [ ] Provision PostgreSQL (Neon)
- [ ] Run migrations
- [ ] Seed initial data (if needed)

## Infra
- [ ] Pulumi stack init (staging / production)
- [ ] Redis (Upstash / Cloud Memorystore)
- [ ] Domain + DNS
- [ ] SSL/TLS

## Observability
- [ ] Sentry — API + Web + Mobile
- [ ] PostHog — Web + Mobile

## Auth
- [ ] Auth provider setup (Clerk / Supabase Auth / etc.)

## CI/CD
- [ ] GitHub Actions workflows (per service)
- [ ] Dockerfile per service
- [ ] Environment variables in CI secrets

## Pre-Launch
- [ ] `EXPLAIN ANALYZE` on critical queries
- [ ] Rate limiting on API
- [ ] CORS configuration
- [ ] Error pages (404, 500)
- [ ] OG meta tags / SEO basics
