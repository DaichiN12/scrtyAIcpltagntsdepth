# Lab 02: Entra Agent Identity Configuration and Monitoring

## Introduction

Zava's security team has received confirmation from the CISO that all AI agent identities must be reviewed and brought under governance before Day 2 security policy configuration begins. The registry check in Lab 01 confirmed that agents are active and visible — but agent identity ownership has not been assigned, and no one has verified what permissions or roles these identities currently hold.

MOD Administrator will review each agent identity in Entra, assign Patti Fernandes as owner of the Zava Finance Agent, inspect available audit and sign-in logs, and test the identity disable action to confirm that disabling an agent identity effectively blocks end-user access.

Every Copilot Studio agent deployed in the Zava environment was automatically assigned a unique identity in Microsoft Entra ID when Entra Agent Identity was enabled in Lab 00. These identities appear in the **Agent ID** section of the Microsoft Entra admin center and can be governed like any other identity in the tenant — with owners, sponsors, access controls, audit logs, and Conditional Access policies.

In this lab, MOD Administrator will locate the Zava agent identities, review their current configuration, assign Patti Fernandes as owner, inspect activity logs, and disable and re-enable the Zava Finance Agent identity to simulate an identity quarantine action.

---

## Objectives

- Locate all Zava agent identities in the Microsoft Entra admin center via Entra agents.
- Review agent identity metadata including Status, Sponsors, Owners, Blueprint ID, Object ID, and Created on date.
- Assign Patti Fernandes as owner of the Zava Finance Agent identity.
- Review current permissions and Entra roles assigned to the agent identity.
- Inspect available audit log and sign-in log entries for the agent identity.
- Review Conditional Access policy and Access package links from the agent identity panel.
- Disable the Zava Finance Agent identity and verify that end-user access is blocked.
- Re-enable the Zava Finance Agent identity and confirm it returns to Active status.

---

## Lab Duration

Estimated time: **45 minutes**

---

## Exercise 1: Locate and Inspect the Zava Finance Agent Identity

### Task 1: Navigate to Entra Agent Identities

1. Open a browser and navigate to `https://entra.microsoft.com`.

2. Sign in with **MOD Administrator** credentials if prompted.

3. In the left navigation pane, select **Agent ID**.

4. On the **All agent identities (Preview)** page, review the list of agent identities registered in the tenant.

5. Confirm that the following three agents appear in the list:

   | Display Name | Status |
   |---|---|
   | Zava HR Assistant (Microsoft Copilot Studio) | Active |
   | Zava Finance Agent (Microsoft Copilot Studio) | Active |
   | Zava IT Support Agent (Microsoft Copilot Studio) | Active |

   > **Note:** Agent identities are suffixed with **(Microsoft Copilot Studio)** to indicate the platform that provisioned them. If any agent is not listed, wait five minutes and refresh the page. Agent identity provisioning can take time after initial publishing in Copilot Studio.

---

### Task 2: Review the Zava Finance Agent Identity Overview

1. On the **All agent identities (Preview)** page, select **Zava Finance Agent (Microsoft Copilot Studio)**.

2. On the **Overview (Preview)** page, review and note the following fields:

   - **Status** — confirm it reads **Active**.
   - **Sponsors** — note the user avatars currently listed as sponsors.
   - **Owners** — confirm the current value. Note whether an owner is assigned or whether the field shows a dash ( **-** ), indicating no owner is set.
   - **Blueprint ID** — note the GUID value.
   - **Object ID** — note the GUID value.
   - **Agent blueprint** — note the link text (Microsoft Copilot Studio agent identity).
   - **Created on** — note the date.

3. On the right panel, under **Agent identity's access**, note the current values for:

   - **Permissions**
   - **Entra roles**

   > **Note:** In a newly provisioned environment, both values will show **0**. This confirms that the Zava Finance Agent identity has not been granted any API permissions or Entra directory roles, which is the expected least-privilege starting state.

---

### Task 3: Assign Patti Fernandes as Owner of the Zava Finance Agent Identity

1. In the left sub-navigation of the Zava Finance Agent identity page, under **Access**, select **Owners and sponsors (Preview)**.

2. On the **Owners and sponsors** page, confirm that no owners are currently listed.

4. Select **+ Add** > **Add owner**.

5. In the search field on the **Add owners** panel, enter `Patti`.

6. Select **Patti Fernandes** from the results.

7. Select **Select** to confirm.

8. Confirm that **Patti Fernandes** now appears as the **Owners** on the **Owners and sponsors** page.

   > **Note:** Assigning an owner to an agent identity establishes accountability for that identity within the Entra governance model. Owners receive access review notifications and are responsible for attesting to the identity's continued need and appropriate access.
---

### Task 4: Review Agent Identity Access

1. In the left sub-navigation, under **Access**, select **Agent identity's access (Preview)**.

2. On the **Agent identity's access** page, review the **Permissions** section.

3. Note whether any API permissions are listed.

4. Review the **Entra roles** section.

5. Note whether any directory roles are assigned to this identity.

   > **Note:** In a newly created Copilot Studio agent identity, no permissions or roles will be assigned. This confirms the identity is operating with zero standing access to Microsoft Graph or directory resources. Later in the labs, Conditional Access policies will be applied to control how and when this identity can authenticate.

---

## Exercise 2: Review Activity Logs for the Agent Identity

### Task 1: Review Sign-In Logs

1. In the left sub-navigation of the Zava Finance Agent identity page, under **Activity**, select **Audit logs (Preview)**.

2. On the **Sign-in logs** page, review the list of sign-in entries under **Service principal sign-ins**.

3. If entries are present, select any entry to open the detail panel and review the information.

5. Close the detail panel.

   > **Note:** Audit log entries for a newly created agent identity will typically include the initial provisioning event (Create service principal). Additional entries will appear as the identity is used, modified, or subject to policy changes throughout the course.

<!--

### Task 2: Review Sign-In Logs

1. In the left sub-navigation, under **Activity**, select **Sign-in logs (Preview)**.

2. On the **Sign-in logs** page, review any entries listed.

3. If entries are present, select any entry to open the detail panel.

4. On the detail panel, note the following fields:

   - **Date and time**
   - **Application**
   - **Status**
   - **IP address**
   - **Resource**

5. Close the detail panel.

   > **Note:** Sign-in log entries for agent identities appear when the agent authenticates to access a knowledge source or connected service. In a newly deployed environment with no user interactions yet, the sign-in log may be empty. As agents are invoked throughout Day 2 and Day 3 labs, sign-in entries will populate and become available for investigation.

## Exercise 3: Review Governance Links

### Task 1: Review Conditional Access Policies Applied to the Agent Identity

1. In the left sub-navigation, select **Overview (Preview)** to return to the agent identity overview page.

2. On the right panel, under **Policies & ID Governance**, locate **CA policies**.

3. Select **View** next to **CA policies**.

4. Review the list of Conditional Access policies currently applied to this agent identity.

5. Note whether any policies are listed.

   > **Note:** In a newly configured environment, no Conditional Access policies will be targeting this agent identity yet. This is expected. In Lab 04 (Day 2), Conditional Access policies will be created and scoped to Zava agent identities to enforce access controls. Reviewing this page now establishes the baseline — zero policies applied.

---

### Task 2: Review Access Packages

1. Return to the **Overview (Preview)** page.

2. On the right panel, under **Policies & ID Governance**, locate **Access packages**.

3. Select **View** next to **Access packages**.

4. Review the list of access packages currently associated with this agent identity.

5. Note whether any access packages are listed.

   > **Note:** Access packages are part of Microsoft Entra ID Governance and are used to manage bundled access rights with approval workflows and time-limited assignments. No access packages are associated with the Zava agent identities at this stage. This confirms the current unmanaged state and provides a reference point for future governance configuration.

---

-->

## Exercise 3: Disable and Re-enable the Zava Finance Agent Identity

### Task 1: Disable the Zava Finance Agent Identity

1. In the left sub-navigation, select **Overview (Preview)** to return to the Zava HR Assistant Agent Identity overview page.

2. In the toolbar at the top of the page, select **Disable**.

3. In the confirmation dialog, confirm the action to disable the identity.

4. Wait for the page to refresh.

5. On the **Overview (Preview)** page, confirm that **Status** now reads **Disabled**.

---

### Task 2: Verify that End-User Access is Blocked

1. Open a new **InPrivate** or **Incognito** browser window.

2. Navigate to `https://copilot.microsoft.com`.

3. Sign in with **Adele Vance** credentials from the **Resources** tab.

4. In the Microsoft 365 Copilot chat interface, select the agent picker or enter `@Zava Finance Agent` in the chat input field.

5. Attempt to invoke the **Zava Finance Agent**.

6. Note the response — the agent should be unavailable or return an error indicating it cannot be accessed.

   > **Note:** Identity disable propagation may take up to five minutes. If the agent responds normally immediately after disabling, wait three to five minutes and attempt again. Do not proceed to Task 3 until the agent is confirmed unavailable.

7. Close the InPrivate browser window.

---

### Task 3: Re-enable the Zava Finance Agent Identity

1. Return to the **MOD Administrator** browser session at `https://entra.microsoft.com`.

2. In the left navigation pane, select **Agent ID**.

3. On the **All agent identities (Preview)** page, select **Zava Finance Agent (Microsoft Copilot Studio)**.

4. On the **Overview (Preview)** page, in the toolbar, select **Enable**.

   > **Note:** The toolbar button will have changed from **Disable** to **Enable** after the identity was disabled in Task 1.

5. In the confirmation dialog, confirm the action to enable the identity.

6. Wait for the page to refresh.

7. On the **Overview (Preview)** page, confirm that **Status** now reads **Active**.

8. Open a new **InPrivate** or **Incognito** browser window.

9. Navigate to `https://copilot.microsoft.com`.

10. Sign in with **Adele Vance** credentials.

11. Attempt to invoke the **Zava Finance Agent** again.

12. Confirm that the agent responds normally.

13. Close the InPrivate browser window.

---

## Summary

In this lab, you located all three Zava agent identities in the Microsoft Entra admin center using the **Entra agents** left navigation entry. You reviewed the Zava Finance Agent identity overview, confirming its Active status, Blueprint ID, Object ID, and the current absence of assigned owners. You assigned Patti Fernandes as owner of the Zava Finance Agent identity to establish accountability within the Entra governance model. You reviewed the agent identity's current permissions and Entra roles, confirming zero standing access as expected in a least-privilege deployment. You inspected available audit log and sign-in log entries to establish the activity baseline for this identity. You reviewed the Conditional Access policy and Access package links from the governance panel, confirming no policies or packages are currently applied — establishing the baseline for Day 2 configuration. Finally, you disabled the Zava Finance Agent identity to simulate an identity quarantine action, verified that Adele Vance could no longer invoke the agent via Microsoft 365 Copilot, and re-enabled the identity to restore normal access.

The Zava agent identities are now confirmed as visible, governed with ownership assigned, and responsive to identity-level lifecycle controls. Day 1 is complete. Day 2 labs build on this foundation to apply security policies, Conditional Access controls, and threat detection configuration across the Zava agent environment.
