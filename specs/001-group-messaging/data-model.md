# Data Model: Group Messaging SaaS
- All actions must be logged for audit.
- Opted-out persons must not receive messages.
- Announcements cannot be sent to empty groups.
- Groups must have at least two members to send announcements.
- Phone numbers must be E.164 format and unique per tenant.
## Validation Rules

- Announcement 1:N DeliveryReceipt
- Group N:M Person (via GroupMembership)
- Tenant 1:N User, Group, Person, Announcement
## Relationships

- **delivered_at**: datetime (nullable)
- **status**: enum (pending, delivered, failed)
- **person_id**: UUID (FK → Person)
- **announcement_id**: UUID (FK → Announcement)
- **id**: UUID (PK)
### DeliveryReceipt

- **delivery_status**: enum (pending, delivered, failed, partial)
- **sent_at**: datetime
- **sent_by**: UUID (FK → User)
- **message**: string
- **group_id**: UUID (FK → Group)
- **tenant_id**: UUID (FK → Tenant)
- **id**: UUID (PK)
### Announcement

- **created_at**: datetime
- **added_by**: UUID (FK → User)
- **person_id**: UUID (FK → Person)
- **group_id**: UUID (FK → Group)
- **id**: UUID (PK)
### GroupMembership

- **updated_at**: datetime
- **created_at**: datetime
- **opted_out**: boolean (default: false)
- **phone_number**: string (E.164, unique per tenant)
- **name**: string
- **tenant_id**: UUID (FK → Tenant)
- **id**: UUID (PK)
### Person

- **updated_at**: datetime
- **created_at**: datetime
- **created_by**: UUID (FK → User)
- **description**: string
- **name**: string
- **tenant_id**: UUID (FK → Tenant)
- **id**: UUID (PK)
### Group

- **updated_at**: datetime
- **created_at**: datetime
- **role**: enum (organizer, admin)
- **password_hash**: string
- **email**: string (unique per tenant)
- **tenant_id**: UUID (FK → Tenant)
- **id**: UUID (PK)
### User

- **updated_at**: datetime
- **created_at**: datetime
- **subscription_tier**: enum (free, silver, premium)
- **name**: string
- **id**: UUID (PK)
### Tenant

## Entities


