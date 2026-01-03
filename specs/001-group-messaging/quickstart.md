# Quickstart: Group Messaging SaaS (Phase 1)

## Prerequisites
- Node.js (LTS) or Python 3.11+
- PostgreSQL
- Twilio account (or SMS provider)
- Docker (optional, for local dev)

## Setup
1. Clone the repository and checkout the `001-group-messaging` branch.
2. Install dependencies (`npm install` or `pip install -r requirements.txt`).
3. Configure environment variables for DB, JWT secret, and Twilio credentials.
4. Run database migrations to set up tables with RLS for tenant isolation.
5. Start the backend server (`npm run start` or `uvicorn main:app`).
6. Run tests (`npm test` or `pytest`).

## Usage
- Register a new user (organizer) for a tenant.
- Create groups and add people (with phone numbers).
- Send announcements to groups.
- Check delivery status and audit logs.
- Test opt-out and subscription tier limits.

## Compliance & Security
- All data is tenant-isolated and encrypted.
- Opt-in/opt-out and consent management enforced.
- All actions logged for audit and compliance.
- Automated tests cover edge and failure cases.

