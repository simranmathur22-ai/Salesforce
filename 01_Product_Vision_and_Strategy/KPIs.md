# Success Metrics (KPIs)

| KPI | Baseline (e-mail/PDF) | Target | How measured in Salesforce |
|---|---|---|---|
| Average claim processing time (submit → decision) | ~5 working days | **−30 %** | Report on `CreatedDate` vs. Closed/Approved date using field history |
| Share of claims auto-approved | 0 % | ≥ 40 % | Report: `Approval_Type__c = Auto` / all claims |
| Claims with complete data (VIN, amount, ≥ 1 photo) | ~60 % | ≥ 95 % | Required fields + validation; report on missing evidence |
| GDPR-compliant data capture | not auditable | **100 %** | PII fields only on controlled fields; `PII_Masked__c` = true for all claims closed > 90 days |
| Partner adoption | 0 % | ≥ 80 % of claims via portal after 3 months | Claims by creation source |
| Partner satisfaction (CSAT, 1–5) | n/a | ≥ 4.0 | Post-submission survey |

Review cadence: weekly during pilot, monthly afterwards, in the Sprint Review / stakeholder meeting.
