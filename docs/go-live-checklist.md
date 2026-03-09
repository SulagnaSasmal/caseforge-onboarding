# CaseForge Go-Live Checklist

**Applies to:** CaseForge 3.x  
**Owner:** CaseForge CSM (with customer PM)  
**Last reviewed:** March 2025

This checklist is the execution plan for go-live day. Work through every item in order. Do not proceed to the next item until the current one is confirmed complete. The CSM and customer PM both sign off at the end.

---

## Pre-requisites for running this checklist

The following must be true before you start this checklist:

- [ ] Milestone 4 (Training and UAT) is signed off
- [ ] Go-live date and change window are confirmed with the customer and in the project tracker
- [ ] Customer IT admin is available for the full go-live day (or at least the three-hour window)
- [ ] CaseForge IE is available for the full change window
- [ ] Rollback plan is documented and both parties know how to execute it (see [Rollback procedure](#rollback-procedure) at the end of this document)
- [ ] Customer has notified end users of the go-live date and any service interruption window (if legacy system is being decommissioned)

---

## Section 1: Pre-cutover validation (T-2 hours)

Complete this section at least two hours before the cutover window begins.

### 1.1 Environment health

- [ ] CaseForge production tenant is accessible: `https://[tenant].caseforge.io`
- [ ] All integrations show healthy status in Admin Console → Integrations
- [ ] Email delivery test passes: Admin Console → Settings → Email → Test delivery
- [ ] At least two admin accounts can log in successfully (having two prevents a lockout if one admin account has an issue)

### 1.2 Configuration verification

- [ ] All production workflows are in Published state (not Draft)
- [ ] User roles and group assignments match the approved configuration from Phase 3
- [ ] Notification rules are enabled for all contracted notification events
- [ ] Data migration records are confirmed as complete (if applicable — customer PM signed off in Phase 3)
- [ ] Document storage integration verified: upload a test attachment and confirm it appears correctly

### 1.3 User access verification

Select five users (including at least one Supervisor and one Agent) and confirm each can:

- [ ] Log in via SSO (if SSO is enabled)
- [ ] See the case types they should have access to
- [ ] Not see case types outside their role (negative test)

| User | Role | Login confirmed | Access confirmed |
|---|---|---|---|
| | | | |
| | | | |
| | | | |
| | | | |
| | | | |

### 1.4 Communication readiness

- [ ] Go-live announcement drafted and ready to send to end users
- [ ] Support contact information for users documented (internal helpdesk or CaseForge support URL)
- [ ] Hypercare escalation path confirmed with CaseForge CSM

---

## Section 2: Legacy system freeze (T-0, start of cutover window)

If the customer is migrating from a legacy system, complete this section at the start of the cutover window.

- [ ] Legacy system set to read-only mode or taken offline per the agreed procedure
- [ ] Customer PM confirms no new cases can be created in the legacy system
- [ ] Any cases created in the legacy system after the last data migration extraction are documented for manual entry into CaseForge (or a final extraction is run now)

If the customer is running CaseForge in parallel with a legacy system (dual-running), skip this section.

---

## Section 3: Go-live activation

- [ ] CaseForge IE changes tenant status from staging to production
  - Confirm production flag is active: visible as "Production" badge in Admin Console → Settings
- [ ] IE confirms all scheduled jobs are running (sync jobs, report schedules)
- [ ] Customer admin logs in and confirms dashboard is loading correctly
- [ ] Customer admin creates the first production case:
  - Case type: _______________
  - Case ID: _______________
  - Created at: _______________
- [ ] Customer admin confirms the case appears in the correct queue/group

---

## Section 4: Integration smoke tests

Run a quick smoke test on each active integration immediately after go-live activation. These tests confirm the integration is pointing to the production tenant, not the staging tenant.

### SSO

- [ ] At least three different users log in via SSO successfully
- [ ] A user not in the allowed IdP group is denied access

### Directory sync (if applicable)

- [ ] Trigger a manual sync: Admin Console → Integrations → [Directory] → Sync now
- [ ] Confirm sync completes without errors
- [ ] New test user added to the IdP group appears in CaseForge within the sync interval

### Email notifications

- [ ] Assign a test case to a user
- [ ] Confirm the assignee receives the notification email within two minutes
- [ ] Confirm email comes from the correct sender address (not a staging domain)

### Document storage (if applicable)

- [ ] Upload an attachment to the test case created in Section 3
- [ ] Confirm the attachment is stored in the production storage location (not staging)
- [ ] Confirm the attachment is retrievable

### CRM integration (if applicable)

- [ ] Open the test case and confirm the linked CRM contact panel is loading
- [ ] Confirm data is from production CRM (not test/sandbox)

### Ticketing integration (if applicable)

- [ ] Create a ticket from the test case
- [ ] Confirm the ticket appears in the production Jira/ServiceNow instance (not test project)

---

## Section 5: Post-activation checks

### System stability (T+30 minutes)

Wait 30 minutes after go-live activation, then confirm:

- [ ] No error alerts in Admin Console → Settings → System health
- [ ] End-user login success rate is normal (no spike in failed logins)
- [ ] Integration sync jobs have completed at least one cycle without errors
- [ ] Customer admin confirms no user-reported issues in the first 30 minutes

### Go-live confirmation communication

- [ ] CaseForge CSM sends go-live confirmation to the customer executive sponsor
- [ ] Customer PM sends go-live announcement to end users (if not already sent)
- [ ] CaseForge IE sends hypercare start notification to the CSM

---

## Section 6: Go-live sign-off

Sign off only after all items in Sections 1–5 are confirmed complete.

**Outstanding items (if any):** List any incomplete items with agreed resolution dates.

| Item | Owner | Resolution date |
|---|---|---|
| | | |

**Go-live confirmed?** Yes / No (circle)

**Customer PM:** ________________________________ Date: __________

**CaseForge CSM:** ________________________________ Date: __________

---

## Rollback procedure

If a critical issue is discovered after go-live activation that cannot be resolved within the change window, execute the rollback:

1. **Customer PM and CaseForge CSM must agree** to rollback. Do not rollback unilaterally.
2. CaseForge IE reverts the tenant from production to staging mode.
3. Customer IT admin re-enables the legacy system (or lifts read-only mode) if it was taken offline.
4. Customer PM notifies end users of the delay.
5. CaseForge CSM updates the project tracker with the rollback reason and a revised go-live date.
6. The CaseForge IE performs a root cause analysis before rescheduling go-live.

**Rollback is a last resort.** Most issues encountered at go-live are resolvable within the change window — escalate to the CaseForge IE before deciding to rollback. Common issues that look critical but are resolvable in minutes: SSO attribute mapping errors, notification rule typos, workflow stage permission misconfiguration.

**Issues that warrant rollback:**
- Data loss or data corruption discovered in production
- Security misconfiguration that exposes data to unauthorized users
- Integration failure that blocks all case creation or processing (not just one edge case)

---

## After this checklist

Once go-live is signed off:

1. File the signed checklist in the project documentation (shared folder or project tracker).
2. Begin the hypercare period per the [Implementation Guide](implementation-guide.md#phase-6-hypercare-weeks-8-12).
3. Schedule the Week 1 hypercare check-in.
4. Transfer ongoing configuration support to the customer admin (CaseForge IE steps back from daily involvement).
