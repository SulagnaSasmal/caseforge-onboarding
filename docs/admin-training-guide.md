# CaseForge Admin Training Guide

**Applies to:** CaseForge 3.x  
**Audience:** Customer workspace administrators  
**Training duration:** 4 hours (two 2-hour sessions recommended)  
**Last reviewed:** March 2025

---

## Overview

This guide covers the CaseForge administration skills that a workspace admin needs to manage the platform day-to-day without requiring CaseForge support for routine tasks. By the end of training, you will be able to:

- Manage user accounts, roles, and access
- Create and modify case workflows
- Configure notification rules
- Generate and schedule operational reports
- Diagnose the most common user-reported issues

Training is hands-on. You will complete these tasks in your CaseForge staging environment during the session. The staging environment is a full replica of your production tenant — changes you make during training do not affect production.

---

## Prerequisites

Before training, confirm the following:

- [ ] Your staging environment is accessible at `https://[tenant]-staging.caseforge.io`
- [ ] Your admin account is active and you can log in
- [ ] SSO is configured (if your organization uses SSO, training uses SSO login)
- [ ] You have reviewed the [Getting Started section](#module-1-the-admin-console) of this guide

Contact your CaseForge IE if any of the above is not ready — training requires a working environment.

---

## Module 1: The admin console

**Duration:** 20 minutes  
**Goal:** Understand the admin console layout and know where to find each administrative function.

### Accessing the admin console

The admin console is available to users with the **Workspace Admin** role. To open it:

1. Click your avatar in the top-right corner of any CaseForge page.
2. Select **Admin Console** from the dropdown.

The admin console opens in a separate browser tab. It is distinct from the main CaseForge interface — changes made here affect the entire workspace.

### Admin console sections

| Section | What you manage here |
|---|---|
| **Users** | User accounts, roles, SSO assignment, account deactivation |
| **Groups** | User groups for bulk role assignment and report filtering |
| **Workflows** | Case types, stages, transitions, required fields |
| **Notifications** | Email notification rules, templates, and delivery settings |
| **Integrations** | Connected services (read-only view; changes require the IE) |
| **Reports** | Scheduled and on-demand operational reports |
| **Audit Log** | Tamper-evident log of all admin actions |
| **Settings** | Tenant name, logo, timezone, data retention policy |

> **Audit Log:** Every action you take in the admin console is recorded in the Audit Log, including the timestamp and your account. This is a compliance requirement for most CaseForge customers. You cannot delete or modify audit log entries.

---

## Module 2: User management

**Duration:** 40 minutes  
**Goal:** Manage the full user lifecycle — invite, activate, assign roles, and deactivate.

### User roles

CaseForge has four default roles. Your organization may have customized these — check the **Roles** page in the admin console for your current role definitions.

| Role | What users in this role can do |
|---|---|
| **Viewer** | Read cases assigned to their groups; cannot create or edit cases |
| **Agent** | Create and edit cases; manage their own cases; cannot access admin functions |
| **Supervisor** | All Agent permissions plus: reassign cases, manage queue, view all team cases |
| **Workspace Admin** | Full access including the admin console; can create all case types and modify all workflows |

Assign the Workspace Admin role with care — users with this role can modify workflows, change notification rules, and access all cases in the tenant regardless of group assignment.

### Inviting a new user

1. Navigate to **Admin Console → Users → Invite user**.
2. Enter the user's email address.
3. Select their role.
4. (Optional) Assign them to one or more groups.
5. Click **Send invitation**.

The user receives an invitation email with a link valid for 48 hours. If they do not accept within 48 hours, resend the invitation from the **Users** page — the original link expires and cannot be extended.

If your organization uses SSO, the user activates their account by completing SSO login — they do not set a password. The invitation email still triggers the SSO flow.

### Changing a user's role

1. Navigate to **Admin Console → Users**.
2. Find the user by name or email using the search bar.
3. Click the user's name to open their profile.
4. In the **Role** dropdown, select the new role.
5. Click **Save**.

Role changes take effect immediately. The user does not need to log out and back in.

### Deactivating a user

1. Open the user's profile in the admin console.
2. Click **Deactivate account**.
3. Confirm the deactivation in the dialog.

Deactivated users cannot log in, but their case history and audit trail are preserved. You can reactivate a deactivated account at any time from the same profile page. Do not delete user accounts — deletion is permanent and removes the user from case history, which creates audit gaps.

### Bulk operations

For bulk role changes or group assignments (for example, during an organizational restructure), use the **Import users** function:

1. Navigate to **Admin Console → Users → Import**.
2. Download the CSV template.
3. Populate the CSV with the changes.
4. Upload the CSV and review the preview.
5. Confirm the import.

The import processes asynchronously — large imports (500+ users) may take 5–10 minutes. You receive an email when the import completes.

---

## Module 3: Workflow management

**Duration:** 60 minutes  
**Goal:** Create and modify case workflows without IE assistance.

### What is a workflow?

A workflow in CaseForge defines:
- **Case type** — the category of case (for example, Customer Dispute, Fraud Investigation, or Exception Review)
- **Stages** — the ordered steps a case moves through (for example: New → Under Review → Escalated → Resolved → Closed)
- **Transitions** — which roles can move a case from one stage to another
- **Required fields** — which fields must be completed before a case can advance to the next stage

Every case belongs to exactly one case type. Changes to a workflow affect all cases of that type — including cases currently in progress. Plan workflow changes carefully.

### Creating a new case type

1. Navigate to **Admin Console → Workflows → New case type**.
2. Enter the case type name and description.
3. Click **Create**.

A new case type starts with two default stages: **Open** and **Closed**. You build the stages between them.

### Adding and configuring stages

1. In the workflow editor, click **+ Add stage**.
2. Name the stage (use the term your team uses, not a technical abbreviation).
3. Set the stage color — used to distinguish stages in case list views and dashboards.
4. Define **entry rules** — conditions that must be true for a case to enter this stage. Example: a required attachment must be present.
5. Define **exit rules** — which roles can move a case out of this stage.
6. Click **Save stage**.

Repeat for each stage. Use drag handles to reorder stages — the order determines the default case progression, but cases can advance or regress based on your transition rules.

### Adding required fields

Required fields enforce data completeness before a case can advance. To add a required field to a stage transition:

1. Click the transition arrow between two stages in the workflow editor.
2. Click **+ Add required field**.
3. Select an existing field from the dropdown, or click **Create new field** to add a custom field.
4. Mark the field as required for this specific transition.
5. Save.

When a user attempts to advance a case past this transition without completing the required field, CaseForge blocks the action and displays a validation message listing the missing fields.

### Versioning and published workflows

Workflows have a **Draft** and a **Published** state. Draft changes are visible only in the workflow editor — they do not affect active cases. Publish a workflow to make changes live.

> **Warning:** Publishing a workflow immediately affects all in-progress cases of that type. If you remove a stage that currently contains active cases, those cases are moved to the previous stage automatically. Notify your team before publishing significant workflow changes.

To publish:
1. Review the changes in the workflow editor.
2. Click **Publish workflow**.
3. Enter a description of the changes (stored in the workflow audit trail).
4. Confirm.

---

## Module 4: Notification rules

**Duration:** 30 minutes  
**Goal:** Configure notification rules so the right people are emailed at the right time.

### How notification rules work

A notification rule combines:
- A **trigger event** (for example: case created, case assigned, case stage changed, deadline approaching)
- A **recipient filter** (who receives the notification: the assignee, the case creator, a specific group, or all supervisors)
- A **message template** (subject line and body, with field substitution variables)

### Viewing existing rules

Navigate to **Admin Console → Notifications**. All active notification rules are listed here. Enabled rules have a green indicator; disabled rules are grayed out.

Click any rule to view or edit it. The email preview shows how the notification looks with sample data substituted.

### Creating a notification rule

1. Click **New rule**.
2. Select the trigger event from the dropdown.
3. Set the recipient filter. For most events, start with the **Assignee** recipient — add more recipients only if there's a documented business need. Too many notifications reduce the signal-to-noise ratio and lead to ignoring them.
4. Edit the message template. Field substitution variables are listed in the right panel — click any variable to insert it into the template.
5. Toggle the rule on.
6. Click **Save**.

### Testing a notification rule

After saving a new rule, test it before go-live:

1. Create a test case in the staging environment.
2. Trigger the event the rule responds to.
3. Check that the notification email arrives within two minutes.
4. Review the email content — confirm that substitution variables resolved correctly (no visible `{{variable_name}}` placeholders in the received email).

If the notification does not arrive, check:
- The rule is enabled (green indicator in the rule list)
- The recipient user has a valid email address in their CaseForge profile
- Email delivery is functioning (send a test email from **Settings → Email → Test delivery**)

---

## Module 5: Reports

**Duration:** 30 minutes  
**Goal:** Generate and schedule operational reports for common management use cases.

### Standard reports

CaseForge includes built-in report templates for the most common operational needs. Navigate to **Admin Console → Reports** to see the full library.

| Report | What it shows |
|---|---|
| **Case volume** | Cases created, closed, and in progress per time period; filterable by case type and group |
| **Resolution time** | Average and median time from case creation to closure, by case type and assignee |
| **Stage aging** | Cases that have been in a given stage longer than a threshold; used to identify stuck cases |
| **User activity** | Actions taken per user over a time period — useful for capacity planning |
| **SLA compliance** | Cases resolved within defined SLA targets vs. those that breached; requires SLA rules to be configured |

To run a report, click it, set the date range and any filters, and click **Generate**. Reports render in the browser and can be exported as CSV or PDF.

### Scheduling a report

To receive a report automatically on a schedule:

1. Open the report.
2. Click **Schedule**.
3. Set the frequency (daily, weekly, monthly).
4. Select the delivery time and recipients.
5. Click **Save schedule**.

Scheduled reports are sent as PDF attachments. Recipients do not need a CaseForge account to receive scheduled reports.

### Creating a custom report

If the standard reports don't meet a specific need:

1. Navigate to **Admin Console → Reports → New report**.
2. Select the primary data object (Cases, Users, or Audit events).
3. Choose the fields to include as columns using the field picker.
4. Apply filters to narrow the data set.
5. Preview the report.
6. Save with a name and description.

Custom reports appear in the report library alongside standard reports. You can schedule custom reports the same way as standard ones.

---

## Module 6: Common issues and diagnostics

**Duration:** 30 minutes  
**Goal:** Diagnose and resolve the ten most common support issues without escalating to CaseForge support.

### 1. User cannot log in via SSO

- Confirm the user is assigned to the CaseForge application in the IdP.
- Confirm the user's email in the IdP matches the email in their CaseForge user profile (case-sensitive comparison for some IdPs).
- Check the Audit Log (**Admin Console → Audit Log**) for authentication failure entries for the user's email.
- If the Audit Log shows no entry for the login attempt, the issue is in the IdP — the request is not reaching CaseForge.

### 2. User not receiving notifications

- Confirm the notification rule is enabled.
- Confirm the user's email address in their CaseForge profile is correct.
- Confirm the notification rule's recipient filter matches the user's role or group.
- Check the user's spam folder — ask them to add `noreply@[tenant].caseforge.io` to their safe senders list.
- Send a test email from **Settings → Email → Test delivery** to the user's address. If it doesn't arrive, there is a delivery issue.

### 3. User cannot see a case

Cases are visible only to users whose group assignment matches the case's access group, plus supervisors and admins. If a user cannot see a case they should be able to access:
- Confirm the case's access group in the case Details tab.
- Confirm the user belongs to that group in **Admin Console → Users → [User] → Groups**.
- Agents can only see cases they are assigned to or cases in groups they belong to. Supervisors can see all cases in their groups.

### 4. Workflow stage not advancing

If a user reports that a case is stuck and they cannot advance it to the next stage:
- Open the case and check the stage transition validation message — it lists the specific required fields that are missing.
- Confirm the user's role has transition permissions for this stage (check **Admin Console → Workflows → [Case type] → [Stage] → transitions**).
- If the stage advance button is grayed out with no error message, check whether the case has an open task that must be completed before the transition. Open tasks are shown in the **Tasks** tab.

### 5. Integration stopped working

If a connected integration (Salesforce, Jira, SharePoint, etc.) stops delivering data:

1. Navigate to **Admin Console → Integrations → [Integration name]**.
2. Click **Connection health** — this shows the last successful sync and any error messages.
3. The most common cause is an expired credential (API token, OAuth token, or service account password).  
   - For OAuth integrations: click **Reconnect** and complete the OAuth flow again.
   - For API token integrations: ask the customer IT admin to generate a new token and update it in CaseForge.
4. If the health check shows a network error, escalate to the customer IT admin — a firewall change may be blocking the integration endpoint.

### 6. Attachment upload failing

- Maximum attachment size is 50 MB per file. Files larger than this are rejected at the browser with a clear error message.
- Supported file types are configured in **Admin Console → Settings → Allowed file types**. Uploads of unsupported types are silently blocked in some browser configurations — add the relevant type if needed.
- If a user reports attachments "disappearing" after upload, the document storage integration may be misconfigured. Check **Admin Console → Integrations** for storage errors.

### 7. Report showing no data

- Confirm the date range includes a period when cases existed. A common mistake is selecting "last 7 days" immediately after go-live when few cases have been created.
- Confirm the case type and group filters are not over-scoping the query.
- If the report is a custom report, check that the filter conditions are correct — an AND filter where OR was intended is a common cause.

### 8. Duplicate user accounts

If a user has two accounts (one SSO, one email/password from before SSO was enabled):
1. Identify both email addresses from the Users list.
2. Confirm with the user which account holds their active cases.
3. Reassign any cases from the account to be deactivated.
4. Deactivate the unused account.  
   Do not delete either account — deactivation preserves audit history.

### 9. Admin console not loading

- Confirm the user has the Workspace Admin role — the admin console is not accessible to other roles.
- Clear the browser cache and try again (admin console assets are cached aggressively).
- Try a different browser or incognito mode to rule out a browser extension conflict.
- If the admin console does not load in any browser, check the CaseForge status page at `status.caseforge.io`.

### 10. Scheduled report not delivering

- Confirm the schedule is active (navigate to the report and check the **Schedules** tab).
- Confirm the recipient email addresses are valid.
- Check that report delivery is not blocked by a spam filter — ask recipients to check their junk folder.
- Re-run the report manually. If it renders correctly, the issue is with scheduled delivery — contact CaseForge support.

---

## Post-training assessment

After completing all six modules, you should be able to complete the following tasks independently, without referring to this guide:

1. Invite a new user, assign them to a group, and set their role to Agent
2. Create a new case type with three stages and one required field on the middle transition
3. Create a notification rule that emails the assignee when a case is escalated
4. Run the Stage Aging report for the past 30 days, filtered to one case type
5. Diagnose why a user is not receiving notifications and resolve the issue

If you are not confident in any of these areas, ask your CaseForge IE to run a focused practice session before go-live.

---

## Quick reference card

Print or save this section for day-to-day reference.

| Task | Path in admin console |
|---|---|
| Invite a user | Users → Invite user |
| Deactivate a user | Users → [User] → Deactivate account |
| Change a user's role | Users → [User] → Role dropdown |
| Create a workflow stage | Workflows → [Case type] → + Add stage |
| Publish a workflow change | Workflows → [Case type] → Publish workflow |
| Create a notification rule | Notifications → New rule |
| Run a report | Reports → [Report name] → Generate |
| Schedule a report | Reports → [Report name] → Schedule |
| View audit log | Audit Log |
| Test email delivery | Settings → Email → Test delivery |
| Check integration health | Integrations → [Integration name] → Connection health |
