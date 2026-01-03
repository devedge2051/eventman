
# Phase 0 Research: Group Messaging Feature

## Decision: Stack and Integration
- **Stack**: Next.js (frontend), FastAPI (backend), Clerk (auth), Supabase (PostgreSQL), Twilio (SMS), Railway (deploy), GitHub Actions (CI/CD)
- **Rationale**: Modern, scalable, and developer-friendly stack with strong SaaS and compliance support. Clerk provides hosted authentication with good Next.js integration, Supabase provides managed Postgres with RLS, Railway simplifies deployment, Twilio is industry standard for SMS.
- **Alternatives considered**: Firebase (less SQL flexibility), AWS Amplify (more complex), Supabase Auth (less specialized auth tooling than Clerk), custom SMS gateway (less reliable).

## Decision: Phased Delivery
- **Phases**: 1) Infra/CI, 2) Auth, 3) Groups/SMS, 4) Agent
- **Rationale**: De-risk infra and auth first, then core features, then advanced agent. Allows incremental value and feedback.
- **Alternatives considered**: All-in-one delivery (riskier, slower feedback).

## Decision: Multi-Tenancy & Compliance
- **Approach**: Enforce tenant ID at all API/data layers, use Supabase RLS, log all actions, require opt-in/opt-out, encrypt PII.
- **Rationale**: Constitution and regulatory compliance.
- **Alternatives considered**: Single-tenant (not SaaS), manual audit (less robust).

## Decision: SMS Deduplication
- **Approach**: Deduplicate per announcement per user, even if in multiple groups.
- **Rationale**: Prevents spam, aligns with requirements.
- **Alternatives considered**: Send per group (risk of spam).

## Decision: Agent Fallback
- **Approach**: Agent replies with fallback/escalation if unsure.
- **Rationale**: User-friendly, safe, aligns with requirements.
- **Alternatives considered**: Ignore or escalate all unknowns (less helpful).

## Open Questions
- None for Phase 1 (infra/CI/CD) as all stack and compliance decisions are clear.

---

## 1. Language/Framework for Secure, Multi-Tenant SaaS with Messaging and Compliance
- **Decision**: Node.js with TypeScript and NestJS, or Python with FastAPI.
- **Rationale**: Both frameworks support rapid development, strong typing, robust middleware for security, and have mature libraries for multi-tenancy, authentication, and SMS integration.
- **Alternatives considered**: Ruby on Rails (less common for strict multi-tenancy), Java/Spring Boot (more heavyweight).

## 2. Storage/Database for Tenant Isolation and Auditability
- **Decision**: PostgreSQL with row-level security (RLS).
- **Rationale**: PostgreSQL supports RLS for strict tenant isolation, strong audit logging, and is widely used in SaaS.
- **Alternatives considered**: MongoDB (less strict isolation), MySQL (less advanced RLS).

## 3. SMS Delivery, Opt-Out, and Delivery Notifications
- **Decision**: Use Twilio or similar SMS gateway with webhook callbacks for delivery status.
- **Rationale**: Twilio provides opt-out management, delivery receipts, and compliance features out of the box.
- **Alternatives considered**: Nexmo, Plivo (similar features, less market share).

## 4. Authentication/Authorization Patterns for Multi-Tenant SaaS
- **Decision**: JWT-based authentication with tenant context in claims, RBAC for permissions.
- **Rationale**: JWT allows stateless auth, tenant context ensures isolation, RBAC is standard for SaaS.
- **Alternatives considered**: OAuth2 with tenant scoping, session-based auth (less scalable).

## 5. Subscription Tier Enforcement
- **Decision**: Store subscription tier per tenant, enforce feature/resource limits in middleware/services.
- **Rationale**: Centralized enforcement, easy to update, aligns with SaaS best practices.
- **Alternatives considered**: Feature flags (for more granular control), external billing platforms (Stripe, Chargebee).

## 6. Testing Frameworks for SaaS
- **Decision**: Jest (Node.js) or Pytest (Python) for unit/integration, Cypress or Playwright for E2E, custom compliance test suites.
- **Rationale**: These frameworks are widely adopted, support mocking, and integrate with CI/CD.
- **Alternatives considered**: Mocha/Chai (Node.js), Robot Framework (Python).

## 7. Regulatory Requirements for SMS, Data Retention, and Audit
- **Decision**: GDPR/CCPA compliance, opt-in/opt-out, audit logs for all actions, encrypted storage, data minimization.
- **Rationale**: Required by law, aligns with constitution, and industry best practices.
- **Alternatives considered**: None (must comply).

