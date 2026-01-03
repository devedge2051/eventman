# EventMan Group Messaging Platform

## Overview
EventMan is a multi-tenant SaaS platform for event organizers to manage attendee groups and send targeted SMS announcements. The platform is designed for security, compliance, and extensibility, with a modern monorepo architecture.

## Features
- Create, edit, and delete attendee groups
- Add/remove people to/from groups
- Send SMS announcements to group members (Twilio integration)
- Deduplication: attendees in multiple groups receive only one message per announcement
- Delivery status feedback and audit logging
- Opt-in/opt-out and privacy compliance (GDPR, CCPA)
- Agent integration for attendee Q&A via SMS (future phase)

## Architecture
- **Frontend:** Next.js (TypeScript, React)
- **Backend:** FastAPI (Python 3.11)
- **Auth:** Clerk (hosted authentication for web)
- **Database:** Supabase (PostgreSQL with RLS)
- **SMS:** Twilio
- **Deployment:** Railway
- **CI/CD:** GitHub Actions
- **Monorepo:**
  - `apps/frontend` (Next.js)
  - `apps/backend` (FastAPI)
  - `libs/` (shared types, utils)
  - `config/` (env, deployment)
  - `scripts/` (dev/build/deploy)

## Phased Delivery
1. **Infrastructure & CI/CD**: Railway, Supabase, GitHub Actions
2. **Authentication**: Sign-in/sign-up, multi-tenant, RBAC
3. **Group Management & SMS**: CRUD, Twilio, deduplication, audit
4. **Agent Integration**: SMS Q&A, fallback/escalation, logging

## Compliance & Security
- End-to-end encryption for sensitive data
- Row-level security and tenant isolation
- Consent management and opt-out
- Full audit trails for all actions
- Accessibility (WCAG 2.1 AA)

## Status
- [x] Specification & planning
- [x] Monorepo scaffolding
- [ ] Infrastructure & CI/CD setup (Phase 1)
- [ ] Authentication (Phase 2)
- [ ] Group management & SMS (Phase 3)
- [ ] Agent integration (Phase 4)

---
See `specs/001-group-messaging/spec.md` for the full feature specification.
