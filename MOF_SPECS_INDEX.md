# MOF Specs & Reference Library
**Saved from ai-assisted-emr repo — May 23, 2026**
**Original repo deleted after extraction. These are the preserved assets.**

---

## 📋 Technical Specification Documents

| File | Size | Use When |
|------|------|----------|
| MOF_FHIR_Spec.md | 57 KB | Building FHIR R4 module in caresync-pro |
| MOF_Security_Spec.md | 73 KB | Building audit/compliance dashboard |
| MOF_Patient_Portal_Spec.md | 31 KB | Building patient.html portal module |
| MOF_PMS_Guide.md | 15 KB | Building practice management features |
| MOF_Architecture_CLAUDE.md | 9 KB | Reference for backend architecture decisions |
| MOF_Architecture_Overview.md | 7 KB | NestJS microservices architecture diagram |
| MOF_Env_Template.txt | 3 KB | All environment variables needed for production |
| MOF_Competitive_Evaluation.docx | 29 KB | EMR competitive analysis vs Epic/eCW/AdvancedMD |

---

## 🎨 UI Reference Mockups (JSX/HTML)

| File | Size | Use When |
|------|------|----------|
| MOF_Billing_v3_Reference.html | 119 KB | Building billing.html — most advanced billing UI |
| MOF_Intake_Eligibility_Reference.jsx | 85 KB | Building check-in / eligibility workflow |
| MOF_Unified_Demo_Reference.jsx | 87 KB | Reference for unified patient + provider UI |
| MOF_Master_Dashboard_Reference.jsx | 76 KB | Provider command center dashboard patterns |
| MOF_Physician_Dashboard_Reference.jsx | 63 KB | MD-specific dashboard layout reference |
| MOF_SuperAdmin_Reference.jsx | 68 KB | Super admin console patterns |
| MOF_Auth_System_Reference.jsx | 75 KB | Authentication / IAM UI reference |
| MOF_TCM_EMR_Template.jsx | 54 KB | TCM encounter template (CPT 99495/99496) |

---

## 🤖 AI System

| File | Size | Use When |
|------|------|----------|
| MOF_AI_Call_Agent_Prompt.pdf | 244 KB | Building AI phone agent / call center module |

---

## Build Order Reference
Use these files in this order as you build caresync-pro modules:

1. **encounter.html** → MOF_TCM_EMR_Template.jsx + MOF_Intake_Eligibility_Reference.jsx
2. **billing.html** → MOF_Billing_v3_Reference.html + MOF_FHIR_Spec.md
3. **patient.html** → MOF_Patient_Portal_Spec.md + MOF_Unified_Demo_Reference.jsx
4. **AI call agent** → MOF_AI_Call_Agent_Prompt.pdf + MOF_Env_Template.txt
5. **Security/Audit** → MOF_Security_Spec.md + MOF_Architecture_CLAUDE.md
