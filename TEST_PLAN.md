# FactoryOps AI 1.0 test plan

## Build checks
- [ ] `npm install` completes without dependency errors.
- [ ] `npm run build` completes successfully.
- [ ] `npm run preview` serves the production build.

## Local mode
- [ ] Open app with no `.env`; local workspace status is shown.
- [ ] Create a BOM record with a part number and status Approved.
- [ ] Create production record with planned, actual, good and scrap quantities.
- [ ] Create quality NCR, cost record and maintenance record.
- [ ] Refresh browser; local records remain.
- [ ] Search records; export workbook; import a sample workbook.
- [ ] Edit and delete records.
- [ ] Switch EN/FR/AR/DE; Arabic uses RTL direction.
- [ ] Check desktop, tablet and Android viewport layouts.
- [ ] Install PWA from HTTPS Chrome and relaunch.

## Cloud mode (requires Supabase project)
- [ ] Apply `supabase/schema.sql`.
- [ ] Create account and sign in.
- [ ] Create company using `create_company` RPC.
- [ ] Create/read/update records.
- [ ] Sign out and sign in again; records persist.
- [ ] Use a second account with no membership; it cannot read first company's records.
- [ ] Verify a user cannot insert/update a record for a company they do not belong to.
- [ ] Verify only admin/manager can delete records under RLS.
- [ ] Test expired session and email-confirmation flow.

## Known limitations
- This source package has not been connected to your Supabase instance, deployed, or tested against your production environment. Do not treat cloud mode as validated until the cloud tests above pass.
- Role-aware controls and invitations are not fully implemented in the frontend; RLS is the enforcement layer.
- Offline writes and background synchronization are not implemented.
