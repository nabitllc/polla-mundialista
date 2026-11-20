> Cloud/sandbox sessions (claude.ai/code): clone `github.com/michsaenz/Mich-Brain2` and follow its `BOOTSTRAP.md` before working. Local sessions ignore this - the global shim already covers it.

# Polla Mundialística — repo guidance for AI sessions

World Cup prediction pool for a group of friends: secret per-person predictions that lock at
kickoff, automatic points and standings, group tables, a self-filling knockout bracket and
Telegram alerts. The UI and the code comments are in Spanish. This file is a pointer. The
project facts live one layer down, in `docs/ai_context/`.

## Stack

Single package (`nabitllc/polla-mundialista`), npm, Node 20+, hosted on Vercel (`vercel.json`:
framework `vite`, every `/api/*` request rewritten to one function, a cron on `/api/notify`
every 10 minutes in June and July).

- `src/` — React 19 + Vite 7 frontend: `views/`, `components/`, one stylesheet (`styles.css`).
  No UI framework.
- `server/` — Express 5 API: `app.ts` (routers and the admin guard), `routes/`, `db.ts`
  (schema and seed), `fixtures.ts` (2026 schedule and example participants), `notifier.ts`.
- `shared/` — types, scoring, teams, bracket and time helpers used by both sides.
- `api/_entry.ts` — the Vercel entry; the build bundles it to `api/index.mjs` (gitignored).
- `scripts/`, `server/seedStickers.ts`, `server/reseedStickers.ts` — one-off seed scripts.
- Database: SQLite through `@libsql/client` — the local file `data/polla.db` with no env set,
  Turso when `TURSO_DATABASE_URL` is set.

TypeScript 5 (strict), passkey login through `@simplewebauthn`.

## Commands

```bash
npm install
npm run dev
npm run typecheck
npm run build
```

`npm run dev` starts the API on `127.0.0.1:4517` and Vite on `localhost:5173` (Vite proxies
`/api`). `npm run db:reset` wipes and reseeds the local database only. There is no lint or
test script and no CI workflow. A session ends with `typecheck` and `build` green
(`docs/ai_context/session_gates.md`).

## Read these, in this order

1. **`docs/ai_context/`** — the AI-facing project layer:
   - `architecture.md` — stack and directory map (descriptions still open)
   - `session_gates.md` — the gate commands; `file_size_limits.md` — per-file thresholds
   - `ui_standards.md` — measured UI tokens (Mode 0 not yet run)
   - `decisions.md` — **append-only decision log; write architectural decisions here**
2. **`README.md`** (Spanish) / **`README.en.md`** — features, scoring rules, deploy steps and
   the env var table.

## Environment

Names only, never values. Server: `ADMIN_PASSWORD`, `TURSO_DATABASE_URL`, `TURSO_AUTH_TOKEN`,
`FOOTBALL_DATA_TOKEN`, `TELEGRAM_BOT_TOKEN`, `TELEGRAM_CHAT_ID`, `TELEGRAM_WEBHOOK_SECRET`,
`CRON_SECRET`. Client: `VITE_APP_TITLE`, `VITE_ENABLE_GAME`. `SEED_PROD=1` points the sticker
seed scripts at Turso. Secrets live in `.env.local` (gitignored) and the Vercel env store.
Never commit one.

## Vault — personal context and universal standards

Identity, communication style, operating model, frameworks, agents and skills live in the
Mich-Brain2 vault: `C:\Development\Mich-Brain2\BOOTSTRAP.md` locally. **The vault is read-only.**
Propose vault changes as diffs; only its `_pending/` is AI-writable.

## Write surfaces

Writable by an AI session: `docs/ai_context/decisions.md` (append-only) and source under
`src/`, `server/`, `shared/`, `api/`, `scripts/`. Everything else under `docs/ai_context/` is
curated: propose a diff rather than editing in place.

## Golden rules and do-not-touch

- **Every change follows Michael's golden rules:** `Mich-Brain2/Playbooks/Maintainability_and_Consistency.md` (design system, UI consistency, code structure, config, agent hygiene, releases, budgets, dependencies, hosting cost). Run the audit block of each section the change touches; any FAIL blocks the merge.
- **Do not touch:** `package-lock.json` except through npm; `node_modules/`; `public/keepers/*` images (friends' photos, kept out of Git on purpose — see `.gitignore`); the production Turso data (`db:reset` is local-only; a `SEED_PROD=1` run needs Michael's go).
