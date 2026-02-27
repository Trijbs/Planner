# Weekplanner Web App v1.1

Dutch-first weekplanner PWA with:
- PIN login (6 digits)
- Week tasks (`titel`, `info`, `deadline`, `prioriteit`, `status`)
- Hour blocks (`Uurblokken`)
- Day-linked hour registration (`Urenregistratie`) with weekly/project totals
- Automatic history notes on create/update/delete
- Excel import (`.xlsx`) + Google Drive hourly sync
- CSV export of current week

## Stack
- Next.js 16 + TypeScript + Tailwind
- Local JSON data store (development default)
- Supabase SQL migrations included in `supabase/migrations/`
- Vercel cron config in `vercel.json`

## Local Run
```bash
npm install
cp .env.example .env.local
npm run dev
```
Open [http://localhost:3000](http://localhost:3000).

## Environment Variables
Set in `.env.local`:
- `SUPABASE_URL`
- `SUPABASE_SERVICE_ROLE_KEY`
- `TOKEN_ENCRYPTION_KEY`
- `CRON_SECRET`
- `GOOGLE_CLIENT_ID`
- `GOOGLE_CLIENT_SECRET`
- `GOOGLE_REDIRECT_URI`
- `GOOGLE_DRIVE_FOLDER_ID`

Optional:
- `LOCAL_DB_PATH`

## API Overview
- `POST /api/auth/pin/setup`
- `POST /api/auth/pin/login`
- `POST /api/auth/pin/logout`
- `GET /api/weeks/current`
- `POST /api/weeks/:id/tasks`
- `PATCH|DELETE /api/tasks/:id`
- `POST /api/weeks/:id/hour-blocks`
- `PATCH|DELETE /api/hour-blocks/:id`
- `GET|POST /api/weeks/:id/hours`
- `GET /api/weeks/:id/hours/summary`
- `PATCH|DELETE /api/hours/:id`
- `POST /api/import/manual`
- `POST /api/import/sync`
- `POST /api/integrations/google-drive/connect`
- `GET /api/integrations/google-drive/callback`
- `GET /api/export/csv?weekId=...`
- `POST /api/cron/sync-drive` (Bearer `CRON_SECRET`)

## Supabase Setup
1. Create Supabase project.
2. Run SQL files under `supabase/migrations/`.
3. Set `SUPABASE_URL` and `SUPABASE_SERVICE_ROLE_KEY` in `.env.local`.
4. The app auto-switches to Supabase runtime repository when those env vars are present.
5. If missing, it falls back to local JSON storage for development.

## Tests
```bash
npm run test
```
Covers:
- Excel parser
- Hours summary math
- Change diff behavior

## Notes
- UI is Dutch and Monday-Friday by default.
- PWA includes service worker + offline mutation queue (IndexedDB).
