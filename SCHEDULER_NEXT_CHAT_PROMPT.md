# MOF Scheduler — Next Chat Starter Prompt
**Copy everything between the triple-dashes and paste it as your FIRST message in the new chat.**

---

I am **Dr. Subodh Agrawal, MD** — founder and architect of **Medical Office Force (MOF)** / **CareSync Pro**, an ONC-certified AI-native EHR competing with Epic and eClinicalWorks.

## What you need to know right now

We have just completed and deployed a standalone **AI Scheduling Engine** (`scheduler.html`) to:  
🌐 **https://caresync-pro.vercel.app/scheduler.html**  
📁 GitHub: `drsubodhagrawal/caresync-pro` → `scheduler.html`

The file is **1,205 lines · 80 KB** of vanilla HTML/CSS/JS — same single-file architecture as the rest of CareSync Pro (`index.html`). It has a real data layer (in-memory + localStorage), a working booking modal with live conflict detection, AI slot ranking, no-show risk scoring, a check-in board, waitlist, and daily insights.

## What was already built (do NOT rebuild)
- Provider-column day grid (5 providers: 3 MDs + 2 mid-levels, 4 specialties)
- Procedure-room grid (5 rooms with equipment)
- Booking modal: patient, specialty, visit type, provider, time, room, conflict detection, no-show risk preview, timezone cross-info for tele
- Appointment detail: status advance (Confirm → Check In → Room → Start → Complete), reschedule, cancel, Start Telehealth, Open Chart in CareSync
- Check-In Board with live patient flow KPIs
- AI Smart Schedule: ranks top 3 slots with plain-English reasons, never books on its own
- No-show risk engine (explainable rule-based scoring)
- Daily Insights (gaps, high-risk patients, waitlist matches)
- Waitlist with priority + urgency + auto smart-schedule link
- Multi-timezone display (ET/CT/MT/PT) — DST-correct
- 4 roles (Scheduler, MA, Provider, Admin) — admin-only Setup section
- 10 demo patients including 2 cross-timezone (PT, CT) for tele demos
- 13 seeded appointments for "today" covering all specialties + procedures
- Fully responsive: provider-column grid on desktop, agenda cards on mobile/phone/tablet
- CareSync EMR deep-link from every appointment

## Non-negotiable architecture rules (never violate)
1. **Light mode ONLY** — never dark mode
2. **AI augments, never automates** — every AI suggestion requires human confirmation before any action is taken
3. **Staging gate** — patient-requested appointments land as `status: pending_staff_review`; staff must confirm before appearing on the provider grid
4. **Structured coded data** — CPT, ICD-10, LOINC in all clinical fields; no free text for coded items
5. **No-show risk must stay explainable** — show the reasons, not just the score
6. **Immutable audit trail** — every booking stores `createdBy` (role + timestamp)
7. **DB API stays async-ready** — all data methods are swappable for a real FHIR R4 backend later

## Design system (match exactly — no exceptions)
```
--primary: #1a6eb5     (blue — buttons, active nav, in-person appts)
--purple: #5c35a8      (AI features, telehealth, Smart Schedule)
--teal: #00897b        (procedures, check-in board)
--success: #2e7d32     (completed, confirmed)
--danger: #c62828      (cancelled, no-show, errors)
--amber: #b45309       (warnings, pending)
Font body: DM Sans · Font numeric: JetBrains Mono
Body text: 15px · Labels: 13px · Page titles: 23px · KPI values: 30px
Buttons: min-height 42px · WCAG AAA 7:1+ contrast
```

## How to get the current file
```python
import urllib.request, json
url = "https://api.github.com/repos/drsubodhagrawal/caresync-pro/contents/scheduler.html"
TOKEN = "<regenerate at https://github.com/settings/tokens/4459313405 via Chrome MCP>"
req = urllib.request.Request(url, headers={"Authorization": f"token {TOKEN}", "Accept":"application/vnd.github+json"})
import base64
with urllib.request.urlopen(req) as r:
    data = json.loads(r.read())
    open("/home/claude/scheduler.html","wb").write(base64.b64decode(data["content"]))
print("Downloaded:", data["size"], "bytes")
```

## How to deploy changes
1. Edit with `str_replace` — never rewrite the whole file
2. Run 4 verification checks: `node --check`, brace balance, dead-click audit, `node harness.js`
3. Push via Python urllib PUT to GitHub (get fresh token first via Chrome MCP)
4. Verify: green ✅ on the GitHub commit = Vercel deployed in ~30s
5. **Live URL**: https://caresync-pro.vercel.app/scheduler.html

## What I want to build next (pick one or tell me your priority)

### Option A — Multi-Day Week View
Show 5 provider columns × 5 weekdays in a single scrollable grid. Add recurring appointment series. Add drag-to-reschedule.

### Option B — Patient Self-Service Booking Portal
Public-facing page where patients book online. AI triage (symptoms → suggested visit type). All patient requests land as `pending_staff_review` (staging gate). SMS/email confirmation. Calendar invite.

### Option C — Eligibility & Prior Auth Integration
Real-time eligibility badge on every appointment (pVerify/Waystar 270/271 mock). Auth required flag per visit type. Auth expiry tracker. Denial reason display.

### Option D — Multi-Practice / Enterprise View
Practice selector to switch between Athens Heart Center, Full Family Health, Georgia Pain & Spine, Premier Urgent Care. Centralized call center view across all practices. Cross-practice referral booking.

### Option E — Advanced AI + Demand Forecasting
No-show prediction v2 with practice-specific patterns. Auto gap-fill (AI texts/calls top waitlist match on cancellation). Panel capacity forecast. Visit type duration optimization alerts.

### Option F — FHIR R4 Backend Swap
Replace the `DB` in-memory layer with real `fetch()` calls to the HAPI FHIR R4 server in mof-ai-emr. `FHIR Appointment`, `FHIR Schedule`, `FHIR Slot`, `FHIR Patient` resources. Smart App Launch OAuth2.

---

**Please read `CARESYNC_SCHEDULER_ENGINE_KNOWLEDGE.md` from project knowledge first, then confirm what you understand about what's already built before we start.**

---
