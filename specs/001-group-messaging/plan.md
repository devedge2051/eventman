# Implementation Plan: Group Messaging

**Branch**: `001-group-messaging` | **Date**: 2026-01-03 | **Spec**: `specs/001-group-messaging/spec.md`
**Input**: Feature specification from `specs/001-group-messaging/spec.md`

## Summary

Build a multi-tenant, privacy-first group messaging feature for event organizers.
Organizers create groups (hotel attendees, locals, youths, resources, train travelers),
add people to those groups, and send SMS announcements via Twilio. A later phase adds
an agent that can answer attendee questions over SMS using event/group context.

The stack is a monorepo with a Next.js SPA frontend, a FastAPI backend, Clerk for
authentication, Supabase PostgreSQL (with RLS) for storage, and Railway + GitHub
Actions for deployment and CI/CD. Work is delivered in four phases:

1. Phase 1 – Infrastructure & CI/CD (this phase): repository layout, basic FastAPI
   app and health check, backend/frontend CI workflows, environment variable
   definitions, and Railway service wiring.
2. Phase 2 – Authentication: Clerk-powered sign-in/sign-up, tenant context and RBAC
   on the backend, and profile management.
3. Phase 3 – Groups & SMS: group and member CRUD, Twilio integration, announcement
   deduplication, delivery tracking, and dashboards.
4. Phase 4 – Agent Integration: SMS question routing to an agent, fallbacks and
   human escalation, and full audit logging of agent interactions.

## Technical Context

**Language/Version**: 
- Frontend: TypeScript, React, Next.js 14+ (Node.js LTS)
- Backend: Python 3.13 with FastAPI

**Primary Dependencies**: 
- Frontend: Next.js, React, React DOM, React Query (or similar), TailwindCSS
- Backend: FastAPI, Uvicorn, SQLAlchemy (planned), httpx, pytest, pytest-asyncio
- Auth: Clerk (Next.js SDK and backend token validation)
- Data: Supabase PostgreSQL with Row-Level Security
- Messaging: Twilio SMS API/SDK
- CI/CD: GitHub Actions, Railway CLI

**Storage**: Supabase-hosted PostgreSQL with RLS for tenant isolation and audit
logging tables. Twilio for transient message delivery state, mirrored into
DeliveryReceipt records.

**Testing**: 
- Backend: pytest, pytest-asyncio, httpx for API integration tests
- Frontend: Jest, React Testing Library
- E2E: Playwright (or Cypress) for mobile-web flows
- Tooling: coverage reports in CI, linting, basic security scans

**Target Platform**: 
- Multi-tenant SaaS deployed on Railway: FastAPI backend and Next.js frontend
- Mobile-first web UI optimized for iOS Safari, with PWA support

**Project Type**: Web monorepo with separate `apps/frontend` and `apps/backend`
applications plus shared libraries under `libs/`.

**Performance Goals**: 
- Support at least hundreds of concurrent organizers and thousands of recipients
  per announcement batch.
- Typical API latency under 250ms p95 for core group and announcement operations.
- End-to-end SMS delivery to Twilio within seconds; Twilio network latency is
  out of scope but tracked via delivery receipts.

**Constraints**: 
- Strict tenant isolation at every layer (auth, API, DB) with RLS enforced.
- Compliance with GDPR/CCPA and SMS regulations (opt-in/opt-out, audit logs).
- Test-driven development is mandatory; all features require automated tests.
- Simplicity and maintainability: avoid unnecessary indirection and over-
  engineering; follow YAGNI.

**Scale/Scope**: 
- Initial target: up to tens of tenants and thousands of recipients per tenant.
- Scope for this feature: group management, SMS announcements, and an optional
  agent for Q&A, with full auditability.

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

Based on `.specify/memory/constitution.md` and `constitution.md`:

- **Privacy-First & Compliance** (PASS): Personal and messaging data is stored
  in Supabase Postgres with RLS, access is always scoped by tenant, and SMS
  flows enforce opt-in/opt-out and audit logging. All data in transit is over
  HTTPS; at-rest encryption is provided by the platform.
- **Modular & Testable Design** (PASS): The feature is split into clearly
  defined backend services (auth, groups, announcements, agent) and frontend
  modules (auth, dashboards, notifications). Each has dedicated tests and
  documented API contracts.
- **API-First** (PASS): All capabilities are exposed through FastAPI endpoints
  documented via OpenAPI (`contracts/openapi.yaml`). The architecture allows
  future CLI tooling to reuse the same APIs.
- **TDD & Quality Gates** (PASS): Tests are required before implementation
  (unit, integration, E2E). GitHub Actions pipelines run tests, linting,
  and basic security checks, blocking merges on failure.
- **Observability & Auditability** (PASS): Audit logging is a first-class
  requirement (groups, membership, announcements, agent actions). Railway
  logs plus structured application logs support incident analysis.
- **Multi-Tenancy & Subscription Tiers** (PASS): Tenant IDs are enforced in
  auth claims and DB schemas. Subscription tier fields exist on Tenant/User
  and are enforced in middleware/services.

**Post-Design Recheck**: Current design and Phase 0/1 documents (research,
data-model, quickstart, contracts) remain compliant. No constitution
violations require complexity tracking at this time.

## Project Structure

### Documentation (this feature)

```text
specs/001-group-messaging/
├── spec.md          # Feature specification and clarified requirements
├── plan.md          # This implementation plan (/speckit.plan output)
├── research.md      # Phase 0 research and stack decisions
├── data-model.md    # Phase 1 domain entities and relationships
├── quickstart.md    # Phase 1 setup and usage guide
├── contracts/       # Phase 1 API contracts (OpenAPI)
│   └── openapi.yaml
└── tasks.md         # Detailed engineering tasks (/speckit.tasks output)
```

### Source Code (repository root)

```text
apps/
  backend/
    src/
      api/
      models/
      services/
      main.py
    tests/
      conftest.py
      test_health.py

  frontend/
    src/
      components/
      pages/
      services/
    tests/

libs/
  shared-types/
  utils/

config/
  env/
    backend.example.env
    frontend.example.env   # planned

specs/
  001-group-messaging/
    ... (docs as above)

.github/
  workflows/
    backend-ci.yml
    frontend-ci.yml        # planned
```

**Structure Decision**: Use a monorepo with `apps/frontend` and `apps/backend`
for clear separation of concerns, shared libraries under `libs/`, and
feature-specific documentation under `specs/001-group-messaging`. This keeps
frontend and backend deployable independently while sharing types and tooling.

## Complexity Tracking

No constitution violations or extra complexity beyond standard monorepo and
frontend/backend separation have been introduced. This section remains empty
unless future changes require explicit justification.
