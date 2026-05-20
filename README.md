# CareSync Pro — Worldwide ONC-Certified EMR Platform

## Live Demo
**[https://caresync-pro.vercel.app](https://caresync-pro.vercel.app)**

## What's New (May 2026)

### Unified Auth System (index.html)
- Single-URL login with 8-tier RBAC role hierarchy
- Tiered MFA: FIDO2 (Tier 0), TOTP (Tier 1-4), Optional (Tier 5+)
- Chrome autofill-compatible (`autocomplete="username"` / `"current-password"`)
- 3-method password recovery: Email, SMS, Voice (landline-friendly)
- Role-filtered dashboard with module tiles linking to existing modules
- Dynamic Role & Permission Builder (14 modules x 8 actions)

### Super Admin Command Center (CareSync_SuperAdmin.html)
- Global Overview: 6 regions, 42 countries, 3,666+ practices, 51,750+ users
- Live audit feed with severity-coded events
- 8-tier role hierarchy explorer with expandable permission details
- Security policy table (NIST/HIPAA/ONC mapped)
- Regional compliance framework (HIPAA, GDPR, DPDP, LGPD, PDPA)

### Demo Credentials
All accounts use password: `Demo@2026!`

| Email | Role | MFA |
|-------|------|-----|
| subodh@caresync.pro | Global Super Admin (Tier 0) | FIDO2 |
| amelia@caresync.pro | Regional Super Admin (Tier 1) | TOTP |
| sarah@athensheart.com | Practice Admin (Tier 3) | TOTP |
| james@fullfamily.com | Physician (Tier 4) | TOTP |
| priya@apollocare.in | Nurse (Tier 5) | None |
| rosa@caresync.mx | Front Desk (Tier 6) | None |
| ahmed@caresync.ae | Billing (Tier 6) | TOTP |
| patient@gmail.com | Patient (Tier 9) | None |

## Modules
- **Clinical EHR** — Physician dashboard, HHA RPM workflows
- **Patient Portal** — 7-step intake wizard
- **Population Health** — Risk stratification dashboard
- **Global Command** — Super Admin command center
- **+ 10 more** — Scheduler, Billing/RCM, Telehealth, e-Prescribe, AI Call Agent, VOIP, MIPS, Audit, Interop, System Admin

## Architecture
- Static HTML + React via CDN (no build step)
- Hosted on Vercel (auto-deploy from GitHub)
- Designed for AWS GovCloud (HIPAA production)

## Standards
ONC HTI-1/HTI-2 | HIPAA Security Rule | NIST SP 800-63B | FHIR R4 | USCDI v3
