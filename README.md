# B2B Warranty Management System

A Salesforce portal for dealership partners to submit warranty claims online, with automated approval of low-value claims and GDPR-compliant data handling. Built as a **Product Owner portfolio project** for the German manufacturing/automotive market.

| | |
|---|---|
| **Kanban board** | _add GitHub Projects link_ |
| **Demo (2 min)** | _add Loom link_ |

## The "Why" – business value
Warranty claims arrive by e-mail/PDF and are re-keyed by hand: slow, error-prone, and a GDPR risk.

| Outcome | Target |
|---|---|
| Claim processing time | **−30 %** |
| Claims auto-approved (< €500) | ≥ 40 % |
| GDPR-compliant data capture | **100 %** (PII auto-masked 90 days after closure) |

Agents spend time on claims that need judgement; partners get instant answers on simple ones.

## The "How"
| Phase | Artifacts |
|---|---|
| 1. Vision & strategy | [PRD](01_Product_Vision_and_Strategy/PRD.md) · [Personas](01_Product_Vision_and_Strategy/Personas.md) · [KPIs](01_Product_Vision_and_Strategy/KPIs.md) |
| 2. Backlog & Agile | [Epics](02_Product_Backlog_and_Agile_Artifacts/Epics/Epics.md) · [User stories with Gherkin AC](02_Product_Backlog_and_Agile_Artifacts/User_Stories/User_Stories.md) |
| 3. Data privacy | [GDPR strategy](03_Data_Privacy_and_Compliance/GDPR_Strategy.md) |
| 4. Implementation | [Setup guide](04_Salesforce_Implementation/Setup_Guide.md) · [Metadata](force-app/main/default) |

### Solution overview
- **Data model:** Account (Dealership) → Contact; custom object `Warranty_Claim__c` (looks up Account and Contact)
- **Automation:** record-triggered Flow – amount < €500 → auto-approve + e-mail; ≥ €500 → *Warranty Agents* queue
- **GDPR:** scheduled Flow masks end-customer name/e-mail 90 days after closure
- **Security:** private sharing, permission sets for partners and agents
- **UX:** Experience Cloud partner portal; Lightning Warranty Console for agents

### Screenshots
_Add ERD, Flow canvas, record page and portal screenshots from `04_Salesforce_Implementation/Screenshots/`._

## Deploy
```bash
sf project deploy start --manifest manifest/package.xml --target-org <alias>
```
