# CaseForge Integration Checklist

**Applies to:** CaseForge 3.x  
**Owner:** CaseForge Implementation Engineer  
**Last reviewed:** March 2025

Use this checklist during Phase 2 of the implementation (weeks 3–4). Work through each section with the customer IT administrator. Mark items complete only after end-to-end testing — not after configuration alone.

---

## How to use this checklist

- Complete sections in order. Later items depend on earlier ones (for example, user sync requires SSO to be working).
- Both the CaseForge IE and the customer IT admin must confirm each item before marking it complete.
- Log blockers in the shared project tracker immediately — do not hold them until the weekly check-in.
- Items marked **[Required]** must be complete before go-live. Items marked **[Conditional]** apply only if this integration is in scope.

---

## 1. Network and access prerequisites

- [ ] **[Required]** CaseForge tenant URL is accessible from the customer network (no proxy or firewall blocking outbound HTTPS)
- [ ] **[Required]** CaseForge IP ranges added to customer allowlist (if egress filtering is in use)

  CaseForge IP ranges (current): `34.120.0.0/16`, `34.149.0.0/16`. Confirm with the IE that these are current at time of implementation — ranges are subject to change with 30-day notice.

- [ ] **[Required]** Customer admin account activated and password changed from temporary credential
- [ ] **[Conditional]** Dedicated service account created in the customer directory for CaseForge integrations (required if using LDAP sync or on-premises document storage)
- [ ] **[Conditional]** Outbound webhook endpoint accessible from CaseForge (required if customer will receive CaseForge webhook events)

---

## 2. Single sign-on

Complete the SSO section before configuring user sync. SSO must be working before you can verify that synced users can log in.

### 2.1 SSO method: SAML 2.0

- [ ] CaseForge Service Provider metadata XML provided to customer IT admin
- [ ] SAML application created in the customer IdP (Okta, Azure AD, ADFS, or other)
- [ ] IdP metadata XML or metadata URL provided to CaseForge IE
- [ ] Attribute mappings confirmed:

  | CaseForge field | Mapped to IdP attribute |
  |---|---|
  | Email | |
  | First name | |
  | Last name | |
  | Groups | |

- [ ] SSO login tested with at least one pilot user — login completes without error
- [ ] SSO login tested with a user who is **not** assigned the CaseForge application — access denied as expected
- [ ] SSO enabled for the production tenant (not just testing)

### 2.2 SSO method: OIDC

- [ ] Redirect URI provided to customer IT admin: `https://[tenant].caseforge.io/auth/callback`
- [ ] OIDC application created in the customer IdP; discovery endpoint URL provided to CaseForge IE
- [ ] Client ID and client secret configured in CaseForge tenant settings
- [ ] OIDC login tested with a pilot user — login completes without error
- [ ] Token expiry and refresh behavior confirmed (default: 60-minute session, 24-hour refresh token)

### 2.3 Post-SSO verification

- [ ] User can reach the CaseForge tenant via SSO without entering a separate CaseForge password
- [ ] SSO-provisioned user appears in the CaseForge admin user list with correct name and email
- [ ] Deprovisioning tested: user removed from IdP application; CaseForge session terminates and re-login denied

---

## 3. User directory sync

Directory sync keeps the CaseForge user list in sync with the customer's identity provider. Changes in the IdP (new hires, departures, group changes) propagate to CaseForge automatically.

- [ ] **[Conditional]** Active Directory or LDAP sync configured (required if customer manages users via on-premises AD)
  - LDAP server hostname and port provided to CaseForge IE
  - Service account DN and password configured in CaseForge
  - Sync filter confirmed (which OUs or groups to sync)
  - Initial sync completed — user count in CaseForge matches expected count
  - Incremental sync verified (add a test user in AD; confirm they appear in CaseForge within the sync interval)

- [ ] **[Conditional]** SCIM provisioning configured (required if customer uses Okta, Azure AD, or another SCIM-capable IdP)
  - SCIM endpoint URL and bearer token provided to customer IT admin
  - SCIM application configured in the IdP
  - Test user pushed via SCIM — user appears in CaseForge with correct attributes
  - Test deprovision — user deactivated in IdP; CaseForge account deactivated within one sync cycle

- [ ] Group-to-role mapping confirmed: IdP groups map to the correct CaseForge roles

  | IdP group | CaseForge role |
  |---|---|
  | | |
  | | |
  | | |

---

## 4. Email

CaseForge sends notifications (case assignments, status changes, deadlines) via email. This section ensures email delivery is working before go-live.

- [ ] **[Required]** Outbound email method selected: CaseForge-managed relay (default) or customer SMTP

  If using customer SMTP:
  - [ ] SMTP hostname, port, and authentication credentials provided to CaseForge IE
  - [ ] SMTP credentials entered in CaseForge tenant settings
  - [ ] Test email sent from CaseForge settings page — delivery confirmed

- [ ] **[Required]** Notification sender address confirmed (`noreply@[tenant].caseforge.io` or custom sender domain)
- [ ] **[Required]** SPF and DKIM records added to the customer's DNS (CaseForge IE provides the required record values)
- [ ] Test notification email delivered to at least three different recipient email addresses, including one on a different domain from the primary
- [ ] Notification emails not landing in spam (if they are, work with the customer IT admin to configure SPF/DKIM correctly)

---

## 5. Document storage

CaseForge stores case attachments in an integrated document storage system. Choose one of the following options.

### 5.1 CaseForge-managed storage (default)

- [ ] Customer confirmed that CaseForge-managed storage (S3-backed, encrypted at rest) is acceptable
- [ ] Data residency region confirmed and configured (US-East, EU-West, or APAC)
- [ ] Storage quota reviewed and appropriate plan confirmed

No further configuration required for CaseForge-managed storage.

### 5.2 Customer-provided storage

- [ ] **SharePoint Online:**
  - SharePoint site URL and document library path confirmed
  - CaseForge Azure app registration granted `Files.ReadWrite.All` permission on the target site
  - OAuth consent completed by the SharePoint tenant admin
  - Test file upload from CaseForge to SharePoint confirmed
  - Test file retrieval from SharePoint into CaseForge case confirmed

- [ ] **Amazon S3:**
  - S3 bucket name and region provided to CaseForge IE
  - IAM policy with `s3:GetObject`, `s3:PutObject`, `s3:DeleteObject` on the bucket applied to the CaseForge service role
  - Bucket encryption confirmed (AES-256 or AWS KMS)
  - Test file upload and retrieval confirmed

- [ ] **Google Drive / Google Shared Drive:**
  - Shared Drive ID provided to CaseForge IE
  - CaseForge service account granted `Contributor` access to the Shared Drive
  - Test file upload and retrieval confirmed

---

## 6. CRM integration (conditional)

Complete this section only if the customer has contracted for CRM integration.

### 6.1 Salesforce

- [ ] Salesforce org type confirmed: Production or Sandbox (Sandbox used for UAT)
- [ ] Connected app created in Salesforce with `api`, `refresh_token`, `offline_access` OAuth scopes
- [ ] Consumer key and secret provided to CaseForge IE
- [ ] OAuth flow completed; CaseForge can query Salesforce contacts and accounts
- [ ] Field mapping confirmed: which Salesforce fields display on CaseForge case sidebars

  | Salesforce field | Displayed as in CaseForge |
  |---|---|
  | | |

- [ ] Test case created in CaseForge; linked Salesforce contact displayed correctly

### 6.2 HubSpot

- [ ] HubSpot portal ID confirmed
- [ ] Private app token created in HubSpot with `crm.contacts.read` and `crm.companies.read` scopes
- [ ] Token provided to CaseForge IE and configured in tenant settings
- [ ] Test case created in CaseForge; linked HubSpot contact displayed correctly

---

## 7. Ticketing integration (conditional)

Complete this section only if the customer has contracted for ticketing integration.

### 7.1 Jira

- [ ] Jira instance URL confirmed (Cloud or Server/Data Center)
- [ ] API token generated by Jira admin; provided to CaseForge IE
- [ ] CaseForge connection tested: can read Jira project list
- [ ] Default Jira project for CaseForge-created tickets confirmed
- [ ] Field mapping confirmed: CaseForge case fields → Jira issue fields
- [ ] Test ticket created from a CaseForge case — appears in Jira with correct field values
- [ ] Jira status change syncs back to CaseForge case timeline

### 7.2 ServiceNow

- [ ] ServiceNow instance URL confirmed
- [ ] Integration user account created in ServiceNow with `itil` role
- [ ] Username and password provided to CaseForge IE
- [ ] Test incident created from CaseForge — appears in ServiceNow
- [ ] Incident resolution in ServiceNow triggers status update in CaseForge

---

## 8. Webhooks (conditional)

Complete this section only if the customer will consume CaseForge webhook events in their own systems.

- [ ] Webhook endpoint URL provided by the customer
- [ ] Endpoint returns HTTP 200 within 10 seconds (CaseForge times out and retries after 10 seconds)
- [ ] HTTPS confirmed on the endpoint (HTTP endpoints are not supported)
- [ ] Webhook secret configured in CaseForge; customer confirms payload signature validation is implemented
- [ ] Test event triggered; customer confirms payload received and signature verified
- [ ] Event subscriptions confirmed (which event types the customer wants to receive):

  - [ ] `case.created`
  - [ ] `case.status_changed`
  - [ ] `case.assigned`
  - [ ] `case.closed`
  - [ ] `case.document_uploaded`
  - [ ] Other: _______________

---

## Sign-off

Before proceeding to Phase 3 (Configuration and data migration), both parties must confirm that all **[Required]** items are complete and all in-scope **[Conditional]** items are complete or have an agreed resolution date.

| Item | Status | Notes |
|---|---|---|
| Network and access prerequisites | | |
| SSO | | |
| User directory sync | | |
| Email | | |
| Document storage | | |
| CRM integration (if applicable) | | |
| Ticketing integration (if applicable) | | |
| Webhooks (if applicable) | | |

**Customer IT admin sign-off:** ________________________________ Date: __________

**CaseForge IE sign-off:** ________________________________ Date: __________
