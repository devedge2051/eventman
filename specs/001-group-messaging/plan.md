# Implementation Plan: Group Messaging

**Branch**: `001-group-messaging` | **Date**: January 3, 2026 | **Spec**: [spec.md]
**Input**: Feature specification from `/specs/[###-feature-name]/spec.md`

**Note**: This template is filled in by the `/speckit.plan` command. See `.specify/templates/commands/plan.md` for the execution workflow.

## Summary

Enable event organizers to create/manage groups and send SMS announcements to group members, with deduplication, delivery feedback, and compliance. Tech stack: Next.js (frontend), FastAPI (backend), Clerk (auth), Supabase (PostgreSQL), Twilio (SMS), Railway (deploy), GitHub Actions (CI/CD).

## Phased Delivery Plan

**Phase 1: Infrastructure & CI/CD**
- Set up Railway projects for backend and frontend deployments
- Configure Supabase project (PostgreSQL, Storage) and Clerk application for authentication
- Integrate GitHub Actions for automated build, test, and deploy pipelines
- Set up environment secrets and basic monitoring/logging

**Phase 2: Authentication (Sign-In/Sign-Up)**
- Implement user registration and login using Clerk (Next.js frontend widgets, FastAPI backend token/session validation)
- Enforce multi-tenant boundaries and role-based access
- Add basic user profile management

**Phase 3: Group Management & SMS Announcements**
- CRUD for groups and group membership (backend API, frontend UI)
- Integrate Twilio for SMS sending
- Implement announcement creation, deduplication, and delivery status feedback
- Logging/audit for all group and announcement actions

**Phase 4: Agent Integration**
- Integrate agent engine for SMS-based Q&A
- Route incoming SMS to agent or human responder as needed
- Provide fallback/escalation for unanswerable questions
- Log agent interactions for compliance and improvement

## Technical Context

**Language/Version**: Python 3.11 (backend), JavaScript/TypeScript (frontend, Next.js 14+)  
**Primary Dependencies**: FastAPI, SQLAlchemy, Supabase Python client, Clerk SDKs (backend), Twilio Python SDK, Next.js, Clerk JS SDK, TailwindCSS, React Query  
**Storage**: Supabase (PostgreSQL)  
**Testing**: pytest, httpx, Playwright, Jest, React Testing Library, CI via GitHub Actions  
**Target Platform**: Railway (cloud, Linux), Web (modern browsers)  
**Project Type**: Web (Next.js frontend, FastAPI backend)  
**Performance Goals**: 99% SMS delivered <2min, API p95 <300ms  
**Constraints**: GDPR/CCPA compliance, end-to-end encryption for sensitive data, multi-tenant data isolation, opt-out support, WCAG accessibility  
**Scale/Scope**: 10k+ users/event, 100+ concurrent organizers, 100k+ SMS/day

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

- All features are testable and measurable (see spec acceptance criteria)
- Data protection: Supabase/Postgres with row-level security, Twilio for SMS (PII encrypted at rest/in transit)
- Consent/opt-out: Required for all SMS, opt-out endpoint and UI
- Audit trails: All group, membership, and announcement actions logged (Supabase audit table)
- Multi-tenancy: Tenant ID enforced at all API/data layers
- Accessibility: Next.js frontend must meet WCAG 2.1 AA
- CI/CD: All code tested and reviewed before deploy (GitHub Actions)

## Clarifications

- Q: What are we using for Next.js deployment? → A: Railway

## Project Structure

### Documentation (this feature)

ios/ or android/

```text
specs/001-group-messaging/
├── plan.md              # This file (/speckit.plan command output)
├── research.md          # Phase 0 output (/speckit.plan command)
├── data-model.md        # Phase 1 output (/speckit.plan command)
├── quickstart.md        # Phase 1 output (/speckit.plan command)
├── contracts/           # Phase 1 output (/speckit.plan command)
└── tasks.md             # Phase 2 output (/speckit.tasks command - NOT created by /speckit.plan)
```

### Source Code (repository root)


```text
apps/
├── backend/           # FastAPI app (Python)
│   ├── src/
│   │   ├── models/
│   │   ├── services/
│   │   └── api/
│   └── tests/
├── frontend/          # Next.js app (TypeScript)
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   └── services/
│   └── tests/
libs/
├── shared-types/      # Shared TypeScript types/interfaces (for API contracts, validation)
├── utils/             # Shared utilities (e.g., validation, formatting)
config/
├── env/               # Environment config, secrets templates, deployment configs
scripts/               # Dev, build, and deployment scripts (CI/CD, setup)
```

**Structure Decision**: Monorepo with clear separation: `apps/frontend` (Next.js) and `apps/backend` (FastAPI), plus `libs` for shared code and `config` for environment/deployment. Enables unified CI/CD, code sharing, and clear boundaries for frontend/backend teams.

## Complexity Tracking

> **Fill ONLY if Constitution Check has violations that must be justified**

| Violation | Why Needed | Simpler Alternative Rejected Because |
|-----------|------------|-------------------------------------|
| [e.g., 4th project] | [current need] | [why 3 projects insufficient] |
| [e.g., Repository pattern] | [specific problem] | [why direct DB access insufficient] |
