# Workflow — Build

## 4. Build (phase-based loop)

Process one phase at a time. Within each phase, tasks run in parallel.

```
tasks/board.md → pick phase with remaining backlog tasks
  ↓
Create team — route tasks to domain agents by `touches` path:
  apps/api/, apps/worker/, db/ → backend-developer
  apps/web/                    → frontend-developer
  apps/mobile/                 → mobile-developer
  apps/desktop/                → frontend-developer (desktop)
  ↓
Each teammate (same repo, no worktree):
  Read tasks/features/{feature}.md for task context
  ↓
  TDD (write tests → fail → implement → pass)
  ↓
  reviewer + verifier (once per task, parallel)
    ├─ reviewer: security (OWASP) + code quality (2-pass)
    └─ verifier: qa skill (browser) + E2E tests
  ↓
  Fix if needed → re-run → all pass
  ↓
  Update tasks/board.md status → done → commit → push

Side duties (not routed as build tasks):
  docs/  → plan agents (product-manager, architect, ux-designer) update on plan change
  tasks/ → developers update tasks/board.md per task; task-manager maintains structure
```

Repeat until all phases are complete — no backlog tasks remain in `tasks/board.md`.
Phase N+1 starts after all Phase N tasks are done.
Same repo — `touches` field is the only file conflict guard. No worktree isolation.
Unit + integration tests via TDD. E2E via verifier per task.
Exception: manually test payment flows before launch.
