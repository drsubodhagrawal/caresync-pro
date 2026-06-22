# CLAUDE.md — CareSync Pro / Medical Office Force

Claude Code reads this file automatically every session. Keep it current.
**Before writing anything, read `CARESYNC_QUALITY_GATE.md` (the definition of done), then this file.**

## What this is
CareSync Pro = a friendly front-end re-skin of **Medical Office Force (MOF)**, an ONC-certified, multi-tenant EHR for US outpatient practices. Owner: **Subodh Agrawal, MD** — he makes all product / clinical / UI calls; Claude writes all the code. Live site: https://caresync-pro.vercel.app . This repo is `drsubodhagrawal/caresync-pro`; pushing to `main` auto-deploys to Vercel.

## Hard rules (never break)
- **Light mode only.** AAA 7:1 contrast. Font floors: body 15 / labels 13 / titles 24 / KPI 32 / buttons 42px. Built for a 75-year-old physician to read easily.
- **Every list** ships: Add / Archive (reason + timestamp) / Restore / Search (focus-preserving) / status dropdown All-Active-Archived.
- **Structured coded data only** (ICD-10 / CPT / RxNorm / LOINC / SNOMED). Decode every code inline + tooltip.
- **Tenant-aware** on every record (Group→Org→Clinic→POD) + 8-tier RBAC `can()` gate.
- **AI augments, never automates:** the physician Confirms/Dismisses every AI suggestion; log every AI call to `AiInteraction`.
- **MEAT before any HCC bills** (Monitor/Evaluate/Assess/Treat). CMS-HCC V28 (operative Jan 1 2026; HCCs don't roll over).
- **Hash-chained, append-only audit** (`logAudit`). **Staging gate** (portal data lands in Intake, never straight to the EMR). Card data never touches our servers (hosted iframe, PCI SAQ A).
- **No shortcuts / no patchwork.** Pre-delivery validation on every single-file build: `node --check`, brace/paren/bracket balance 0/0/0, dead-click audit (every `on*=` handler — including ones built inside JS template strings — maps to a defined function), and an internal-call audit.

## Design system (locked)
Tokens: navy `#0b1e3d`, blue `#1a6ef5`, teal `#0d9488`, green `#15803d`, amber `#b45309`, red `#dc2626`, purple `#5b21b6`/`#7c3aed` (AI accent). Fonts: IBM Plex Sans (body) + IBM Plex Mono (codes). One shared **Foundation spine** is reused verbatim across modules: top nav, persistent VoIP bar, tenant switcher, role chip, `can()`, `logAudit()`+`cyrb53`, modal, toast.

## Key files
- `appointment-dashboard.html` — upcoming appts + check-in / eligibility / copay / begin-encounter
- `caresync-foundation.html` — the testable shared shell (RBAC + audit + list standard demo)
- `encounter.html` — flagship AI Encounter Engine, 6 steps (brief → reconcile → HCC+MEAT → SOAP → orders/E&M → sign)
- `schedule.html` · `scheduler.html` · `practice-builder.html` — scheduling suite
- `CARESYNC_QUALITY_GATE.md` — the 9-point definition of done (read first)
- `PHASE2_HANDOFF.md` — Phase 2 plan + ready-to-paste prompts
- `MOF_SPECS_INDEX.md`, `CARESYNC_SCHEDULER_ENGINE_KNOWLEDGE.md` — module references

## Demo data (always pre-fill; never empty forms)
Athens Heart Center (cardiology). Providers: Dr. Raj Patel, Dr. Kim Holloway, NP Lisa Chen. Encounter demo patient: **Harold Branson, 74M, MRN AHC-04417** — HFpEF (I50.32), AFib (I48.91), DM+CKD (E11.22), CKD3b (N18.32), all HCC + MEAT; meds with RxNorm; sulfonamide allergy vs furosemide teaching point.

## How to work here (the big win of Claude Code)
You have **direct file access** — write and edit files in this folder directly, run `node --check` locally, commit with git. **No base64 chunk transfer** (that bridge was the source of nearly every past error). Workflow: build/edit the file → validate (above) → commit (hyphenated message) → push to `main` (auto-deploys) → fetch the live Vercel URL and confirm its SHA matches your built file → print the filled-in 9/9 quality-gate table.

## Phase status
- **Phase 1 DONE & live:** foundation shell + encounter engine (+ appointment dashboard).
- **Phase 2 = Encounter Intelligence expansion** — see `PHASE2_HANDOFF.md` for the module plan and first build.

## Production track (separate app)
`mof-ai-emr` = the real multi-file Next.js product (Next 14 / TS / Tailwind / Prisma / Postgres-Neon / HAPI FHIR R4). Local path: `C:\Users\agraw\mof-md-emr-unified` (port 3001). Keep every `.tsx`/`.ts` ≤250 lines. App AI model pinned: `claude-sonnet-4-20250514`. Same rules + same quality gate apply.
