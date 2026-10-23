# Product Requirements Document: B2B Warranty Management System

| | |
|---|---|
| **Product Owner** | Simran Mathur |
| **Status** | v1.0 – MVP scope agreed |
| **Platform** | Salesforce (Sales/Service objects, Flow, Experience Cloud) |

## 1. Problem Statement
Dealerships and B2B partners submit warranty claims by e-mail or PDF. The internal warranty team re-keys each claim by hand, which causes:
- **Long cycle times** – every claim, even trivial ones, waits in a shared inbox.
- **Lost or incomplete data** – missing VINs, unreadable photos, no single source of truth.
- **GDPR risk** – end-customer personal data (name, e-mail) sits indefinitely in mailboxes and attachments, with no retention rule.

## 2. Vision
> One self-service portal where partners submit complete claims in minutes, low-risk claims are approved instantly, and personal data is removed automatically when no longer needed.

## 3. Target Audience
| Persona | Type | Need |
|---|---|---|
| Klaus – Dealership Service Manager | External (Experience Cloud) | Fast, simple submission and status visibility |
| Julia – Internal Warranty Agent | Internal (Lightning) | Clean queue, full data, one-click decisions |

See [Personas.md](Personas.md).

## 4. Scope
### In scope (MVP)
- Partner portal: submit a claim, attach evidence photos, see own dealership's claims and status
- Data model: Account (Dealership), Contact, `Warranty_Claim__c`
- Automation: claims **< €500 auto-approved** with e-mail confirmation; **≥ €500 routed** to the *Warranty Agents* queue
- Internal Lightning app with optimised record page and list views
- GDPR: automated masking of end-customer PII 90 days after claim closure

### Out of scope (later releases)
- Payment / credit-note integration with ERP
- Multi-language portal (DE/EN toggle planned for Release 2)
- Supplier recovery process
- Native Salesforce Data Retention Policies / Shield (licence required)

## 5. Business Rules
1. Claim amount is entered in EUR.
2. Amount < 500 → Status `Approved`, Approval Type `Auto`.
3. Amount ≥ 500 → Status `In Review`, Approval Type `Manual`, owner = queue *Warranty Agents*.
4. A dealership can only see its own claims (Private OWD + partner sharing).
5. Closed claims: end-customer name set to `ANONYMIZED` and e-mail cleared after 90 days.

## 6. Success Metrics
See [KPIs.md](KPIs.md).

## 7. Assumptions & Risks
| # | Item | Mitigation |
|---|---|---|
| 1 | Partners will adopt the portal over e-mail | Onboarding session; stop accepting e-mail claims after pilot |
| 2 | €500 threshold is right | Make threshold a Custom Metadata value in Release 2; review after 4 weeks of data |
| 3 | Fraud on auto-approved claims | Weekly sampling audit by agents; report on auto-approved claims |
| 4 | Data Retention Policies need extra licensing | Scheduled Flow implements the same outcome |

## 8. Backlog
See [Epics and User Stories](../02_Product_Backlog_and_Agile_Artifacts/).
