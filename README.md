# CaseForge Customer Onboarding Kit

This repository contains the onboarding documentation package for new CaseForge customers. It covers the full post-sale implementation journey — from contract close through go-live and first 90 days.

**Intended audiences:**

| Document | Primary audience |
|---|---|
| [Implementation Guide](docs/implementation-guide.md) | Customer project manager, IT lead |
| [Integration Checklist](docs/integration-checklist.md) | IT administrator, developer |
| [Admin Training Guide](docs/admin-training-guide.md) | CaseForge workspace administrator |
| [Go-Live Checklist](docs/go-live-checklist.md) | Customer project manager, CaseForge CSM |
| [CSM Playbook](docs/csm-playbook.md) | CaseForge Customer Success Manager (internal) |

---

## About CaseForge

CaseForge is a cloud-based case management platform for financial services and compliance teams. It provides workflow automation, audit trails, document management, and regulatory reporting across investigations, exception handling, and customer dispute resolution.

---

## Onboarding timeline

```
Week 1–2    Kickoff, access provisioning, environment setup
Week 3–4    Data migration, SSO configuration, integration setup
Week 5–6    Admin training, user acceptance testing (UAT)
Week 7      Go-live validation, cutover
Week 8–12   Hypercare, stabilization, first retrospective
```

Full milestone details are in the [Implementation Guide](docs/implementation-guide.md).

---

## How to use this kit

Each document is standalone — share individual files with the relevant stakeholder rather than sending the full package. The recommended sequence:

1. Share the **Implementation Guide** with the customer project manager at kickoff.
2. Work through the **Integration Checklist** with the customer IT team during weeks 2–4.
3. Deliver **Admin Training Guide** content to the customer's designated workspace admins during week 5.
4. Complete the **Go-Live Checklist** with the customer PM in the week before cutover.
5. Use the **CSM Playbook** internally throughout the engagement.

---

## Document versions

This kit applies to **CaseForge v3.x** (released Q1 2025). If your customer is on an earlier version, contact the Solutions Engineering team for the legacy onboarding kit.

---

*Maintained by the CaseForge Customer Success team. Last reviewed: March 2025.*

## Dark / Light Mode

All pages support dark and light themes via a toggle button in the header. The selected theme persists in `localStorage` and system `prefers-color-scheme` is honored on first visit.

## Status

**Phase 3 — Complete / Active Customer Deployment**

| Document | Status |
|----------|--------|
| Implementation Guide | Complete |
| Integration Checklist | Complete |
| Admin Training Guide | Complete |
| Go-Live Checklist | Complete |
| CSM Playbook | Complete |
| Dark / light theme support | Complete |

This kit targets **CaseForge v3.x** (released Q1 2025). Individual customer deployments will have their own open checklist items.

## Future Enhancements

- v4.x migration addendum (when CaseForge releases a major version)
- SCIM provisioning integration checklist
- Embedded API integration guide for customer developer teams
- Video walkthrough companion for admin training module
