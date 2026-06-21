# CareSync Pro — 9/9 Quality Gate

**Definition of Done for every CareSync / Medical Office Force module.**
Owner: Subodh Agrawal, MD · Locked June 2026

This document is the standard each build must pass *before* it is presented or
deployed. It is based on the nine product-quality characteristics of
**ISO/IEC 25010**. Every delivery ends with the 9-row PASS table at the bottom
of this file, filled in for that module.

---

## The 9 characteristics → binary PASS check

| # | ISO 25010 characteristic | What "PASS" means in CareSync |
|---|--------------------------|-------------------------------|
| 1 | **Functional Suitability** | Zero dead clicks. Every list ships the full **Add / Archive (reason + timestamp) / Restore / Search / All-Active-Archived** standard. |
| 2 | **Performance Efficiency** | Single-file where practical. Lists re-render with a **focus-preserving inner re-render** (typing in search never loses focus). |
| 3 | **Compatibility** | Identical design tokens across every module. Cross-links wire **both ways**. Every record carries tenant context (Group→Org→Clinic→POD). Coded data only (ICD-10 / CPT / RxNorm / LOINC / SNOMED). |
| 4 | **Usability** | **Light mode only.** AAA contrast 7:1. Font floors: body 15px, title 24px, KPI 32px, buttons 42px tall. Codes decoded inline + tooltip. Works down to a 380px phone. |
| 5 | **Reliability** | `node --check` clean **and** brace / paren / bracket balance 0 / 0 / 0. |
| 6 | **Security** | 8-tier `can()` RBAC gate. Hash-chained, append-only `logAudit()`. Staging gate (portal data never writes straight to the EMR). Card data never touches MOF servers. |
| 7 | **Maintainability** | One shared **Foundation spine** (tokens, nav, VoIP bar, modal, toast, `can()`, `logAudit`) reused verbatim. No patchwork. |
| 8 | **Portability** | **Live Vercel SHA == build SHA**, verified Windows-side (the container can't reach vercel.app). |
| 9 | **Safety (clinical)** | AI **augments, never automates** — physician Confirms/Dismisses every suggestion. **MEAT required before any HCC bills.** Every AI call logged to `AiInteraction`. CMS-HCC V28. |

---

## Enforcement ritual (run in this order, every time)

1. **Three syntax checks**
   - `node --check` on the extracted `<script>` block.
   - Brace / paren / bracket balance must be **0 / 0 / 0**.
   - **Dead-click audit** — every `on*=` handler (static HTML **and** the ones built inside JS template strings) must map to a defined function.
2. **Internal-call audit** — every `fn(` resolves to a defined function or a known builtin. Catches undefined calls the handler audit misses.
3. **Contrast / font eyeball** — light mode, AAA, font floors met.
4. **Push** — `git add` the specific files, hyphenated commit message, push to `main` (auto-deploys to Vercel).
5. **Windows-side live-SHA verify** — `Invoke-WebRequest -OutFile` + `Get-FileHash` on the live URL, compare to the build SHA. Only then is the module "done."

---

## The permanent list standard (non-negotiable)

Any list or table — solo practice to enterprise — ships all five:

1. **Add**
2. **Archive** — soft-delete with a required **reason + timestamp** (never hard-delete)
3. **Restore** — bring an archived row back
4. **Search** — a box that filters the list, **focus-preserving** on each keystroke
5. **Status dropdown** — **All / Active / Archived**

---

## Design system (locked)

- **Light mode only.** Never dark mode.
- Tokens: navy `#0b1e3d`, blue `#1a6ef5`, teal `#0d9488`, green `#15803d`, amber `#b45309`, red `#dc2626`, purple `#5b21b6` / `#7c3aed` (AI / violet accent — kept visually distinct from clinical blue).
- Fonts: **IBM Plex Sans** (body) + **IBM Plex Mono** (codes / numbers) via Google Fonts.
- Readability floors: body 15px, labels 13px, titles 24px, KPI values 32px, buttons ≥42px tall. AAA 7:1 (dark text on light, never light gray on white). Built for a 75-year-old physician to read.

---

## Architecture non-negotiables

1. **Tenant-aware from row one** — every record carries Group→Org→Clinic→POD and passes the 8-tier RBAC gate, so the admin roll-up aggregates later with no rebuild.
2. **Structured coded data only** — ICD-10, CPT, RxNorm, LOINC, SNOMED. No free-text clinical fields.
3. **AI augments, never automates** — the physician confirms every AI suggestion. No autonomous clinical decisions.
4. **Every AI call logged** to `AiInteraction`.
5. **MEAT before HCC** — Monitor / Evaluate / Assess / Treat documented before a diagnosis is billable (OIG audit defense).
6. **Immutable audit** — append-only, hash-chained, tamper-evident (45 CFR 164.312(b)).
7. **Staging gate** — patient-portal data lands in an Intake table (`pending_promotion`); staff promote with an audit trail. It never writes straight to the EMR.
8. **Card data never on MOF servers** — hosted iframe fields only (PCI SAQ A).

---

## Deploy transport (the part that actually causes "errors")

The code passes validation; the **container → Windows file bridge** is the fragile
step. Two failure modes seen: a single dropped character mid-chunk, and
`write_file` silently truncating a long single-line paste.

**Robust pipeline:**

1. In the container: `gzip -9 | base64 -w0`, then split into **~3500-char** pieces.
2. Write **each** piece to its **own** file on the Windows repo.
3. Verify a **per-chunk sha256** Windows-side **before** assembling — pinpoints exactly which chunk is bad so only that one is re-sent.
4. Assemble, gate on the **full-blob sha**, then `gzip.decompress` and gate on the **decoded-HTML sha == build sha** (abort + never push on mismatch).
5. `git add` the specific files, hyphenated commit, push to `main`.
6. Verify the **live Vercel URL sha** from Windows. Git's "LF will be replaced by CRLF" warning is benign — the committed blob stays LF and the live sha matches.

---

## Module sign-off table (copy into every delivery)

| # | Characteristic | Result |
|---|----------------|--------|
| 1 | Functional Suitability | ☐ |
| 2 | Performance Efficiency | ☐ |
| 3 | Compatibility | ☐ |
| 4 | Usability | ☐ |
| 5 | Reliability | ☐ |
| 6 | Security | ☐ |
| 7 | Maintainability | ☐ |
| 8 | Portability (live SHA == build SHA) | ☐ |
| 9 | Safety (clinical) | ☐ |

Build SHA: `__________`  ·  Live SHA: `__________`  ·  Module: `__________`
