# GDPR / DSGVO Strategy

## 1. Personal data in scope
| Data | Field | Data subject | Purpose |
|---|---|---|---|
| End-customer name | `Warranty_Claim__c.End_Customer_Name__c` | Vehicle owner | Identify the warranty case |
| End-customer e-mail | `Warranty_Claim__c.End_Customer_Email__c` | Vehicle owner | Contact for clarification |
| Partner contact data | `Contact` | Dealership employee | Portal login and notifications |

Legal basis: Art. 6(1)(b) (contract performance) / 6(1)(f) (legitimate interest in processing warranty). Principle: **data minimisation (Art. 5(1)(c))** – only the two PII fields above are captured on the claim; VIN and defect text are not personal data in isolation.

## 2. Controls
| GDPR principle | Control in the solution |
|---|---|
| Storage limitation (Art. 5(1)(e)) | Scheduled Flow `Warranty Claim - GDPR PII Masking (90 days)` runs daily: for claims `Closed` more than 90 days ago, name → `ANONYMIZED`, e-mail cleared, `PII_Masked__c` = true |
| Integrity & confidentiality (Art. 32) | Private org-wide default; partners see only their dealership's claims; PII fields visible only via permission sets |
| Access control | Permission sets `Partner Claim Submitter` (create/read, status read-only) and `Warranty Agent` |
| Accountability (Art. 5(2)) | Field history on status and amount; `PII_Masked__c` flag provides audit evidence; this document |
| Right to erasure (Art. 17) | Agents can run the same masking on request (set Closed Date, Status) – manual process documented in runbook (Release 2: screen flow) |
| Data residency | Choose an EU Hyperforce region (Frankfurt/Paris) when provisioning the production org |

## 3. Note on Salesforce Data Retention Policies
Salesforce offers native Data Retention Policies (Archive/Delete) as a Shield-related feature requiring additional licensing. This MVP achieves the required outcome – automated removal of PII after 90 days – with a scheduled Flow so it runs in any Enterprise-class org. For production at scale, evaluate the native feature or Big Object archiving.

## 4. Open items for Data Protection Officer
- Confirm the 90-day period against warranty and tax retention obligations (claim records without PII may be retained longer)
- Sign Data Processing Agreement with Salesforce (Salesforce DPA)
- Add privacy notice text to the portal submission form
