# Engineering Tasks: Group Messaging Feature

**Test-Driven Development (TDD) Approach:**
For every feature and bugfix, write the relevant test cases first (which should fail), then implement the code to make the tests pass. This applies to all unit, integration, and E2E tests for both frontend and backend.

## Phase 1: Infrastructure & CI/CD

### Backend
- [ ] Create Railway service for FastAPI backend using `apps/backend` as the project root
- [ ] Create Supabase project and enable PostgreSQL and Storage for the `eventman` backend
- [x] Define backend environment variables and secrets required by FastAPI, Supabase, Clerk, and Twilio and document them in `config/env/backend.example.env`
- [x] Add GitHub Actions workflow file to build, test (pytest), and deploy the backend to Railway on pushes to the main branch
- [x] Configure the backend CI job in GitHub Actions to run pytest unit and integration test suites and fail the pipeline on any test failure

### Backend Test Cases
- [ ] Add tests or checks that verify backend CI/CD pipeline is triggered on pull requests and main-branch pushes and that required environment variables are present

### Frontend
- [ ] Create Railway service for Next.js frontend using `apps/frontend` as the project root
- [ ] Define frontend environment variables and secrets required by Next.js, Clerk, and Supabase and document them in `config/env/frontend.example.env`
- [ ] Add GitHub Actions workflow file to build, test (Jest), and deploy the frontend to Railway on pushes to the main branch
- [ ] Add a PWA manifest file and iOS-compatible icons under `apps/frontend/public` and reference the manifest in the Next.js document head
- [ ] Configure the frontend CI job in GitHub Actions to run Jest unit and integration test suites and fail the pipeline on any test or lint failure

### Frontend Test Cases
- [ ] Add tests or checks that verify frontend CI/CD pipeline is triggered on pull requests and main-branch pushes and that required environment variables are present

## Phase 2: Authentication (Sign-In/Sign-Up)

### Backend
- [ ] Implement FastAPI endpoints that validate Clerk-authenticated sessions or tokens and expose the current user and tenant context
- [ ] Add backend middleware or dependency that enforces tenant scoping and role-based access control for all protected endpoints
- [ ] Implement FastAPI endpoints for reading and updating the authenticated user profile (name, contact info, preferences)

### Backend Test Cases
- [ ] Write unit tests for auth-related dependencies and endpoints covering valid, invalid, and expired tokens
- [ ] Write integration tests that verify tenant and role restrictions for protected endpoints using multiple tenants and roles
- [ ] Write unit and integration tests for user profile endpoints covering read, update, and unauthorized access cases

### Frontend
- [ ] Implement sign-in and sign-up forms on the SPA landing page with mobile-first, iOS-optimized layout
- [ ] Integrate Clerk JS client in the frontend to perform sign-in, sign-up, and sign-out operations
- [ ] Implement a user profile screen that displays and allows editing of the current user’s profile fields
- [ ] Implement a React context/provider to manage authentication state (logged-in user, loading, error) across the SPA

### Frontend Test Cases
- [ ] Write unit and integration tests for sign-in/sign-up UI covering validation, error, and success states
- [ ] Write integration tests for Clerk auth flows including sign-in, sign-up, sign-out, and error handling
- [ ] Write unit and integration tests for the user profile UI covering display, edit, save success, and save failure
- [ ] Write unit tests for the auth context/provider to verify state transitions and consumer behavior

## Phase 3: Group Management & SMS Announcements

### Backend
- [ ] Implement FastAPI endpoints for creating, reading, updating, and deleting groups backed by Supabase/PostgreSQL tables
- [ ] Implement FastAPI endpoints for adding and removing members in groups, ensuring referential integrity in the database
- [ ] Integrate Twilio Python SDK in a service layer and expose an API endpoint for sending SMS announcements to a set of recipients
- [ ] Implement announcement creation logic that deduplicates recipients per announcement and records announcement metadata and recipient list in the database
- [ ] Implement background or callback handling to update delivery status based on Twilio webhooks and expose an API to query delivery status by announcement
- [ ] Implement audit logging that records all group, membership, and announcement actions in a dedicated audit table with timestamps, user, tenant, and action details

### Backend Test Cases
- [ ] Write unit and integration tests for group CRUD endpoints covering happy paths, validation errors, and authorization failures
- [ ] Write unit and integration tests for member add/remove operations including edge cases such as duplicate members and removal of non-members
- [ ] Write integration tests for the Twilio SMS sending layer using mocked Twilio client responses and error conditions
- [ ] Write unit and integration tests for announcement creation ensuring per-user deduplication and correct persistence of announcement records
- [ ] Write integration tests that simulate Twilio webhook callbacks and verify delivery status updates and query APIs
- [ ] Write unit and integration tests that verify audit log entries are written for each group, membership, and announcement change

### Frontend
- [ ] Implement landing page components that allow an authenticated user to create a new group and add members using mobile-first, iOS-optimized layout
- [ ] Implement a groups dashboard view that lists all groups for the current tenant and provides actions to view, edit, and delete each group
- [ ] Implement member management UI within the dashboard to add, edit, and remove members for a selected group
- [ ] Implement an announcement composer UI that lets an organizer draft a message, select a target group, and trigger the send call to the backend
- [ ] Implement a notification tracking dashboard that displays delivery acknowledgements, failed deliveries, and opt-outs per announcement
- [ ] Implement client-side logic to update delivery status in near real-time using WebSockets or a periodic polling mechanism
- [ ] Apply TailwindCSS styles and layout primitives across landing, dashboard, and notification views and verify they meet basic WCAG 2.1 AA accessibility guidelines

### Frontend Test Cases
- [ ] Write unit and integration tests for group creation UI covering required fields, validation, error display, and success behavior
- [ ] Write unit and integration tests for member add/remove UI covering add, edit, delete, and edge cases such as duplicate or invalid phone numbers
- [ ] Write integration and E2E tests for dashboard flows, including creating a group, adding members, sending an announcement, and viewing results
- [ ] Write integration and E2E tests for the notification tracking dashboard to verify correct rendering of delivered, failed, and opted-out recipients
- [ ] Write E2E tests that verify delivery status updates are reflected in the UI in response to WebSocket messages or polling results
- [ ] Run automated accessibility tests (axe, Lighthouse) against landing, dashboard, and notification pages and create tickets for any failing criteria

## Phase 4: Agent Integration

### Backend
- [ ] Integrate a configurable agent engine client in the backend and expose an API endpoint that accepts a question and context and returns the agent’s answer
- [ ] Implement SMS inbound handling that routes messages from Twilio to either the agent engine or a human responder queue based on routing rules
- [ ] Implement fallback and escalation logic that detects when the agent cannot answer and records an escalation task for a human responder
- [ ] Implement logging of all agent interactions, including questions, answers, confidence scores, and escalation events, into an audit or dedicated agent log table

### Backend Test Cases
- [ ] Write unit and integration tests for the agent Q&A endpoint covering successful answers, error responses, and timeouts
- [ ] Write integration tests for SMS routing logic verifying correct routing between agent and human responder under different scenarios
- [ ] Write unit and integration tests for fallback and escalation logic ensuring that unanswerable questions always produce a recorded escalation
- [ ] Write unit and integration tests that verify all agent interactions are persisted with the expected fields and can be queried for audit

### Frontend
- [ ] Implement a UI view that lists escalated questions assigned to the current organizer and allows entering and submitting responses
- [ ] Implement UI notifications or badges indicating when new agent fallbacks or escalations are available for review

### Frontend Test Cases
- [ ] Write unit and integration tests for the escalated question UI covering list rendering, detail view, and response submission
- [ ] Write integration and E2E tests that verify fallback and escalation notifications appear and update as new escalations are created and resolved

---


## Test Infrastructure & Test Pyramid

### Test Pyramid
- **Unit Tests** (base):
	- Backend: pytest for Python (models, services, API logic)
	- Frontend: Jest + React Testing Library (components, hooks, utils)
- **Integration Tests** (middle):
	- Backend: pytest + httpx (API endpoints, DB, Twilio/Supabase mocks)
	- Frontend: Integration tests for flows (sign-in, group creation, announcement)
- **End-to-End (E2E) Tests** (top):
	- Playwright (or Cypress) for full user journeys (mobile web, iOS focus)
	- Covers landing, auth, group management, SMS send, delivery dashboard
- **Compliance & Accessibility Tests**:
	- Linting, type checks, and security scans in CI
	- Automated accessibility (axe, Lighthouse)
	- GDPR/CCPA compliance checks (opt-in/out, audit logs)

### Test Infrastructure Tasks
- [ ] Set up pytest and coverage for backend (unit/integration)
- [ ] Set up Jest and React Testing Library for frontend (unit/integration)
- [ ] Set up Playwright for E2E (mobile web, iOS viewport)
- [ ] Add accessibility and compliance checks to CI/CD
- [ ] Add test reporting and coverage thresholds to pipelines

---
All tests run in CI/CD (GitHub Actions) and are required for merge. The pyramid ensures fast feedback at the base, with full user journey and compliance validation at the top.
