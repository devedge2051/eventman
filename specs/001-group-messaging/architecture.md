# EventMan Group Messaging Platform: Architecture & Design

## Overview
EventMan is a multi-tenant SaaS platform for event organizers to manage attendee groups and send targeted SMS announcements. The system is designed for security, compliance, and extensibility, with a mobile-first, single-page web app and robust API backend.

## High-Level Architecture Diagram

```mermaid
graph TD
  subgraph Frontend (Next.js, PWA, iOS-optimized)
    A[Landing Page / Dashboard]
    B[Sign-In/Sign-Up]
    C[Group Management UI]
    D[Announcement/Notification Dashboard]
    E[Agent Escalation UI]
  end

  subgraph Backend (FastAPI)
    F[API Gateway]
    G[Auth Integration (Clerk)]
    H[Group & Member Service]
    I[Announcement Service]
    J[Agent Engine Integration]
    K[Audit Logging]
  end

  subgraph Infrastructure
    L[Supabase (PostgreSQL, Storage)]
    M[Twilio (SMS Gateway)]
    N[Railway (Deployment)]
    O[GitHub Actions (CI/CD)]
    P[Clerk (Auth)]
  end

  A -->|API| F
  B -->|API| F
  C -->|API| F
  D -->|API| F
  E -->|API| F
  F --> G
  F --> H
  F --> I
  F --> J
  F --> K
  H --> L
  I --> M
  G --> P
  J --> L
  K --> L
  F --> L
  F --> M
  F --> N
  F --> O
```

## Key Infrastructure Components

- **Frontend**: Next.js (TypeScript, React), PWA, mobile-first, iOS-optimized
- **Backend**: FastAPI (Python 3.11), RESTful API, stateless, multi-tenant
- **Auth**: Clerk (hosted authentication, user management, session handling)
- **Database/Storage**: Supabase (PostgreSQL with Row-Level Security, Storage)
- **SMS Gateway**: Twilio (send/receive SMS, delivery status, opt-out)
- **Agent Engine**: Pluggable Q&A engine for attendee questions (future phase)
- **Deployment**: Railway (cloud deployment for frontend and backend)
- **CI/CD**: GitHub Actions (build, test, deploy, compliance checks)
- **Audit Logging**: All critical actions logged for compliance and traceability
- **Shared Libraries**: TypeScript types, utilities for API contracts and validation

## Design Principles
- **Separation of Concerns**: Monorepo with clear separation between frontend, backend, shared libs, and config
- **Mobile-First**: SPA optimized for iOS and mobile browsers
- **Security & Compliance**: End-to-end encryption, RLS, audit trails, opt-in/out, GDPR/CCPA
- **Test-Driven**: TDD for all features, with a full test pyramid (unit, integration, E2E, compliance)
- **Extensibility**: Agent engine and other integrations are pluggable

## Data Flow Summary
1. User interacts with SPA (landing page, dashboard, group management, etc.)
2. Frontend calls FastAPI backend via REST API
3. Backend validates Clerk-authenticated sessions or tokens and enforces tenant boundaries
4. Group/member/announcement actions are processed and logged
5. Announcements trigger SMS via Twilio, with delivery status tracked
6. Agent engine (future) handles inbound SMS Q&A, escalates as needed
7. All actions are auditable and compliant

---
See `plan.md` and `tasks.md` for detailed delivery and engineering breakdown.
