# CaseForge CSM Playbook

**Internal document — CaseForge Customer Success team only**  
**Applies to:** Enterprise accounts on CaseForge 3.x  
**Last reviewed:** March 2025

---

## Purpose

This playbook guides the Customer Success Manager through the full post-sale engagement lifecycle — from kickoff through renewal. It covers the CSM's responsibilities at each phase, common customer scenarios with recommended responses, and escalation protocols.

This document is a reference, not a script. Use your judgment based on individual customer context. If this playbook conflicts with a specific account agreement or SOW, the SOW takes precedence.

---

## Your role as CSM

The CSM is the customer's primary relationship owner after the sale closes. Your responsibilities are:

- **Engagement health** — hold regular check-ins, surface risks before they become escalations, and ensure the customer is progressing toward their defined success outcomes
- **Implementation oversight** — own the project timeline, coordinate between the customer PM and the CaseForge IE, and enforce milestone gates
- **Adoption** — monitor usage metrics post-go-live and proactively engage customers whose adoption is lower than expected
- **Renewal** — position the renewal conversation at least 90 days before contract end; involve the account executive for expansion opportunities
- **Voice of customer** — document customer feedback and product gaps in the internal feedback system; represent the customer's needs in product planning discussions

You are not the technical owner of the implementation — that is the IE. You are not the sales owner of the account — that is the AE. Your position is at the intersection: you own the relationship and the outcome.

---

## Phase 1: Pre-kickoff (contract close to day 7)

### Your tasks

- [ ] Review the signed order document — confirm contracted modules, user count, integrations, and any custom terms
- [ ] Introduce yourself to the customer executive sponsor and PM via email within 24 hours of contract close
- [ ] Schedule the kickoff call within five business days of contract close
- [ ] Create the project tracker in the CS tool (Gainsight / your internal tool) and populate the implementation timeline
- [ ] Assign an IE and confirm their availability for the kickoff date
- [ ] Confirm the customer received tenant provisioning credentials (the IE sends these — verify with the IE)

### Email template: CSM introduction

```
Subject: Welcome to CaseForge — [Customer Name]

Hi [Sponsor name],

I'm [Your name], your Customer Success Manager at CaseForge. I'll be your main 
point of contact through implementation and beyond.

I've scheduled our kickoff call for [date/time] — you should have the calendar 
invite. I'll be joined by [IE name], who leads our technical implementation.

In the meantime, [IE name] will reach out to [IT admin name] to begin environment 
access setup.

Looking forward to working with you.

[Signature]
```

---

## Phase 2: Implementation (kickoff through go-live)

### Weekly check-in format

Run a 30-minute weekly check-in with the customer PM every week until hypercare exit. Standard agenda:

1. **Milestone status** (5 min) — green/yellow/red on each open milestone
2. **Blockers** (10 min) — what's preventing progress; who owns resolution; deadline
3. **This week's tasks** (10 min) — what the customer team and CaseForge team need to complete before the next call
4. **Risks** (5 min) — anything that could affect the go-live date; discuss early, not when it's too late

Take notes in the project tracker during the call. Send a summary within two hours. Keep summaries factual — record what was agreed, not context or interpretation.

### Milestone gate process

Do not allow the team to skip a milestone sign-off even if "it's basically done." Milestone sign-offs are the customer PM's formal confirmation that a phase is complete. They:

- Create a clear record of what was delivered and when
- Give the customer a natural point to raise any issues before moving forward
- Protect CaseForge from scope creep disputes ("I thought X was included in the implementation")

If a customer PM is reluctant to sign off ("we trust you, just keep going"), explain that the sign-off protects them — if something is unexpectedly wrong, it's much easier to address it at a milestone than after go-live.

### Common implementation risks

**Risk: IT admin unavailable during integration sessions**

- Symptoms: Sessions are scheduled but the IT admin joins unprepared, lacks necessary credentials, or is pulled to other priorities.
- Impact: Integration delays that compress the testing window.
- Response: At kickoff, get the IT admin's direct contact. Confirm each session 48 hours in advance with a preparation checklist. If the admin is consistently unavailable, escalate to the executive sponsor — frame it as a project risk, not a performance issue.

**Risk: Data migration is larger or messier than scoped**

- Symptoms: Customer provides a data export that is far larger than estimated, has significant data quality issues, or is in a non-standard format.
- Impact: Migration slips, potentially pushing go-live.
- Response: Surface this immediately. Work with the IE to estimate revised migration effort. If the additional work is out of scope, involve the AE for a scope amendment before proceeding. Do not absorb unscoped migration work silently — it creates a precedent and affects margin.

**Risk: UAT reveals significant workflow rework**

- Symptoms: UAT test cases are failing due to workflow configuration mismatches with the customer's actual processes.
- Impact: Rework in Phase 3 delays the Phase 4 timeline.
- Response: Assess severity with the IE. Minor misconfigurations (field labels, stage names) are resolved quickly. Structural workflow redesign (adding or removing stages, changing transition rules) requires more time. Adjust the go-live date proactively rather than accepting a compressed timeline — a rushed go-live produces a poor customer experience.

---

## Phase 3: Go-live and hypercare (weeks 7–12)

### Go-live day responsibilities

- Be available for the full change window — not just the kickoff of the cutover activity.
- Confirm that the go-live checklist is being worked in order. Do not let the team skip verification steps.
- Send the go-live confirmation email to the executive sponsor within one hour of go-live sign-off (template below).
- Log the go-live date in the CS tool and trigger the hypercare onboarding workflow.

### Email template: Go-live confirmation

```
Subject: CaseForge is live — [Customer Name]

Hi [Sponsor name],

Great news — CaseForge went live for [Customer Name] today at [time].

Your team can now access the platform at https://[tenant].caseforge.io.

For the next four weeks, we're in an elevated support period. [IE name] 
is monitoring the system daily and available for same-business-day response 
on any issues. Weekly check-ins with [PM name] continue on [day/time].

Thank you to the [Customer Name] team for a smooth implementation. We're 
looking forward to seeing the impact.

[Signature]
```

### Hypercare monitoring

During hypercare (weeks 8–12):

| Week | CSM activity |
|---|---|
| 8 | Weekly check-in; confirm no P1/P2 issues; review adoption metrics (first week logins) |
| 9 | Review integration health with IE; confirm notification delivery is stable |
| 10 | Review usage data — are users creating cases, or is adoption stalling? Proactive outreach if usage is low |
| 11 | Pre-exit review with customer PM — are exit criteria being met? |
| 12 | Hypercare exit call; transition to standard support; introduce renewal timeline |

### Adoption red flags

If you see any of the following in the first 30 days, take action immediately — do not wait for the next check-in:

- Fewer than 50% of provisioned users have logged in within two weeks of go-live
- No cases have been created (or only test cases) after the first week
- Integration delivering no data (zero syncs)
- Admin reports receiving calls about the same issue three or more times (indicates a training gap, not a one-off)

**Low adoption response:** Schedule a 30-minute call with the customer PM and admin. Do not draw conclusions without hearing their perspective first — low adoption is sometimes intentional (phased rollout) and sometimes a systemic issue. If it's systemic, bring in the IE for a configuration review.

---

## Phase 4: Steady-state and renewal (months 3–12)

### Quarterly business review (QBR)

Schedule a QBR at months 3, 6, and 9. QBR agenda:

1. **Usage metrics** — cases created, closed, average resolution time vs. baseline (if baseline was established)
2. **Value delivered** — tie metrics to the business outcomes the customer defined at kickoff
3. **Open items** — any unresolved configuration questions, deferred integrations, or enhancement requests
4. **Product roadmap** — share relevant upcoming features and how they apply to this customer's use case
5. **Renewal preview** (at month 9) — confirm renewal intent, start the expansion conversation if applicable

### Health score

Update the health score in the CS tool after each check-in. Use the following signals:

| Signal | Weight | Green | Yellow | Red |
|---|---|---|---|---|
| Active users / licensed users | High | >70% | 40–70% | <40% |
| Cases created (last 30 days) | High | Increasing or stable | Declining | Zero |
| Open P1/P2 support tickets | High | 0 | 1–2 with plan | 3+ or unresponsive |
| Last check-in date | Medium | <14 days | 14–30 days | >30 days |
| Renewal engagement | High (at 90-day mark) | Renewal doc signed or verbal confirmation | In conversation | No engagement |

A Yellow health score requires a documented action plan in the CS tool. A Red health score requires CSM manager review within five business days.

### Renewal playbook

- **T-90 days:** Send renewal notification; schedule renewal discussion with economic buyer
- **T-60 days:** If no response to renewal outreach, escalate to CSM manager + AE
- **T-45 days:** Multi-thread — contact both the sponsor and the PM if the sponsor is unresponsive
- **T-30 days:** Renewal document should be signed by this date for a clean renewal

> If the renewal is at risk due to a product or service issue, do not escalate the price or commercial discussion until the issue is resolved. Customers renew based on value delivered, not commercial pressure — if value has not been delivered, no amount of commercial incentive will sustain the relationship.

---

## Escalation protocol

### Internal escalations (you escalating within CaseForge)

| Situation | Escalate to | Timeline |
|---|---|---|
| IE unresponsive to customer for >24 hours | IE manager | Immediately |
| Implementation milestone slipping by >5 days with no resolution plan | Implementation manager | Within 24 hours of identifying the slip |
| Customer threatening churn | CSM manager + AE | Immediately |
| Product defect blocking go-live | Product + Engineering via priority support ticket | Immediately; also alert CSM manager |
| Data breach or security incident | Security team + CSM manager | Immediately; follow the security incident runbook |

### External escalations (customer escalating to CaseForge)

If a customer escalates beyond you — contacting your manager, the CIO, or submitting a formal complaint:

1. Thank the customer for raising the concern and confirm you will investigate.
2. Notify your manager within 30 minutes.
3. Prepare a factual summary: what happened, when, what the current status is, and what the resolution path is.
4. Respond to the customer within two business hours with the summary and a defined resolution timeline.

Do not send an apology that admits CaseForge fault without a factual basis. Do not promise outcomes you cannot control.

---

## Tools and resources

| Resource | Location |
|---|---|
| Customer project tracker | Gainsight → Accounts → [Customer] |
| Implementation document library | Shared Google Drive (Implementation → [Customer]) |
| Health score dashboard | Gainsight → Reports → CS Health Dashboard |
| Support ticket escalation | support.caseforge.io → Priority flag → CSM escalation |
| Product roadmap (internal) | Confluence → Product → Roadmap (current quarter) |
| CaseForge status page | status.caseforge.io |
| IE assignment and availability | CS Engineering calendar (Shared Calendar → Implementation) |
