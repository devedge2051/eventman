# EventMan Constitution

## Core Principles

### I. Privacy-First
All user and attendee data must be handled with strict privacy controls. Data collection is minimized, and all personal information is stored securely. SMS and group communications must comply with relevant privacy regulations (e.g., GDPR, CCPA). Consent is required for all communications.

### II. Modular & Testable Design
Each feature is implemented as a self-contained, independently testable module. All modules must be documented and have a clear, user-driven purpose. No organizational-only modules are permitted.

### III. API-First & CLI Support
All core functionality is exposed via a documented API. Where appropriate, a CLI interface is provided for administrative tasks. Text in/out protocols are supported for integration and debugging.

### IV. Test-Driven Development (NON-NEGOTIABLE)
Tests must be written and approved before implementation. The Red-Green-Refactor cycle is strictly enforced. Automated tests (unit, integration, and end-to-end) are required for all features, including SMS delivery and agent Q&A.

### V. Observability & Simplicity
Structured logging and monitoring are required for all user-facing and backend services. The system must be simple, maintainable, and follow YAGNI principles. Versioning follows MAJOR.MINOR.PATCH format. Breaking changes require migration plans.

## Security & Compliance
- All data in transit and at rest must be encrypted.
- SMS and messaging integrations must comply with carrier and regional regulations.
- Regular security reviews and dependency vulnerability scans are mandatory.
- Data retention and deletion policies must be documented and enforced.

## Development Workflow & Quality Gates
- All code changes require peer review and must pass automated tests.
- CI/CD pipelines enforce linting, testing, and security checks.
- Feature specifications, plans, and tasks must be documented and reviewed before implementation.
- Regulatory and privacy compliance is verified before deployment.

## Governance
The constitution supersedes all other practices. Amendments require documentation, approval, and a migration plan. All PRs and reviews must verify compliance with this constitution. Complexity must be justified. Use the project guidance file for runtime development guidance.

**Version**: 1.0.0 | **Ratified**: 2025-12-30 | **Last Amended**: 2025-12-30
