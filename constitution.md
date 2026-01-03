# Constitution: EventMan Project

**Last Updated:** December 30, 2025

## Principles

- **User-Centric Design:** Prioritize the needs, privacy, and experience of event organizers and attendees.
- **Transparency:** All workflows, data handling, and communications must be clear and auditable.
- **Security & Privacy:** Adhere to industry best practices for data protection, including encryption, access controls, and regular audits. All sensitive data must be protected with end-to-end encryption where technically feasible.
- **Compliance:** Meet or exceed all relevant legal and regulatory requirements (e.g., GDPR, CCPA, CAN-SPAM).
- **Quality First:** All features must be testable, measurable, and deliver clear value to users.
- **Continuous Improvement:** Regularly review and refine processes, code, and user feedback.

## Workflow

1. **Specification:** All new features require a clear, testable specification before planning or implementation.
2. **Planning:** Features are broken down into tasks with acceptance criteria and dependencies identified.
3. **Implementation:** Code is written following best practices, peer-reviewed, and tested before merging.
4. **Testing:** Automated and manual tests are required for all user-facing and critical backend features.
5. **Review:** Regular code, security, and compliance reviews are conducted.
6. **Deployment:** Only tested and reviewed code is deployed to production. Rollback plans are mandatory.
7. **Feedback Loop:** User and stakeholder feedback is collected and used to inform future improvements.

## Regulatory & Test Practices

- **Data Protection:** All personal data is stored securely, with access limited to authorized personnel. Data minimization and retention policies are enforced. All data in transit and at rest must be encrypted. End-to-end encryption is required for all communications and sensitive data exchanges, ensuring only intended recipients can access the content. Tenant data isolation is tested and verified regularly, including during automated and manual security reviews.
- **Consent Management:** Users must provide explicit consent for communications and data processing. Opt-out mechanisms are always available.
- **Audit Trails:** All critical actions (group creation, messaging, agent responses) are logged for traceability.
- **Incident Response:** A documented process exists for handling data breaches or compliance incidents, including user notification and remediation.
- **Testing Standards:**
  - Unit, integration, and end-to-end tests are required for all features.
  - Edge cases and failure scenarios must be explicitly tested.
  - Success criteria are defined in specifications and must be met before release.
- **Accessibility:** The application must be usable by people with disabilities, following WCAG guidelines.
- **Documentation:** All workflows, data flows, and compliance measures are documented and kept up to date.
- **Multi-Tenancy & Data Isolation:** The platform is a multi-tenant SaaS. Each tenant's data is logically and physically separated to prevent unauthorized access or data leakage between tenants. All data access, processing, and storage must enforce strict tenant boundaries at every layer.
- **Subscription Models:** The system supports free, silver, and premium subscription tiers. Features and resource limits are enforced per tenant according to their subscription level. Upgrades and downgrades are handled securely and with minimal disruption.

## Assumptions

- All team members are trained in privacy, security, and compliance best practices.
- The project will adapt to new regulations and standards as they arise.
- The platform is designed for multi-tenant SaaS operation, with strict tenant data isolation and subscription management.

---

This constitution is a living document and should be reviewed quarterly or after any major regulatory change.
