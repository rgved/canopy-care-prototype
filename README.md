# Canopy · Quarantine facility prototype

A responsive, functional prototype for a 74-patient quarantine and treatment facility. Uses fictional patients only. No paid services, backend credentials or environment variables are needed.

## Run

Open `index.html`, or run `python3 -m http.server 3000` and visit localhost:3000.

Deploy the repository to Vercel as **Other** (static site), with no build command and output directory `.`.

## Demonstrate the workflow

1. **Nurse:** filter patients needing temperature, record a reading, view timestamp and staff attribution. Further same-day readings require a re-check reason.
2. **Doctor:** switch the demo role, inspect the ward board and temperature curve, add notes and log a visit. Missing temperatures display a warning; urgent care is not blocked.
3. **Discharge:** sample patients with 3 completed fever-free days enter a review queue automatically. Doctor approves, admin records departure, capacity becomes available.
4. **Admin:** admit a fictional patient into a free place. Admissions are blocked at 74 active patients. Allocation slots represent patient capacity, not 74 separate rooms.
5. **Head doctor:** inspect mortality/recovery with denominator, cohort window and benchmark; edit labelled clinical demo settings; void a reading with a reason; view activity or reset data.
6. Record a deceased outcome as doctor to demonstrate an outcome distinct from recovered discharge.

Use the in-app Demo guide for a walkthrough.

## Alignment with the scenario and design answers

- Four distinct role workspaces: nurse, doctor, clerical admin and head doctor.
- Temperature measurement and doctor visit are separate daily tasks, with overdue indicators and daily checklist history.
- All readings retained; reasoned re-checks and audited head-doctor voids.
- Doctor and head-doctor temperature entries retain their actual staff role.
- Doctor approval and admin completion are separate. Eligibility is rechecked; new fever or corrections remove stale approval.
- Missing observations are insufficient evidence, not fever-free days.
- Mortality = deaths / (deaths + recovered discharges), excluding active patients. Window: last rolling 30 days. Raw counts and small-sample cautions shown.
- Q3's sprint exercise has no defined mapping from A–J to app features. Its answer remains A, B, D, F, G, H, I, J: effort 8.3, value 22, total sprint effort 23.3/25. No invented feature mapping is used.

## Explicit prototype limits

This is **not a production clinical system**. Do not enter real patient information.

- Demo role switching is not individual staff login, real authentication or server-enforced authorization. Client role guards illustrate the workflow only.
- Persistence is `localStorage`, local to a browser/device. Other tabs receive storage updates, but devices do not share a database. Simultaneous writes are not transactional and may conflict. No production concurrency or multi-user allocation guarantee is claimed.
- Activity history is a local demonstration, not a secure immutable clinical audit log. There are no server backups, access logging, offline sync queue, password sessions or integrations.
- Demo assumptions: fever is ≥38°C; three full days means three **completed** calendar days in Asia/Kolkata. Today never counts as a completed day. The original brief leaves these definitions open. Clinician approval is required before real use. A fever today blocks discharge even when the previous three days qualify.
- Unusual readings (<34 or >43°C) require confirmation, not rejection; this is a demo typo-check, not a medically approved safety range.
- Discharge notifications are an in-app queue, not external push/SMS delivery.
- New measurements use current time for measured/entered timestamps; there is no backdated entry UI. C is the fixed entry unit.
- Readmissions can be represented as new admissions with a new ID, but person-level identity matching and outcome amendment workflows are not implemented.

Production would need approved clinical rules, a shared database, authenticated accounts and server RBAC, transactions, idempotency, reviewed privacy controls, backups and security validation.

## Tests

`npm install && npx playwright install chromium && npm test`

Automated checks cover nurse entry and repeat, browser persistence, doctor visit, discharge approval/completion, admission after a place is freed, audit entries, mortality counts, full-day/fever/missing-day rule cases and mobile overflow. Screenshots are produced locally by the test; they are not runtime dependencies.

## Implementation

Dependency-free static HTML/CSS/JavaScript for deployment. Playwright is a development-only dependency. No copied reference agent instructions or patient data are included.
