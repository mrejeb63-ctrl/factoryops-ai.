# FactoryOps AI 1.0

Responsive React + TypeScript PWA for industrial operations. Includes BOM, Production, Quality, Cost, Maintenance, rule-based AI insights, four UI languages (English, French, Arabic/RTL, German), Excel import/export, local workspace, and optional Supabase cloud authentication/multi-company storage.

## Important scope and honesty
- This is a functional MVP, not a certified ERP/MES/QMS.
- The AI Assistant in v1.0 uses transparent deterministic rules; it does not call a generative AI API.
- Without Supabase configuration, records are stored in browser localStorage on that device/browser only. This is not multi-user cloud storage and is not a backup.
- Cloud sign-up, login, company creation, and record synchronization require your own Supabase project and running SQL schema.
- Company membership and records are protected with RLS policies. Do not put a Supabase service-role key in the browser.
- Invite/member management, payment collection, email notifications, external ERP integrations, advanced audit trails and production-grade backup/monitoring are not included in this MVP.

## Requirements
Node.js 20 LTS or later and npm. Android users can use a cloud IDE or a computer for the initial build. Once deployed, users install the PWA from Chrome's browser menu.

## Run locally
1. Extract the ZIP.
2. Install Node.js 20+.
3. In this folder run:
   ```bash
   npm install
   cp .env.example .env
   npm run dev
   ```
4. Open the local URL printed by Vite.

The app works in local workspace mode without Supabase.

## Enable cloud authentication and multi-company data
1. Create a free project at https://supabase.com.
2. Open Project Settings > API. Copy the Project URL and public anon key.
3. Copy `.env.example` to `.env` and set:
   `VITE_SUPABASE_URL=https://...supabase.co`
   `VITE_SUPABASE_ANON_KEY=your-public-anon-key`
4. In Supabase SQL Editor, run all of `supabase/schema.sql`.
5. In Authentication > URL Configuration, set your local and deployed site URLs as allowed redirect URLs.
6. Restart the dev server or rebuild after changing environment variables.
7. In the app, create an account, sign in, then create a company workspace.

Never use the Supabase `service_role` key in `.env` for this frontend. Only the anon/publishable key belongs in the browser; RLS is mandatory.

## Build
```bash
npm run build
npm run preview
```
The production files are generated in `dist/`.

## Free deployment: Cloudflare Pages
1. Push this project to a GitHub repository.
2. In Cloudflare Dashboard > Workers & Pages, create a Pages project and connect the repository.
3. Build command: `npm run build`
4. Build output directory: `dist`
5. Add environment variables `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY` in Pages settings, if cloud mode is enabled.
6. Add the deployed URL to Supabase Authentication allowed redirect URLs.
7. Redeploy after environment changes.

Alternative static hosting: GitHub Pages (configure Vite base path if deploying to a project subpath).

## PWA / Android
Deploy over HTTPS. Open the site in Chrome on Android > menu > Install app / Add to Home screen. The service worker caches the app shell. This MVP's data writes require an active session/network for cloud mode; offline record-write queue/sync is not implemented.

## Modules
- **BOM:** part number, description, revision, quantity, unit, master BOM, validation status. Status choices include New, Trial, Approved, Released, Blocked. BOM accuracy dashboard is based on records explicitly marked Approved.
- **Production:** production line, planned/actual quantity, good/scrap parts, shift, cycle time. Dashboard calculates achievement and quality yield.
- **Quality:** NCR, defect category, severity, root cause, corrective action, owner, due date.
- **Cost:** cost category, quantity, unit cost, currency, cost center, reference.
- **Maintenance:** equipment, maintenance type, downtime, technician, next due date, work order.
- **AI Assistant:** rule-based production/BOM/quality/maintenance observations.

## Data import/export
- Use Import Excel on any module. First worksheet rows are imported; columns are preserved in the record details JSON.
- Export produces an `.xlsx` workbook of the currently filtered module/list.
- Keep a backup of source workbooks before importing.

## Security and production-readiness checklist
Before real customer use:
- Review RLS policies with an independent security reviewer.
- Add a company invitation flow and role-aware UI controls.
- Configure email confirmation, strong password policy and MFA as appropriate.
- Configure database backups/PITR on a paid plan and test restores.
- Add audit logs for record changes and immutable history for engineering approvals.
- Add rate limiting, monitoring, error reporting and privacy/retention policies.
- Perform tenant-isolation tests using two separate accounts.
- Review local laws, customer contracts, GDPR obligations where applicable, and data residency requirements.
- Do not upload confidential customer BOMs to third-party AI services without authorization.

## Tests
`npm run build` performs TypeScript checking and production bundling. Manual acceptance tests are listed in `TEST_PLAN.md`.
