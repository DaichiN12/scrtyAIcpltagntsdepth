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

Estimated time: **10 minutes**

---

## Exercise 1: Locate and Inspect the Zava Finance Agent Identity

### Task 1: Navigate to Entra Agent Identities

1. Open a browser and navigate to `https://entra.microsoft.com`. Sign in with **MOD Administrator** credentials if prompted.

3. In the left navigation pane, select **Agent ID**. On the **All agent identities (Preview)** page, review the list of agent identities registered in the tenant.

	![](./media/image1.png)

5. Confirm that the following three agents appear in the list:

   | Display Name | Status |
   |---|---|
   | Zava HR Assistant (Microsoft Copilot Studio) | Active |
   | Zava Finance Agent (Microsoft Copilot Studio) | Active |
   | Zava IT Support Agent (Microsoft Copilot Studio) | Active |

	![](./media/image2.png)

   > **Note:** Agent identities are suffixed with **(Microsoft Copilot Studio)** to indicate the platform that provisioned them. If any agent is not listed, wait five minutes and refresh the page. Agent identity provisioning can take time after initial publishing in Copilot Studio.

---

### Task 2: Review the Zava Finance Agent Identity Overview

1. On the **All agent identities (Preview)** page, select **Zava Finance Agent (Microsoft Copilot Studio)**.

	![](./media/image3.png)

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

	![](./media/image4.png)

   > **Note:** In a newly provisioned environment, both values will show **0**. This confirms that the Zava Finance Agent identity has not been granted any API permissions or Entra directory roles, which is the expected least-privilege starting state.

---

### Task 3: Assign Patti Fernandes as Owner of the Zava Finance Agent Identity

1. In the left sub-navigation of the Zava Finance Agent identity page, under **Access**, select **Owners and sponsors (Preview)**.

	![](./media/image5.png)

2. On the **Owners and sponsors** page, confirm that no owners are currently listed. Select **+ Add** > **Add owner**.

	![](./media/image6.png)

5. In the search field on the **Add owners** panel, enter `Patti`. Select **Patti Fernandes** from the results. Select **Select** to confirm.

	![](./media/image7.png)

8. Confirm that **Patti Fernandes** now appears as the **Owners** on the **Owners and sponsors** page.

	![](./media/image8.png)

   > **Note:** Assigning an owner to an agent identity establishes accountability for that identity within the Entra governance model. Owners receive access review notifications and are responsible for attesting to the identity's continued need and appropriate access.
---

## Exercise 2: Disable and Re-enable the Zava HR Assistant

### Task 1: Disable the Zava HR Assistant Identity

1. On the **All agent identities (Preview)** page in Microsoft Entra navigate to the **Zava HR Assistant** Agent Identity overview page.

	![](./media/image9.png)

2. In the toolbar at the top of the page, select **Disable**.

	![](./media/image10.png)

3. In the confirmation dialog, confirm the action to disable the identity.

	![](./media/image11.png)

4. Wait for the page to refresh.

5. On the **Overview (Preview)** page, confirm that **Status** now reads **Disabled**.

	![](./media/image12.png)

---

### Task 2: Verify that End-User Access is Blocked

1. Open a new **InPrivate** or **Incognito** browser window. Navigate to `https://copilot.microsoft.com`. Sign in with **MOD Administrator** credentials from the **Resources** tab.

4. In the left navigation, select **All agents** and then search for `Zava`.

6. Note the results — the agent **Zava HR Assistant** should not be visible.

   > **Note:** Identity disable propagation may take up to five minutes. If the agent responds normally immediately after disabling, wait three to five minutes and attempt again. Do not proceed to Task 3 until the agent is confirmed unavailable.

---

### Task 3: Re-enable the Zava Finance Agent Identity

1. Return to the **MOD Administrator** browser session at `https://entra.microsoft.com`.

2. In the left navigation pane, select **Agent ID**. On the **All agent identities (Preview)** page, select **Zava HR Agent (Microsoft Copilot Studio)**.

4. On the **Overview (Preview)** page, in the toolbar, select **Enable**.

	![](./media/image13.png)

   > **Note:** The toolbar button will have changed from **Disable** to **Enable** after the identity was disabled in Task 1.

5. In the confirmation dialog, confirm the action to enable the identity.

	![](./media/image14.png)

6. Wait for the page to refresh.

7. On the **Overview (Preview)** page, confirm that **Status** now reads **Active**.

	![](./media/image15.png)

8. Repeat the process to check the End-User access.

---

## Summary

In this lab, you located all three Zava agent identities in the Microsoft Entra admin center using the **Entra agents** left navigation entry. You reviewed the Zava Finance Agent identity overview, confirming its Active status, Blueprint ID, Object ID, and the current absence of assigned owners. You assigned Patti Fernandes as owner of the Zava Finance Agent identity to establish accountability within the Entra governance model. You reviewed the agent identity's current permissions and Entra roles, confirming zero standing access as expected in a least-privilege deployment. You inspected available audit log and sign-in log entries to establish the activity baseline for this identity. You reviewed the Conditional Access policy and Access package links from the governance panel, confirming no policies or packages are currently applied — establishing the baseline for Day 2 configuration. Finally, you disabled the Zava Finance Agent identity to simulate an identity quarantine action, verified that Adele Vance could no longer invoke the agent via Microsoft 365 Copilot, and re-enabled the identity to restore normal access.

The Zava agent identities are now confirmed as visible, governed with ownership assigned, and responsive to identity-level lifecycle controls. Day 1 is complete. Day 2 labs build on this foundation to apply security policies, Conditional Access controls, and threat detection configuration across the Zava agent environment.
