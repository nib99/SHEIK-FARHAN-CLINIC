# First deployment checklist

## Before deploy
- [ ] Clinic owner approves clinic name, branding and location wording.
- [ ] Verify practitioner/facility authorization and which services can be advertised.
- [ ] Verify every product's market authorization and batch documentation before sale.
- [ ] Review privacy notice, consent, data controller, retention and patient rights with qualified local adviser.
- [ ] Choose owner email and create a unique password of at least 14 characters.
- [ ] Prepare licensed HD MP4 and owner-approved clinic/product photos, or launch with the poster only.
- [ ] Verify phone numbers, WhatsApp number and Facebook page.

## Railway
- [ ] Push source to a private GitHub repository.
- [ ] Create Railway service from repository (Dockerfile).
- [ ] Attach persistent volume at `/data`.
- [ ] Set `NODE_ENV=production`, `PORT=3000`, `HOST=0.0.0.0`, `DATA_DIR=/data`.
- [ ] Set `ADMIN_EMAIL` and strong `ADMIN_PASSWORD` before first start.
- [ ] Keep one service replica only.
- [ ] Add custom domain and verify HTTPS.
- [ ] Verify `/api/health`.
- [ ] Sign in at `/admin` and test with fictitious records.
- [ ] Test appointment, product, batch, sale, refund and restore workflows.
- [ ] Configure encrypted off-site backups and test a restore.
- [ ] Perform owner acceptance and local regulatory/privacy sign-off before real data.

## Do not launch if
- The owner login is not secure.
- HTTPS or persistent volume is missing.
- Backups have not been tested.
- The clinic cannot explain who can access patient data.
- Product authorization is unknown.
- The practitioner/facility permissions are not verified.
