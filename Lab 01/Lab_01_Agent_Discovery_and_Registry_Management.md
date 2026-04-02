# Lab 01: Agent Discovery and Registry Management

## Introduction

Zava's CISO has asked the security team to confirm that all deployed AI agents are visible, governed, and accounted for before any security policy work begins. At the same time, the team must ensure that the monitoring and threat detection infrastructure is ready for further configuration. Patti Fernandes, Zava's Security Admin and SOC Analyst, will be granted the permissions she needs to perform the tasks and will use the Agent Registry in Microsoft Entra ID to inspect the three Zava agents, test lifecycle controls, and identify any governance gaps. The Defender XDR workspace and Defender for Cloud Apps connector will be initialised so that agent activity data begins flowing before moving further in their security journey.

This lab introduces the Microsoft 365 Admin Center Agent Registry and Microsoft Entra Agent Registry as the primary tool for agent discovery, lifecycle management, and governance. Patti will explore the registry, review agent metadata, take lifecycle actions, identify ownerless agents, and complete the prerequisite configuration tasks required for Day 2 security labs — including Purview Audit verification, Defender XDR provisioning, and Defender for Cloud Apps initialisation.

---

## Scenario

---

## Objectives

- Assign Patti Fernandes the Security Operator role in Microsoft Admin Center.
- Assign Patti Fernandes the Security Operator role in Microsoft Defender XDR.
- Assign Patti Fernandes to the Audit Reader role group in Microsoft Purview.
- Explore the Agent 365 Overview dashboard and interpret key metrics.
- Inspect all three Zava agents in the Agent Registry and review their metadata.
- Block and unblock the Zava HR Assistant to validate lifecycle controls.
- Export the agent inventory to confirm audit trail capability.
- Identify ownerless agents using the registry dashboard filter.
- Verify that Purview Audit is active and run a baseline audit log search.
- Provision Microsoft Defender XDR by signing in to the Defender portal.
- Configure Defender for Cloud Apps organisation details and connect the Microsoft 365 app connector.


---

## Lab Duration

Estimated time: **75 minutes**

---

## Exercise 1: Explore the Agent 365 Overview and Agent Registry

### Task 1: Access the Agent 365 Overview Page

1. Open a browser and navigate to `https://admin.microsoft.com`.

2. Sign in with **Patti Fernandez** credentials if prompted. (Use pattif@<TenantName> as ID and User Password as the password)

3. In the left navigation pane, expand **Agents**. and then select **Overview**.

5. On the **Agent Overview** page, locate the following metrics and note their current values:

   - **Agent Registry** — total count of agents in the tenant.
   - **Active users in Copilot** — unique users who interacted with an agent in the last 30 days.
   - **Pending requests for agents** — Open requests to add specific agents.
   - **Agents without owners** — Agents whose owner has left the company.
   - **Agent analytics** - Agents by creators, Top platforms used to build agents, Active users in Copilot over time
 
> **Note:** In a freshly configured environment, active user and agents without owners may show zero. This is expected. The metrics will populate as agents are used throughout the course.

---

### Task 2: Approve an agent in Teams admin center

1. Open a new browser tab and navigate to `https://admin.teams.microsoft.com/` and log in using the MOD Admin credentials.

2. From the left navigation under Teams apps, select **Manage apps**.

3. In the search bar, select **Zava HR Assistant**.

4. On the **Zava HR Assistant** page, select **Publish**.

---

### Task 3: Inspect and approve the Zava Agents in the Agent Registry

1. Move back on the admin center or open a browser and navigate to `https://admin.microsoft.com`.

2. In the left navigation pane, select **Agents**.

3. Select **All agents**.

4. On the **All agents** page, select the **Requests** tab.

5. In the agent list, locate **Zava IT Support Agent** and select the verticla ... near the name.

6. From the two options you can either **Reject** or **Publish** the agent. For now select **Publish**.

7. Select the Registry tab, search for `Zava IT Support Agent Review`, select it and note the information in the pane.

---

### Task 3: Block and Unblock the Zava HR Assistant

1. On the **Registry** tab, search for and locate **Zava HR Assistant** in the agent list.

2. Select **Zava HR Assistant** to open its details panel.

3. On the details panel, below the agent name, select **Block**.

4. On the **Block agent** pane that opens, review the message confirming that blocking will prevent all users in the organisation from accessing the agent.

5. Check the box near **Block agent**.

6. Select **Save**.

7. Close the details panel.

8. In the agent list, confirm that **Zava HR Assistant** now displays a **Blocked** status.

9. Select **Zava HR Assistant** to open its details panel again.

10. On the details panel, below the agent name, select **Unblock**.

11. On the **Unblock agent** pane that opens, select **Unblock agent**.

12. Select **Save**.

13. Close the details panel.

14. In the agent list, confirm that **Zava HR Assistant** now displays an **Active** status.

---

### Task 4: Export the Agent Inventory

1. On the **Registry** tab, Select **Export** on the toolbar above the agent list.

   > **Note:** If an **Export** button is not visible in the toolbar, select the ellipsis (**...**) menu in the toolbar to locate the export option.

3. Wait for the export file to be generated and downloaded to your lab VM.

4. Open the downloaded CSV file.

5. Confirm that the file contains rows for **Zava HR Assistant**, **Zava Finance Agent**, and **Zava IT Support Agent**.

6. Confirm that the following columns are present: agent name, publisher, creator, creation date, host products, and availability status.

7. Close the CSV file.

---

### Task 5: Identify Ownerless Agents

1. On the **Registry** tab, locate the **Agent inventory** section above the agent list.

2. Select the **Missing an owner** card.

3. Review the list of agents that are displayed after applying the ownerless filter.

4. Note whether any of the three Zava agents appear in this filtered list.

   > **Note:** In a lab environment where agents were created by MOD Administrator, the agents may or may not appear as ownerless depending on how ownership is propagated from Copilot Studio. If no agents appear, this confirms that ownership was correctly assigned during creation. If agents appear, this represents a governance gap that would be addressed by reassigning ownership.

5. Select **Clear filter** or reset the filters to return to the full agent list.

---

## Exercise 2: Prepare Purview Audit for Day 2
<!--
### Task 1: Assign Patti Fernandes the Audit Reader Role in Microsoft Purview

1. Open a new browser tab and navigate to `https://purview.microsoft.com`.

2. Sign in with **MOD Administrator** credentials if prompted.

3. In the left navigation pane, select **Settings**.

4. Under **Settings**, select **Roles and scopes**.

5. Select **Role groups**.

6. On the **Role groups** page, in the search bar, enter `Audit Reader`.

7. Select **Audit Reader** from the results.

8. On the **Audit Reader** details panel, select **Edit**.

9. On the **Edit role group** page, select **Choose users**.

10. In the search field, enter `Patti`.

11. Select **Patti Fernandes** from the results.

12. Select **Select** to confirm.

13. Select **Next**.

14. Select **Save** to apply the role group membership.

15. Confirm that a success notification appears confirming that the role group was updated.

---
-->

### Task 1: Verify Purview Audit Is Active

1. Remain in the Microsoft Purview portal at `https://purview.microsoft.com`.

2. In the left navigation pane, select **Solutions** then select **Audit**.

3. On the **Audit** page, check whether a banner appears prompting you to start recording user and admin activity.

   - If a banner is displayed, select **Start recording user and admin activity** to enable auditing.
   - If no banner is displayed, auditing is already enabled. Proceed to the next step.

4. Configure the search with the following values:

   - **Start date:** Select today's date minus 3 day.
   - **End date:** Select today's date.
   - **Activities – friendly names:** Leave blank to search all activities.
   - **Users:** Leave blank.
   - **Record type:** Leave blank.

5. Select **Search**.

6. Wait for the search job to complete.

7. Review the results to confirm that audit records are being returned.

   > **Note:** If the search returns no results, this may indicate that no audited activities have occurred yet in the tenant, or that audit log ingestion requires additional time after initial provisioning. This is expected in a new lab environment. The absence of results does not indicate a configuration problem. Audit records generated throughout this and subsequent labs will be searchable from Day 2 onwards.

---

### Task 2: Run a Baseline Copilot Studio Audit Search (optional)

1. On the **Audit** page, configure the search with the following values:

   - **Start date:** Select today's date minus 7 days.
   - **End date:** Select today's date.
   - **Activities – friendly names:** Enter `Copilot Studio` and select any matching activity types shown in the dropdown.
   - **Users:** Leave blank.

2. Select **Search**.

4. Wait for the search job to complete.

5. Review the results.

6. If results are returned, select any result row to open the audit record detail panel.

7. On the detail panel, note the following fields:

   - **Date**
   - **User**
   - **Activity**
   - **Item**
   - **Detail**

8. Close the detail panel.

   > **Note:** This baseline search establishes the audit starting point for Zava's agent environment. In Day 2 labs, Patti Fernandes will run targeted audit searches to investigate specific agent and data activity events.

---

## Exercise 3: Initialise Microsoft Defender XDR and Defender for Cloud Apps

### Task 1: Provision Microsoft Defender XDR

1. Open a new browser tab and navigate to `https://security.microsoft.com`.

2. Sign in with **MOD Administrator** credentials if prompted.

3. On the **Microsoft Defender** portal welcome screen, review the provisioning message if displayed.

   > **Note:** Microsoft Defender XDR provisions automatically when an eligible admin visits the portal for the first time. If provisioning is in progress, a message will indicate the data centre location being used and an estimated completion time. Wait for provisioning to complete before continuing.

4. Once the portal loads fully, confirm that the left navigation pane displays the following sections: **Home**, **Incidents & alerts**, **Hunting**, **Threat intelligence**, **Assets**, **Identities**, **Endpoints**, **Email & collaboration**, **Cloud Apps**, and **Settings**.

5. Select **Home** to confirm the Defender XDR home dashboard loads without errors.

---

### Task 2: Configure Defender for Cloud Apps Organisation Details

1. In the Microsoft Defender portal at `https://security.microsoft.com`, in the left navigation pane, select **Settings**, under **System**.

2. On the **Settings** page, select **Cloud Apps**.

3.  select **Organisation details**.

4. On the **Organisation details** page, in the **Organisation display name** field, enter `Zava Corporation`.

5. In the **Environment name** field, enter `Zava-Prod`.

6. In the **Managed domains** field, enter your tenant's primary domain in the following format:
   `[TenantName].onmicrosoft.com`

   > **Note:** Replace `[TenantName]` with your tenant prefix from the **Resources** tab. Adding managed domains ensures that internal users are correctly identified in Cloud Apps reports and alerts.

7. Select **Save**.

8. Confirm that a success notification appears confirming that the settings were saved.

---

### Task 3: Connect the Microsoft 365 App Connector

1. In the Microsoft Defender portal, in the left navigation pane, select **Settings** under **System**.

2. On the **Settings** page, select **Cloud Apps**.

3. Under **Connected apps**, select **App Connectors**.

4. On the **App Connectors** page, select **+ Connect an app**.

5. In the app list, select **Microsoft 365**.

6. On the **Select Microsoft 365 components** page, confirm that all components are selected by default. If any component is deselected, select it to enable it.

7. Select **Connect**.

8. Wait for the connection to complete.

9. On the **App Connectors** page, confirm that **Microsoft 365** appears in the connectors list with a status of **Connected**.

10. On the **App Connectors** page, select the check box near **Microsoft 365** and from the top options select **Connect Microsoft Azure Instance**.

11. Wait for the connection to complete.

   > **Note:** After connecting, Defender for Cloud Apps begins scanning Microsoft 365 activity. Initial data from the past week will appear in the portal. The first full scan may take several hours depending on tenant size. This connector is required for activity monitoring, DLP policy enforcement, and alert generation in Day 2 and Day 3 labs.

---

### Task 4: Enable File Monitoring in Defender for Cloud Apps

1. In the Microsoft Defender portal, in the left navigation pane, select **Settings**.

2. On the **Settings** page, select **Cloud Apps**.

3. Under **Information Protection**, select **Files**.

4. On the **Files** page, select the **Enable file monitoring** checkbox.

5. Select **Save**.

6. Confirm that a success notification appears confirming that file monitoring was enabled.

---
<!--
### Task 5: Assign Patti Fernandes the Security Operator Role in Defender XDR

1. In the Microsoft Defender portal at `https://security.microsoft.com`, in the left navigation pane, select **Settings**.

2. On the **Settings** page, select **Microsoft Defender XDR**.

3. Under **Permissions**, select **Roles**.

   > **Note:** If **Roles** is not available under Microsoft Defender XDR settings, navigate to `https://entra.microsoft.com` to assign the role directly from the Entra admin center.

4. On the **Roles** page, locate and select **Security Operator**.

5. On the **Security Operator** role page, select **+ Add assignments** or **Edit**.

6. In the user search field, enter `Patti`.

7. Select **Patti Fernandes** from the results.

8. Select **Save** or **Assign** to apply the role assignment.

9. Confirm that **Patti Fernandes** appears in the **Security Operator** role assignments list.

   > **Note:** The Security Operator role grants Patti the ability to view and investigate alerts, access incidents, and use advanced hunting in the Defender portal. This role is required for all Day 2 Defender tasks. Her Security Admin role in Entra provides portal access but does not grant Defender XDR operational permissions independently.
-->
---

## Summary

In this lab, you explored the Agent 365 Overview dashboard and reviewed key governance metrics for the Zava tenant. You inspected all three Zava agents in the Agent Registry, reviewing their metadata, host products, and knowledge sources. You blocked and unblocked the Zava HR Assistant to verify that lifecycle controls function correctly and affect agent availability. You exported the agent inventory to a CSV file to confirm audit trail capability. You used the ownerless agent filter to check for governance gaps in agent ownership.

You then prepared the Day 2 monitoring infrastructure. You assigned Patti Fernandes to the Audit Reader role group in Microsoft Purview, verified that Purview Audit is active, and ran a baseline audit log search scoped to Copilot Studio activity. You provisioned Microsoft Defender XDR by accessing the Defender portal for the first time, configured Defender for Cloud Apps with Zava Corporation organisation details and managed domain, connected the Microsoft 365 app connector to begin activity data ingestion, enabled file monitoring, and assigned Patti Fernandes the Security Operator role in Defender XDR.

The Zava agent environment is now fully visible, governed, and ready for security policy configuration in Day 2.
