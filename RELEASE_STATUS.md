# Release status — 2026-11-21

## Completed in this source package

- [x] Responsive public website and visual identity assets
- [x] Five language content dictionaries (Afaan Oromo, Amharic, Arabic, Somali, English)
- [x] Arabic RTL document direction
- [x] Public appointment request API with consent and validation
- [x] PWA manifest, 192px/512px PNG icons and service worker shell cache
- [x] Owner authentication bootstrap, hashed passwords and protected API routes
- [x] Patient records and appointment-to-patient conversion
- [x] Appointment calendar with Gregorian and Ethiopian date labels
- [x] Consultation and follow-up record endpoints and owner forms
- [x] Product catalog, supplier records, purchase receipt and batch quarantine
- [x] Explicit batch approval gate before sale
- [x] Sale/payment transaction, inventory decrement and full refund stock restoration
- [x] Expenses, basic finance summary and audit log
- [x] Dockerfile, Railway config, environment example and deployment documentation
- [x] Automated API tests: 8/8 passed in this environment

## Not completed / not verified

- [ ] Actual clinic HD video asset; the homepage has a ready video slot and SVG poster only
- [ ] Real photographs of the clinic, owner or products with owner permission
- [ ] Verified exact map pin / geocoded clinic location
- [ ] SMS and WhatsApp Business API notifications (WhatsApp click-to-chat is included)
- [ ] Online payment provider integration and provider-side webhook verification
- [ ] Product document file upload/storage workflow and regulatory document verification
- [ ] Staff accounts and per-role permissions beyond the single owner account
- [ ] Full clinical workflow sign-off by the clinic's authorized practitioner
- [ ] Multi-tenant SaaS, ODAA platform admin and subscription billing
- [ ] Full double-entry accounting, supplier settlements, partial refunds and tax reporting
- [ ] Automated off-site backup schedule and tested disaster recovery
- [ ] Independent security penetration test and privacy/legal review
- [ ] Browser/device visual QA on actual Android phones and low-bandwidth mobile data
- [ ] Actual Railway deployment, custom domain, HTTPS and production smoke test

## Verification executed

Command: `npm run check`

Result: 8 tests passed, 0 failed. Node syntax checks passed for `server.mjs`, `public/app.js` and `public/admin.js`.

Coverage: health endpoint, unauthenticated API protection, public appointment validation, owner login, patient creation, dashboard data, product access control, appointment conversion, product/batch approval, sale, stock decrement, refund, stock restoration, public homepage, admin page and PWA manifest.

Not covered: browser automation, mobile visual QA, load testing, independent security testing, legal/regulatory compliance, real payment provider callbacks, production deployment, or real patient-data workflows.
