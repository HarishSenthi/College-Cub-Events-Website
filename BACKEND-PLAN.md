# Production implementation plan

## Suggested stack
Keep the HTML/CSS/JS frontend. Add Node.js + Express or Java Spring Boot for an API, PostgreSQL for transactional data, and an SSO identity provider (Microsoft Entra ID / Google Workspace / institutional LDAP via an identity broker). Replace localStorage with authenticated API calls.

## Authorization
- super_admin: approve club requests, final-clear events, manage venue inventory/settings, inspect aggregate reporting/audit.
- club_representative: manage events, rosters, and applications **only** for explicitly assigned club_id(s).
- faculty_coordinator: approve proposals **only** for clubs linked in faculty_club_assignments.
- student: view published events, RSVP, and apply for recruitment.

Never accept client-supplied role/club ownership. Resolve identity from validated SSO session and look up server-side memberships. Enforce object-level authorization on every API call.

## Venue booking correctness
Use PostgreSQL exclusion constraints on active time ranges in a venue_reservations table (see schema.sql), with every status transition in a transaction. Drafts do not reserve slots. On submission, atomically create a pending hold. Faculty review keeps the hold. Admin approval changes the hold to confirmed and publishes the event in the same transaction. Rejection/cancellation releases it. Implement hold expiry/reconciliation policies for abandoned proposals and a lock timeout/retry strategy.

## Event workflow
DRAFT → PENDING_FACULTY → PENDING_ADMIN → PUBLISHED. Both faculty and admin may return to DRAFT with a reason. Track actor, timestamp, remarks, and approval version. A significant edit after approval should invalidate prior approvals. Budget/equipment/security requests are part of the proposal snapshot for review.

## SSO and digital signoff
Integrate campus OIDC/OAuth SSO with email/student-number claims mapping after validating signatures, audience, issuer and token expiry. Obtain university IAM agreement before selecting Microsoft 365 vs Google Workspace vs LDAP. For a genuine cryptographic signature use an approved document/e-sign provider and immutable evidence; clicking Approve with a username is merely an auditable approval, not itself a digital signature.

## Core endpoints (illustrative)
GET /api/events?category=&date=&venue=&entry=
GET /api/clubs/:id
POST /api/clubs/:id/applications
POST /api/events (club-scoped)
POST /api/events/:id/submit
POST /api/events/:id/faculty-approve
POST /api/events/:id/admin-approve
POST /api/events/:id/reject
POST /api/events/:id/rsvp
GET /api/venues/availability?date=&start=&end=
GET /api/admin/analytics

Also add notifications, rate limits, personal-data retention/deletion policy, CSRF defenses appropriate to session style, security logging, and institution-approved privacy notice.
