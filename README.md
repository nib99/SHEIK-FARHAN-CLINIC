# Sheik Farhan Traditional Medicine Clinic

A mobile-first, multilingual clinic website and first-clinic operations portal for **Sheik Farhan Muhammed Adem**, Hirna Town, Oromia, Ethiopia. Built with Node.js 22, the built-in SQLite driver, vanilla HTML/CSS/JavaScript, and no runtime npm dependencies.

## Included

- Public clinic website with five language UI dictionaries: Afaan Oromo, Amharic, Arabic (RTL), Somali and English.
- Responsive navigation, clinic details, telephone links, WhatsApp contact and Facebook link.
- Online appointment request form with consent, server validation and basic IP rate limiting.
- Installable PWA manifest, PNG app icons, service worker and offline shell caching.
- Owner login with scrypt password hashing and HttpOnly/SameSite session cookie.
- Owner dashboard with patient counts, appointment requests, revenue/expense summary and appointment calendar.
- Patient registration, appointment request review, appointment-to-patient conversion, consultation records and follow-up records.
- Herbal product catalog with regulatory status field, suppliers, batch receipt, quarantine and explicit batch approval.
- Inventory ledger, sale checkout, payment record, full refund workflow and stock restoration.
- Expenses, finance summary endpoint and audit log.
- Dockerfile and Railway configuration for a single-instance deployment with a persistent volume.
- Automated API tests using Node's built-in test runner.

## Important scope and release status

This is a **single-clinic first deployment candidate**, not yet a multi-clinic SaaS release. The schema includes a clinic identifier for later tenant migration, but this version does not implement tenant onboarding/isolation, multiple staff accounts, permission matrices, platform super-admin, or subscription billing.

The application has not received an independent security audit or Ethiopian healthcare/legal compliance approval. Do not enter real patient medical records or begin clinical operations until the clinic owner and qualified local advisers review licensing, data protection, consent, retention, hosting and access controls. The initial owner account is the only supported account. Do not expose the admin portal publicly without HTTPS and a strong password.

The homepage is video-ready but **no real clinic HD MP4 is included**. A video of the actual clinic or owner must be supplied with consent and placed at `public/media/clinic-hero.mp4`. Until then, the designed SVG poster is displayed. The `<video>` element uses `object-fit: contain` so the complete source frame remains visible rather than being cropped. A poster background fills unused space.

The logo and app icons are original vector artwork made for this project. Confirm the owner approves the final brand before public launch.

## Requirements

- Node.js `>=22.12.0` (Node 22's built-in `node:sqlite` emits an experimental warning in some Node 22 releases).
- Docker for container deployment, or Node.js directly.
- Persistent disk for `DATA_DIR`.

No `npm install` is required because the project has no third-party runtime dependencies.

## Run locally

```bash
cp .env.example .env
# Set ADMIN_EMAIL and ADMIN_PASSWORD in .env. Use a unique password of at least 14 characters.
# Set DATA_DIR to a writable persistent folder if desired.
node server.mjs
```

Open:
- Public site: `http://localhost:3000/`
- Owner portal: `http://localhost:3000/admin`
- Health: `http://localhost:3000/api/health`

The owner account is bootstrapped from `ADMIN_EMAIL` and `ADMIN_PASSWORD` only when the database has no users. Keep `.env` private. Never commit secrets.

## Run automated tests

```bash
npm run check
```

Tests start an isolated temporary server/database and cover health, unauthenticated access, public booking validation, owner login, patient creation, appointment conversion, product batch approval, sale checkout, stock decrement and full refund stock restoration.

## Deploy to Railway (recommended for this release)

This release uses a local SQLite database file, so deploy as **one service instance** with a persistent volume. Do not scale replicas horizontally. Do not deploy this SQLite build to Vercel serverless functions because serverless filesystems are ephemeral and the process/session model is not suitable.

1. Create a private GitHub repository and upload this project.
2. In Railway, create a project from the GitHub repository. Railway detects the `Dockerfile`.
3. Add a persistent volume mounted at `/data`.
4. Set variables:
   - `NODE_ENV=production`
   - `PORT=3000`
   - `HOST=0.0.0.0`
   - `DATA_DIR=/data`
   - `ADMIN_EMAIL` = the owner's login email
   - `ADMIN_PASSWORD` = a long unique password (14+ characters)
   - `CLINIC_WHATSAPP=251915001743`
5. Deploy one replica only.
6. Verify `/api/health` returns `{ "ok": true, "database": "connected" }`.
7. Open `/admin`, sign in and test using fictitious data first.
8. Configure a custom domain and HTTPS before public use.
9. Configure scheduled off-platform backups and test restoration before collecting real data.

A Railway deployment is not performed by this source ZIP. It must be connected to the owner's Railway account and configured there.

## Deploy with Docker

```bash
docker build -t sheik-farhan-clinic .
docker run --rm -p 3000:3000 \
  -e NODE_ENV=production \
  -e PORT=3000 \
  -e HOST=0.0.0.0 \
  -e DATA_DIR=/data \
  -e ADMIN_EMAIL='owner@example.com' \
  -e ADMIN_PASSWORD='REPLACE_WITH_A_LONG_RANDOM_PASSWORD' \
  -v clinic-data:/data \
  sheik-farhan-clinic
```

## Video and media setup

Supply an owner-approved, licensed MP4. Recommended: H.264 video, 1920×1080, 16:9, 24–30 fps, AAC audio (muted autoplay is used), 15–30 seconds, optimized to a modest file size for Ethiopian mobile networks. Copy it to:

`public/media/clinic-hero.mp4`

The source video is rendered with `object-fit: contain` to keep every frame visible. On narrow screens, letterboxing may appear; the poster artwork fills the surrounding hero background. Do not use footage or portraits without permission.

## Clinic information included

- Owner: Sheik Farhan Muhammed Adem
- Location: Hirna Town, Oromia, Ethiopia
- Directions supplied by the clinic: on the road toward Hirna Hospital, approximately 200 metres downhill from the hospital.
- Phone: 0915001743 / 0915030381
- Facebook: `farhan.mohammed.79788`

The map button intentionally opens a town-level search because exact clinic coordinates have not been verified. Confirm the direction text with the owner before publishing.

## Currency and financial values

Financial API values are stored as integer minor units (cents/santim); the admin form labels these as ETB cents. The current UI is an operational first release, not a full accounting package. Purchase payables, supplier settlements, tax handling, cash drawer close, partial refunds and full double-entry accounting are not implemented.

## Security and operational limitations to close before live clinical use

- Only one owner login exists. Staff accounts and per-role clinical permissions are not implemented.
- Sessions and rate-limit counters are in process memory; use one instance. Sessions are invalidated by restart.
- SQLite is suitable for this single-instance pilot only. Move to PostgreSQL with tenant isolation before multi-clinic SaaS or horizontal scaling.
- Configure encrypted storage/volume, managed backups, restore drills, retention/deletion policy, incident response and access reviews.
- Review appointment consent wording and privacy notice with the clinic's data controller/legal adviser.
- The public booking form is not an emergency channel and intentionally does not collect medical histories.
- Notifications are not integrated with SMS/WhatsApp APIs; WhatsApp is a click-to-chat link only.
- Payment integrations are not connected. Cash and manually confirmed payment methods are recorded; do not treat screenshots as provider confirmation.
- Product registration and practitioner/facility permissions are not checked by software. Only authorized staff can decide whether documentation is valid; product batches default to quarantine and cannot be sold until product status and batch are approved.
- No clinical diagnosis, treatment recommendation or dosage engine is provided.
- Ethiopian calendar display in the owner calendar is a client-side date label; confirm date conversion edge cases with local users before relying on it for official records. Database dates remain Gregorian ISO dates.
- No automated backup scheduler is included. A manual backup utility is provided below.

## Manual SQLite backup

With the app stopped or during a controlled maintenance window:

```bash
DATA_DIR=/data node scripts/backup.mjs /data/backups/clinic-backup.sqlite
```

Keep encrypted copies off the hosting volume and periodically test restore to a separate environment.

## API quick reference

Public:
- `GET /api/health`
- `POST /api/public/appointments`

Authenticated:
- `POST /api/auth/login`, `POST /api/auth/logout`, `GET /api/auth/me`
- `GET /api/dashboard`, `GET /api/reports/finance`
- CRUD: `/api/patients`, `/api/appointments`, `/api/consultations`, `/api/followups`, `/api/products`, `/api/suppliers`, `/api/expenses`
- `POST /api/appointments/:id/convert`
- `GET /api/inventory`, `POST /api/purchases`, `POST /api/batches/:id/approve`
- `GET/POST /api/sales`, `POST /api/sales/:id/refund`
- `GET /api/audit-logs`

## Verification record

The included `npm run check` suite was executed in the build environment. It verifies the listed API workflows only. It does not replace a browser-based cross-device QA pass, independent penetration test, production deployment test, regulatory review, or owner acceptance test.
