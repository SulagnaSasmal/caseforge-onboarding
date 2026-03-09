# CaseForge Implementation Guide

**Version:** CaseForge 3.x  
**Last reviewed:** March 2025  
**Owner:** Customer Success — Implementation Team

---

## Overview

This guide covers the end-to-end CaseForge implementation for new enterprise customers. It defines milestones, ownership, dependencies, and success criteria for each phase of the engagement. The typical implementation runs seven weeks from kickoff to go-live, followed by a four-week hypercare period.

Implementation timelines vary based on the scope of integrations, the volume of data migration, and the complexity of the customer's SSO environment. The timeline in this document assumes a standard implementation — a Solutions Engineer will flag deviations at kickoff if your configuration requires adjustments.

---

## Roles and responsibilities

| Role | Responsibilities |
|---|---|
| **CaseForge CSM** | Overall engagement ownership, stakeholder communication, escalation point |
| **CaseForge Implementation Engineer** | Technical configuration, integration setup, environment validation |
| **Customer Project Manager** | Internal coordination, scheduling, change management, sign-off on milestones |
| **Customer IT Administrator** | SSO configuration, network access, integration endpoints, directory service connection |
| **Customer Workspace Administrator** | User provisioning, workflow configuration, report setup |

The customer PM is the single point of contact for the CaseForge team throughout the implementation. Designate this person at or before kickoff and confirm their contact details in the project tracking system.

---

## Pre-kickoff requirements

Complete the following before the kickoff call. Gaps in these items delay the implementation by an equivalent number of business days.

- [ ] Purchase order confirmed and order processing complete
- [ ] Customer PM assigned and contact information provided to CaseForge CSM
- [ ] IT administrator identified and available for the SSO and integration sessions
- [ ] Network security review initiated (if applicable — required for customers with strict egress filtering)
- [ ] CaseForge tenant provisioned (CaseForge IE confirms via email with tenant URL and initial admin credentials)

---

## Phase 1: Kickoff and environment setup (Weeks 1–2)

### Kickoff call

**Duration:** 90 minutes  
**Participants:** CaseForge CSM, IE, customer PM, customer IT lead, executive sponsor (optional)

The kickoff call establishes shared understanding of the implementation scope, timeline, and working model. It covers:

1. **Scope confirmation** — Review the contracted modules, number of users, and integration list. Surface any scope differences between what was sold and what the customer now expects.
2. **Timeline walkthrough** — Walk through this guide's milestone table and agree on target dates for each phase.
3. **Technical requirements** — The IE collects the customer's SSO provider, directory service type (Active Directory, Okta, Azure AD), and network architecture summary.
4. **Communication cadence** — Agree on the weekly check-in format (typically 30 minutes on a fixed day), escalation path, and ticket tracking system.
5. **Success criteria** — Define what "done" means for go-live. Default criteria are in the [Go-Live Checklist](go-live-checklist.md). Customers may add acceptance criteria — document them here.

**Kickoff deliverable:** A completed project plan with agreed milestone dates, shared with the customer PM within 24 hours of the call.

### Environment setup

The IE provisions the CaseForge tenant and completes base configuration:

| Task | Owner | Timeline |
|---|---|---|
| Tenant DNS and SSL setup | CaseForge IE | Day 1–2 post-kickoff |
| Initial admin account creation | CaseForge IE | Day 1 |
| Customer admin account activation | Customer IT admin | Within 24h of receiving invite |
| IP allowlist configuration (if required) | Customer IT admin | Day 2–3 |
| SMTP relay configuration for notifications | CaseForge IE + customer IT | Day 3–5 |

**Milestone 1 — Environment ready:** The customer admin can log into the CaseForge tenant, access the admin console, and confirm email notifications are delivering. Confirm with the customer PM before proceeding to Phase 2.

---

## Phase 2: SSO and integrations (Weeks 3–4)

### Single sign-on configuration

CaseForge supports SAML 2.0 and OIDC for SSO. The IE leads the configuration session with the customer IT admin. Typical session length is two hours.

**SAML 2.0 (Okta, Azure AD, ADFS)**

1. IE provides the CaseForge Service Provider metadata XML.
2. Customer IT admin creates the SAML application in the identity provider and shares the IdP metadata XML or metadata URL.
3. IE imports IdP metadata and configures attribute mappings:

   | CaseForge attribute | SAML assertion attribute |
   |---|---|
   | `user.email` | `email` |
   | `user.firstName` | `givenName` |
   | `user.lastName` | `surname` |
   | `user.groups` | `groups` (multi-value) |

4. Both parties test SSO login with a pilot user account.
5. Customer IT admin enables SSO for the full user population after testing.

**OIDC (Okta, Azure AD)**

The process is equivalent — the IE provides the redirect URI and client credentials; the customer configures the application in the IdP and provides the discovery endpoint URL.

> **Note:** If the customer uses a non-standard attribute name for email, confirm the mapping before the test. Authentication will fail silently if the email attribute is not mapped — it does not produce an error message in the IdP.

### Core integrations

Integrations are configured during weeks 3–4. The scope of integrations for your customer is confirmed in the order documentation.

| Integration | Configuration owner | Typical setup time |
|---|---|---|
| Active Directory / LDAP (user sync) | Customer IT admin | 2–4 hours |
| Email (SMTP or Microsoft 365 / Google Workspace) | Customer IT admin + IE | 1–2 hours |
| Document storage (SharePoint, S3, or Google Drive) | Customer IT admin + IE | 2–3 hours |
| CRM (Salesforce, HubSpot — optional) | Customer IT admin + IE | 3–5 hours |
| Ticketing (Jira, ServiceNow — optional) | Customer IT admin + IE | 2–4 hours |

For each integration, the IE verifies end-to-end connectivity before marking it complete. Integrations that cannot be completed due to customer-side dependencies (firewall rules, API key approval) are logged as blocking items in the project tracker.

**Milestone 2 — Integrations complete:** All contracted integrations are connected, tested end-to-end, and confirmed by the customer IT admin. Capture any deferred integrations with an agreed completion date before proceeding.

---

## Phase 3: Configuration and data migration (Weeks 3–5)

### Workflow configuration

CaseForge workflows are configured by the customer's workspace administrator, with IE guidance. Before the configuration session:

- [ ] Customer provides existing process maps or workflow documentation for the cases they'll manage in CaseForge
- [ ] Customer identifies three to five priority workflows for initial configuration
- [ ] Customer provides user role definitions (who can create cases, who can approve, who can view only)

The configuration session covers:

1. Case type setup — create case categories matching the customer's process taxonomy
2. Workflow stages — configure stage labels, transition rules, and required fields per stage
3. Role-based access control — assign permissions to CaseForge role groups
4. Custom fields — add customer-specific fields to case forms
5. Notification rules — define which events trigger email notifications and to whom

Allow two hours per workflow for initial configuration, plus testing. Customers with more than five initial workflows should plan a second configuration session.

### Data migration

If the customer is migrating cases or records from a legacy system, the IE coordinates data migration during weeks 3–5. This requires:

1. **Data export from the legacy system** — customer IT exports case data in CSV or JSON format. The IE provides the field mapping template.
2. **Data mapping and transformation** — IE maps legacy fields to CaseForge fields. Customer PM reviews the mapping and flags business rule exceptions.
3. **Test migration** — IE loads a sample dataset (5–10% of total records) into a staging environment. Customer PM reviews for accuracy.
4. **Full migration** — After customer sign-off on the test migration, IE runs the full import during a scheduled maintenance window.
5. **Verification** — Customer PM spot-checks migrated records against the legacy system. IE confirms record counts match.

> **Data migration is a customer dependency.** Delays in providing the data export push the migration timeline — and potentially the go-live date — by an equivalent number of days. Surface this risk at kickoff and monitor it weekly.

**Milestone 3 — Configuration and migration complete:** Core workflows are configured, all user roles are set up in CaseForge, and migrated data is verified by the customer PM. Confirmed in writing before proceeding to Phase 4.

---

## Phase 4: Training (Weeks 5–6)

### Admin training

The customer's workspace admin receives four hours of hands-on training with the CaseForge IE. See the [Admin Training Guide](admin-training-guide.md) for the full syllabus.

At the end of admin training, the admin should be able to:

- Add, deactivate, and manage user accounts without IE assistance
- Create and modify workflows using the admin console
- Configure and test notification rules
- Generate standard operational reports
- Diagnose and resolve the ten most common support issues (covered in the training guide)

### End-user training

End-user training is delivered by the customer's admin, not directly by CaseForge. The IE provides:

- A presentation deck: shared two weeks before go-live
- A user quick-start guide: a condensed version of the end-user workflow guide, branded for the customer if requested
- A training environment: a sandbox tenant pre-populated with sample cases for hands-on practice

Customers may request a CaseForge-led training webinar for end users — this is a chargeable service. Confirm with the CSM if the customer has contracted for this.

### User acceptance testing (UAT)

UAT runs in parallel with training, typically in week 6. The customer's PM leads UAT — the IE provides a UAT test script template covering all contracted workflows.

UAT pass criteria:
- All priority workflows exercise successfully end-to-end by at least two testers
- No P1 (data loss, security) or P2 (blocking functional issue) defects open at close of UAT
- All contracted integrations validated in the production tenant

P3 (cosmetic, non-blocking) defects may be deferred to post-go-live. Document them in the shared project tracker.

**Milestone 4 — Training and UAT complete:** Customer admin training complete; all UAT test cases passed or deferred with agreed resolution dates. Sign-off from the customer PM required before setting the go-live date.

---

## Phase 5: Go-live (Week 7)

The go-live phase covers final validation, cutover, and initial production monitoring. See the [Go-Live Checklist](go-live-checklist.md) for the step-by-step execution plan.

**Go-live day sequence:**

1. IE runs the pre-go-live validation checklist (morning of go-live day).
2. Customer confirms legacy system is set to read-only or decommissioned.
3. IE changes tenant configuration from staging to production.
4. Customer admin confirms access and creates the first production case.
5. CaseForge CSM sends go-live confirmation to the customer executive sponsor.
6. Hypercare monitoring begins.

**Milestone 5 — Go-live complete:** Production traffic confirmed; at least one case created by the customer in production; all stakeholders notified.

---

## Phase 6: Hypercare (Weeks 8–12)

The hypercare period provides elevated support and monitoring during the first weeks of production use.

During hypercare:

- The assigned IE is available for same-business-day response on all support issues
- The CSM holds weekly check-ins with the customer PM to review open issues, usage metrics, and any configuration adjustments
- The IE monitors error logs and integration health daily during weeks 8–9, then three times per week in weeks 10–12

**Hypercare exit criteria:**

- No P1 or P2 issues open
- All contracted integrations operating without manual intervention for at least seven consecutive days
- Customer admin team comfortable with day-to-day administration (confirmed by admin, not assumed)
- First 30-day usage report reviewed with the customer PM

At the hypercare exit call, the CSM transitions the account to standard enterprise support and introduces the customer to the support escalation path.

**Milestone 6 — Hypercare complete:** Exit criteria met; customer PM has confirmed readiness to transition to standard support; support contact information shared with all admins.

---

## Milestone summary

| Milestone | Phase | Typical week | Sign-off required |
|---|---|---|---|
| M1: Environment ready | Phase 1 | Week 2 | Customer IT admin |
| M2: Integrations complete | Phase 2 | Week 4 | Customer IT admin |
| M3: Configuration and migration complete | Phase 3 | Week 5 | Customer PM |
| M4: Training and UAT complete | Phase 4 | Week 6 | Customer PM |
| M5: Go-live complete | Phase 5 | Week 7 | Customer PM + CaseForge CSM |
| M6: Hypercare complete | Phase 6 | Week 12 | Customer PM + CaseForge CSM |

---

## Escalation path

| Situation | Contact |
|---|---|
| Implementation blocker (integration failure, data issue) | CaseForge Implementation Engineer (direct) |
| Schedule slip or scope change | CaseForge CSM |
| Executive escalation | CaseForge VP of Customer Success |
| Product defect or security concern | CaseForge Support (support.caseforge.io) + CSM |

All escalations should be logged in the shared project tracker, even if resolved informally. This ensures the full implementation history is available for the post-implementation review.

---

## Document revision history

| Version | Date | Change |
|---|---|---|
| 1.0 | January 2024 | Initial release |
| 1.1 | June 2024 | Added OIDC SSO section; expanded data migration checklist |
| 2.0 | January 2025 | Updated for CaseForge 3.x; added hypercare exit criteria |
| 2.1 | March 2025 | Revised UAT pass criteria; added CRM and ticketing integrations |
