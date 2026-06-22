# CareSync Pro — Phase 2 Handoff (Encounter Intelligence Expansion)

## Where Phase 1 ended (all live + SHA-verified)
- `caresync-foundation.html` — shared spine (RBAC, hash-chained audit, list standard) — build SHA `8520564d…`
- `encounter.html` — AI Encounter Engine, 6 steps, demo patient Harold Branson — build SHA `93a40105…`
- `appointment-dashboard.html` — links forward to the encounter; the encounter links back.
- `CARESYNC_QUALITY_GATE.md` — the 9-point definition of done.

## Phase 2 goal
Extend the clinical suite *around* the encounter, reusing the same Foundation spine and passing the 9/9 gate. Three modules, build in this order:

### 2A — Quality & HCC Tracker  (build first)
The revenue + compliance cockpit that extends the HCC/MEAT work already in `encounter.html`.
- Patient-panel table (full list standard: Add / Archive+reason / Restore / Search / All-Active-Archived).
- Per patient: HCCs captured vs available this year, total RAF captured vs available (label RAF as demo weights), and a "recapture due" flag for HCCs not yet documented in 2026.
- Care-gap column (HEDIS-style: A1c overdue, AWV G0438/G0439 due, etc.), coded + decoded inline.
- Each row: "Open encounter →" deep-link to `encounter.html`; wire the nav back both ways.

### 2B — Patient Chart (longitudinal)
The record the encounter writes into: problems / meds (RxNorm) / allergies / results timeline / past encounters, all coded + decoded inline, archive/restore on problems & meds. Links to the encounter and the Quality Tracker.

### 2C — Physician Daily Dashboard (the cockpit)
Today's panel: encounters to close, unsigned notes, RAF captured today, care gaps, messages — ties appointment-dashboard + encounter + quality-tracker together.

## Definition of done (every module)
Pass `CARESYNC_QUALITY_GATE.md` — the 9-row table, with **Build SHA == Live SHA** verified.

---

## Ready-to-paste prompts (use in Claude Code, opened in this repo folder)

### Prompt 0 — session orientation (paste first, every new session)
```
Read CLAUDE.md and CARESYNC_QUALITY_GATE.md in this repo. Then read encounter.html
and caresync-foundation.html to learn the shared Foundation spine (tokens, top nav,
VoIP bar, tenant switcher, can() RBAC, logAudit()+cyrb53, modal, toast). Don't write
code yet — confirm in 5 bullets what the spine gives you and which design tokens and
font floors you'll reuse, then wait for my go.
```

### Prompt 1 — build Phase 2A (Quality & HCC Tracker)
```
Build quality-tracker.html as a new CareSync module on the EXACT same Foundation spine
as encounter.html (identical tokens, top nav, VoIP bar, tenant switcher, role chip,
can(), logAudit, modal, toast). It is the Quality & HCC Tracker:
- A patient-panel table with the permanent list standard: Add / Archive (reason +
  timestamp) / Restore / Search (focus-preserving) / All-Active-Archived dropdown.
- Per patient: HCCs captured vs available this year, total RAF captured vs available
  (label RAF as demo weights), and a "recapture due" flag for HCCs not yet documented
  in 2026.
- A care-gaps column (e.g. A1c overdue, AWV G0438/G0439 due) with coded items decoded
  inline + tooltip.
- Each row has "Open encounter →" linking to encounter.html; wire the encounter and
  appointment-dashboard navs back to here (both ways).
- Use Athens Heart Center demo data including Harold Branson AHC-04417 and his 4 HCCs.
- Light mode, AAA 7:1, font floors body15/title24/KPI32/btn42. Tenant-aware + 8-tier
  RBAC. Every AI suggestion needs physician Confirm; log AI calls to AiInteraction.

Before telling me it's done: run node --check on the script, confirm brace/paren/bracket
balance 0/0/0, do a dead-click audit (every on*= handler, including ones built inside JS
template strings, maps to a defined function) and an internal-call audit. Then commit with
a hyphenated message and push to main. Then fetch the live Vercel URL and confirm its SHA
matches your built file. Finish with the 9/9 quality-gate table from CARESYNC_QUALITY_GATE.md
filled in.
```

### Prompt 2 — continue to the next module
```
Phase 2A is done and live-verified. Build Phase 2B (Patient Chart, patient-chart.html)
on the same spine and rules, same Athens Heart Center demo data, wired both ways to
encounter.html and quality-tracker.html. Same validation + commit + live-SHA verify +
9/9 table at the end.
```

---

## Production track (optional — when you want the real app)
Open Claude Code in `C:\Users\agraw\mof-md-emr-unified` and paste:
```
Read CLAUDE.md and CARESYNC_QUALITY_GATE.md. This is mof-ai-emr, the production Next.js 14
+ TypeScript + Tailwind + Prisma + Postgres (Neon) + HAPI FHIR R4 app. Confirm the stack and
current state, then propose the Phase 1 (Foundation) plan: Next.js scaffold, Prisma schema
with tenant context + 8-tier RBAC + AiInteraction + append-only audit, and a design-token
theme matching CareSync. Keep every .tsx/.ts file under 250 lines. Don't write code until I
approve the plan.
```
