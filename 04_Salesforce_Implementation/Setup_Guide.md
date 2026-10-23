# Salesforce Implementation Guide

## 1. Deploy the metadata
```bash
sf org login web --alias warranty-dev --set-default
sf project deploy start --manifest manifest/package.xml --target-org warranty-dev --dry-run   # validate
sf project deploy start --manifest manifest/package.xml --target-org warranty-dev
```
Deploys: `Warranty_Claim__c` (+ 11 fields), queue *Warranty Agents*, flows (auto-approval, PII masking), permission sets.

## 2. Manual configuration (not in repo)
1. **Users:** assign `Warranty Agent` permission set to internal users; add them to the *Warranty Agents* queue.
2. **Lightning App Builder:** create record page *Warranty Claim Record Page* (highlights panel, Path on `Status__c`, details, Files related list) and app *Warranty Console* with *My Open Claims* / *Claims In Review* list views.
3. **Experience Cloud:** Setup → Digital Experiences → enable; create site from *Customer Service* (or LWR) template; add a Screen Flow for claim submission (fields + File Upload component restricted to `.jpg,.png`; max 5 files via component settings/validation) and a *My Claims* list.
4. **Partner users:** on Account (Dealership) → Enable as Partner; on Contact → Enable Partner User with profile *Partner Community* + permission set `Partner Claim Submitter`.
5. **Sharing:** Setup → Digital Experiences → Settings → Sharing Sets: give partner users access to Warranty Claims where `Dealership__c` = user's Account.
6. **Sample data:** 3 dealership accounts, 2 contacts each, ~20 claims of mixed amounts (some < €500, some ≥ €500, some Closed > 90 days ago).

## 3. Evidence for the portfolio (add to `Screenshots/`)
- `ERD.png` – Setup → Schema Builder: Account, Contact, Warranty_Claim__c
- `Flow_AutoApproval.png`, `Flow_PII_Masking.png` – Flow Builder canvas
- `Agent_Record_Page.png`, `Portal_Submit_Claim.png`, `Portal_My_Claims.png`

## 4. Test checklist
- [ ] Claim €300 → Approved/Auto, e-mail received
- [ ] Claim €500 → In Review/Manual, owner = Warranty Agents queue
- [ ] Dealer A cannot see Dealer B's claims
- [ ] Closed claim with Closed Date 91 days ago → PII masked after running the flow (Setup → Flows → Run, or wait for schedule)
