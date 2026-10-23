# User Stories (Gherkin Acceptance Criteria)

Definition of Ready: story has persona, value, AC, estimate, no open dependencies.
Definition of Done: config deployed to org, AC tested by PO, no PII exposed, documented in repo.

---
## US-01 Submit a warranty claim (E1, 5 SP)
**As a** Dealership Service Manager, **I want to** submit a warranty claim online, **so that** I don't have to e-mail PDFs.
```gherkin
Scenario: Successful submission
  Given I am logged in to the partner portal
  When I enter VIN, defect description and claim amount in EUR and click "Submit"
  Then a Warranty Claim is created linked to my dealership
  And I see a confirmation with the claim number

Scenario: Missing required data
  Given I am on the claim form
  When I click "Submit" without a VIN or amount
  Then I see an error message and no claim is created
```

## US-02 Upload evidence photos (E1, 5 SP)
**As a** Dealership Service Manager, **I want to** upload photos of defective parts to my claim, **so that** the internal team can verify the damage without an onsite visit.
```gherkin
Scenario: Attach valid photos
  Given the user is on the Claim creation screen
  When they click "Upload Evidence"
  Then they can attach up to 5 JPG/PNG files (max 5 MB each)

Scenario: Invalid file
  Given the user is on the Claim creation screen
  When they upload a file that is not JPG/PNG or larger than 5 MB
  Then the file is rejected with a clear message
```

## US-03 See my dealership's claims (E1, 3 SP)
**As a** Dealership Service Manager, **I want to** see status of all my claims, **so that** I don't need to call.
```gherkin
Scenario: Claim list
  Given I am logged in to the portal
  When I open "My Claims"
  Then I see all claims of my dealership with number, amount and status
```

## US-04 Auto-approve low-value claims (E2, 5 SP)
**As a** Warranty Agent, **I want** claims under €500 approved automatically, **so that** I focus on complex cases.
```gherkin
Scenario: Claim below threshold
  Given a claim is created with amount 499.99
  Then Status = "Approved" and Approval Type = "Auto"
  And the submitter receives a confirmation e-mail

Scenario: Claim exactly at threshold
  Given a claim is created with amount 500.00
  Then it is not auto-approved
```

## US-05 Route high-value claims to the agent queue (E2, 3 SP)
**As a** Warranty Agent, **I want** claims of €500 or more in a queue, **so that** nothing is missed.
```gherkin
Scenario: Claim at or above threshold
  Given a claim is created with amount 500.00 or more
  Then Status = "In Review", Approval Type = "Manual"
  And the owner is the queue "Warranty Agents"
```

## US-06 Optimised agent record page (E2, 3 SP)
**As a** Warranty Agent, **I want** all claim details, evidence and status on one page, **so that** I decide quickly.
```gherkin
Scenario: Agent opens a claim
  Given I am in the Warranty Console app
  When I open a claim
  Then I see key fields in the highlights panel, a status path, and the attached files
```

## US-07 Mask end-customer PII after 90 days (E3, 5 SP)
**As a** Data Protection Officer, **I want** PII removed automatically, **so that** we comply with GDPR storage limitation.
```gherkin
Scenario: Closed claim older than 90 days
  Given a claim has Status "Closed" and Closed Date older than 90 days
  When the nightly job runs
  Then End Customer Name = "ANONYMIZED", End Customer Email is empty and PII Masked is true

Scenario: Recent or open claim
  Given a claim is open or was closed less than 90 days ago
  When the nightly job runs
  Then its data is unchanged
```

## US-08 Dealer data isolation (E1, 3 SP)
**As a** Product Owner, **I want** dealerships to see only their own claims, **so that** partner data stays confidential.
```gherkin
Scenario: Cross-dealership access
  Given Dealer A and Dealer B each have a claim
  When a user of Dealer A opens "My Claims"
  Then Dealer B's claim is not visible
```

## US-09 KPI dashboard (E4, 5 SP) – Release 2
**As a** Product Owner, **I want** a dashboard of processing time and auto-approval rate, **so that** I can track KPIs.

## US-10 Configurable threshold (E2, 2 SP) – Release 2
**As a** Product Owner, **I want** the €500 threshold stored in Custom Metadata, **so that** I can change it without editing the Flow.
