# CIT ClubHub — interactive multi-club prototype

Open `index.html` using VS Code's Live Server extension, or directly in a modern browser. Requires no installation, API key or internet connection.

## Features

- Public event feed with keyword, category, date, venue, and free/paid filters
- Individual club microsites with mission, committee, social handle, recruiting status, and events
- Demo student RSVP and club-joining forms
- Club representative demo: create drafts, submit for review, view registrations, decide membership applications, edit assigned club
- Faculty mentor demo: review assigned clubs' submissions, simulated digital signoff, reject to draft
- Administrator demo: final approval and publishing, club approval requests, all-events dashboard, analytics, audit log, recruitment switch
- Campus venue calendar with overlapping-interval conflicts checked when submitting an event and at approval
- LocalStorage persistence within the same browser/profile; use admin reset to restore sample data
- CIT branding from supplied image

## Demonstrate event approval

1. Switch the Preview as selector to **Club representative**.
2. Visit My workspace > Create event, save a draft. Choose an available venue/date/time.
3. In My club events, click Send for faculty review. The venue now shows a pending hold.
4. Switch to **Faculty mentor** > Faculty approvals > Digitally sign off.
5. Switch to **Super administrator** > Admin approvals > Clear & publish.
6. Return to Discover. The approved event is now visible in All events.

## Important limitations

**This is a standalone frontend simulation, not a production multi-user service.** The role selector is a demo impersonation control, not authentication or authorization. All data stays in localStorage on one browser profile. Edits are not shared across computers. The faculty signoff is a recorded demo action, not a legally valid cryptographic digital signature. Venue conflict protection is scoped to one browser and cannot guarantee cross-user locking. The Paid/Free field is metadata; no checkout or pricing/payments exist. No SSO, email notifications, identity federation, database, server audit assurance or real security controls are connected.

For a real deployment: use a server backend, relational database, server-verified role/club permissions, OIDC/OAuth 2.0 with CIT's identity provider, email notifications, and transactional venue reservations. See `BACKEND-PLAN.md` and `schema.sql` for a proposed implementation.

## Offline event artwork
The six illustrations in `assets/events/` are bundled SVG illustrations tailored to each demo event. They require no internet connection. Newly created club events use a matching club-category illustration.


## Latest update
- Registration form updated for **teams of 4**.
- Cultural Fest artwork updated to a **music-themed design**.


## Preview update
- Cultural Fest now uses the supplied **music-themed concert image**.
- Registration remains configured for **4 team members**.


## Cinematic event artwork update
Five new locally bundled cinematic event banners added for AI Innovation, Web Development, Cricket, Green Campus, and Industry Expert Talk. Cultural Fest original image has been kept **unchanged**. The team-of-four registration remains in place.
