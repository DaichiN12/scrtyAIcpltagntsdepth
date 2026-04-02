# Lab 03: Conditional Access for Zava Agent Identities

## Introduction

Zava's CISO has mandated that only reviewed and approved AI agents may access company resources. Any agent that has not been through the governance review process must be blocked automatically. Additionally, if any agent identity shows signs of compromise — such as anomalous token acquisition behaviour — it must be blocked immediately without manual intervention.

MOD Administrator will implement both controls using Conditional Access for Agent Identities (Preview). Patti Fernandes will validate that policy evaluation is visible in sign-in logs. This lab establishes the Zava agent governance baseline that all subsequent security labs build upon.

Conditional Access for Agent Identities is a preview capability in Microsoft Entra ID that extends Zero Trust controls to AI agents. In this lab, MOD Administrator will create custom security attributes to classify the approval status of each Zava agent, build a Conditional Access policy that blocks all unapproved agent identities from accessing organisational resources, and create a second policy that blocks any agent identity exhibiting high-risk behaviour based on Entra ID Protection signals. The policies will first be validated in Report-only mode before being switched to enforcement. Adele Vance will invoke the Zava HR Assistant to generate agent sign-in events, which Patti Fernandes will then investigate in sign-in logs to confirm Conditional Access policy evaluation.

---

## Objectives

- Create a custom security attribute set and approval status attribute for agent classification.
- Assign approval status attributes to all three Zava agent identities.
- Create a Conditional Access policy that blocks all unapproved agent identities.
- Validate the policy scope using Report-only impact view to confirm an untagged agent would be blocked.
- Switch the policy to enforcement mode.
- Create a second Conditional Access policy that blocks high-risk agent identities.
- Generate agent sign-in events by invoking the Zava HR Assistant as Adele Vance.
- Investigate CA policy evaluation in agent identity sign-in logs.
- Verify CA policy association from the Entra agent identity panel.

---

## Lab Duration

Estimated time: **60 minutes**

---

## Exercise 1: Create Custom Security Attributes for Agent Governance

### Task 1: Assign the Attribute Definition Administrator Role

1. Open a browser and navigate to `https://entra.microsoft.com`.

2. Sign in with **MOD Administrator** credentials if prompted.

3. Under **Entra ID**, select **Roles & admins**.

4. In the search bar, enter `Attribute Definition Administrator`.

5. Select **Attribute Definition Administrator** by selecting its name. Do not select the checkbox.

5. On the **Attribute Definition Administrator** page, select **+ Add assignments**.

6. On the **Add assignments** panel, select **No members selected**.

7. Search for and select **MOD Administrator**.

8. Choose **Select** to confirm.

9. Select **Next**.

10. Under **Assignment type**, select **Active**.

11. In the activation panel, enter a justification — `Lab 03 custom security attribute configuration`.

12. Uncheck Permenently assigned and set duration to **1 hour**. Select **Assign**.

13. Confirm the assignment appears in the list under **Active assignments**.

14. Navigate back to **Roles & admins**.

15. In the search bar, enter `Attribute Assignment Administrator` and repeat the steps to assign the role to **MOD administrator**.

16. Select the **MOD Administrator** account icon in the top-right corner of the page.

17. Select **Sign out**.

18. Sign back in to `https://entra.microsoft.com` with **MOD Administrator** credentials.

> **Note:** The Attribute Definition Administrator role grants permissions to create and manage custom security attribute definitions. This role is intentionally excluded from Global Administrator to enforce separation of duties. A fresh sign-in is required for the new role assignment to take effect.

### Task 2: Create the AgentAttributes Attribute Set

3. In the left navigation pane, expand **Entra ID** and select **Custom security attributes**.

7. On the **Custom security attributes** page, select **+ Add attribute set**.

8. On the **Add attribute set** panel, in the **Attribute set name** field, enter `AgentAttributes`.

9. In the **Description** field, enter `Attribute set for classifying AI agent approval and governance status.`

10. In the **Maximum number of attributes** field, leave the default value.

11. Select **Add** to create the attribute set.

12. Confirm that **AgentAttributes** appears in the attribute set list.

---

### Task 3: Create the AgentApprovalStatus Attribute

1. On the **Custom security attributes** page, select **AgentAttributes** to open the attribute set.

2. On the **AgentAttributes** page, select **+ Add attribute**.

3. On the **Add attribute** panel, configure the following fields:

   - **Attribute name:** Enter `AgentApprovalStatus`.
   - **Description:** Enter `Tracks the approval status of each AI agent identity in the Zava governance review process.`
   - **Data type:** Select **String**.
   - **Allow multiple values to be assigned:** Select **Yes**.
   - **Only allow predefined values to be assigned:** Select **Yes**.

4. Under **Predefined values**, select **+ Add value**.

5. In the value field, enter `New`.

6. Select **+ Add value**.

7. In the value field, enter `In_Review`.

8. Select **+ Add value**.

9. In the value field, enter `HR_Approved`.

10. Select **+ Add value**.

11. In the value field, enter `Finance_Approved`.

12. Select **+ Add value**.

13. In the value field, enter `IT_Approved`.

14. Select **Add** to create the attribute.

15. Select **Save**.

16. Confirm that **AgentApprovalStatus** appears in the attributes list under **AgentAttributes**.

---

### Task 4: Assign HR_Approved to the Zava HR Assistant

1. In the left navigation pane, select **Agent ID**.

2. On the **All agent identities (Preview)** page, select **Zava HR Assistant (Microsoft Copilot Studio)**.

3. On the **Overview (Preview)** page, in the left sub-navigation, select **Custom security attributes (Preview)**.

4. On the **Custom security attributes** page, select **+ Add assignment**.

5. On the **Add custom security attribute assignment** panel, configure the following:

   - **Attribute set:** Select **AgentAttributes**.
   - **Attribute:** Select **AgentApprovalStatus**.
   - **Assigned values:** Select **HR_Approved** and select **Save**.

6. Select **Save** to apply the assignment.

7. Confirm that **AgentApprovalStatus** appears with the value **HR_Approved** on the custom security attributes page.

---

### Task 5: Assign Finance_Approved to the Zava Finance Agent

1. In the left navigation pane, select **Entra agents**.

2. On the **All agent identities (Preview)** page, select **Zava Finance Agent (Microsoft Copilot Studio)**.

3. In the left sub-navigation, select **Custom security attributes (Preview)**.

4. On the **Custom security attributes** page, select **+ Add assignment**.

5. On the **Add custom security attribute assignment** panel, configure the following:

   - **Attribute set:** Select **AgentAttributes**.
   - **Attribute:** Select **AgentApprovalStatus**.
   - **Assigned values:** Select **Finance_Approved**.

6. Select **Save** to apply the assignment.

7. Confirm that **AgentApprovalStatus** appears with the value **Finance_Approved**.

---
<!--
### Task 5: Assign IT_Approved to the Zava IT Support Agent

1. In the left navigation pane, select **Entra agents**.

2. On the **All agent identities (Preview)** page, select **Zava IT Support Agent (Microsoft Copilot Studio)**.

3. In the left sub-navigation, select **Custom security attributes (Preview)**.

4. On the **Custom security attributes** page, select **+ Add assignment**.

5. On the **Add custom security attribute assignment** panel, configure the following:

   - **Attribute set:** Select **AgentAttributes**.
   - **Attribute:** Select **AgentApprovalStatus**.
   - **Assigned values:** Select **IT_Approved**.

6. Select **Save** to apply the assignment.

7. Confirm that **AgentApprovalStatus** appears with the value **IT_Approved**.
---
-->

## Exercise 2: Create a CA Policy to Block Unapproved Agent Identities

### Task 1: Create the Policy and Configure Assignments

1. In the left navigation pane of the Microsoft Entra admin center, expand **Entra ID** then select **Conditional Access**.

3. On the **Conditional Access** page, select **Policies**.

4. On the **Policies** page, select **+ New policy**.

5. On the **New Conditional Access policy** page, in the **Name** field, enter `Zava - Block Unapproved Agent Identities`.

6. Under **Assignments**, select **0 users or agents (Preview) selected** under **Users or agents**.

7. On the assignments panel, under **What does this policy apply to?**, select **Agents (Preview)**.

8. Under **Include**, select **All agent identities (Preview)**.

9. Under **Exclude**, select **Select agent identities based on attributes**.

10. Set **Configure** to **Yes**.

11. Select **+ Add filter**.

12. In the filter configuration, under **AgentAttributes**, select the attribute **AgentApprovalStatus**.

14. Set **Operator** to **Contains**.

15. Set **Value** to **HR_Approved**.

16. Select **+ Add filter** to add a second exclusion.

17. In the filter configuration, under **AgentAttributes**, select the attribute **AgentApprovalStatus**.

19. Set **Operator** to **Contains**.

20. Set **Value** to **Finance_Approved**.

21. Select **+ Add filter** to add a third exclusion.

26. Select **Done** to confirm the exclusion configuration.

<!--
22. In the filter configuration, under **AgentAttributes**, select the attribute **AgentApprovalStatus**.

24. Set **Operator** to **Contains**.

25. Set **Value** to **IT_Approved**.
-->

---

### Task 2: Configure Target Resources and Access Controls

1. Under **Target resources**, select **Resources (formerly cloud apps)**.

2. Under **Include**, select **All resources (formerly 'All cloud apps')**.

3. Under **Access controls**, on the **Grant** panel, select **Block access**.

5. Select **Select** to confirm.

---

### Task 3: Validate Policy Scope in Report-Only Mode

1. Under **Enable policy**, keep **Report-only**.

2. Select **Create** to save the policy.

3. On the **Policies** page, select **Zava - Block Unapproved Agent Identities** to open the policy.

4. On the policy page, select **What If** to open the Report-only impact view.

   > **Note:** The What If tool allows you to simulate whether a specific identity would be affected by this policy without enforcing it.

5. On the **What If** panel, under **User or workload identity**, select **Agent identities**.

6. In the agent identity search field, search for and select **Zava Finance Agent (Microsoft Copilot Studio)**.

6. Under **Target resource**, set **Select target type** to **Cloud apps**.

7. Select **+ Select cloud app**.

8. In the search field, enter `Office 365 Sharepoint Online`.

9. Select **Office 365 Sharepoint Online** from the results.

10. Select **Select** to confirm.

11. Select **What if** to run the simulation.

8. Review the results and confirm that the policy **Zava - Block Unapproved Agent Identities** shows as **Not applied** — because the Zava Finance Agent is excluded by the `Finance_Approved` attribute.

9. Return to the **What If** panel.

10. Clear the current selection and simulate with a different identity to represent an untagged agent:

    > **Note:** To simulate an untagged agent, select any service principal in the tenant that does not have the `AgentApprovalStatus` attribute assigned. If no suitable identity is available, review the **Policies** tab of the What If results to confirm that the policy would apply to any agent identity not excluded by the attribute filter.

11. Select **Close** to exit the What If panel.

---

### Task 4: Switch the Policy to Enforcement Mode (Optional)

	> **Note:** You will not be able to perform this in the current environment as we had enabled Security Defaults in Lab 00 to enable publishing of Copilot Studio Agents

1. On the **Zava - Block Unapproved Agent Identities** policy page, select **Edit**.

2. Under **Enable policy**, select **On**.

3. Select **Save** to apply the change.

4. On the **Policies** page, confirm that **Zava - Block Unapproved Agent Identities** shows a status of **On**.

---

## Exercise 3: Create a CA Policy to Block High-Risk Agent Identities

### Task 1: Create the Policy and Configure Assignments

1. On the **Conditional Access** page, select **Policies**.

2. Select **+ New policy**.

3. In the **Name** field, enter `Zava - Block High Risk Agent Identities`.

4. Under **Assignments**, select **0 users or agents (Preview) selected** under **Users or agents**.

5. Under **What does this policy apply to?**, select **Agents (Preview)**.

6. Under **Include**, select **All agent identities (Preview)**.

1. Under **Target resources**, select **No target resources selected**, then select **All resources (formerly 'All cloud apps')**.

1. Under **Conditions**, under **Agent risk (Preview)**, select **Not Configured**.

2. On the **Agent risk** panel, set **Configure** to **Yes**.

3. Under **Configure agent risk levels needed for policy to be enforced**, select **High**.

4. Select **Done** to confirm the condition.

1. Under **Access controls**, under **Grant**, select **Block access**.

3. Select **Select** to confirm.

4. Under **Enable policy**, select **Report-only**.

   > **Note:** This policy is set to Report-only because agent risk signals from Entra ID Protection require active agent usage over time before risk levels are generated. In a newly provisioned lab environment, no risk signals will be present yet. Report-only mode allows the policy to be evaluated against future sign-in events without blocking access prematurely. In a production environment, this policy would be switched to On once baseline risk signal data is established.

5. Select **Create** to save the policy.

6. On the **Policies** page, confirm that **Zava - Block High Risk Agent Identities** appears with a status of **Report-only**.

---

## Exercise 4: Generate Agent Sign-In Events and Investigate CA Policy Evaluation

### Task 1: Invoke the Zava HR Assistant

1. Open a new **InPrivate** or **Incognito** browser window.

2. Navigate to `https://copilot.microsoft.com`.

3. Sign in with **Patti Fernandez's** credentials from the **Resources** tab.

4. In the Microsoft 365 Copilot chat interface, select the agent picker icon.

5. In the agent picker, search for and select **Zava HR Assistant**.

6. In the chat input field, enter the following:

   ```
   What is Zava's leave policy?
   ```

7. Wait for the Zava HR Assistant to respond.

8. Enter a second message in the chat input field:

   ```
   How do I submit a sick leave request?
   ```

9. Wait for the response.

   > **Note:** These interactions generate agent sign-in events as the Zava HR Assistant authenticates to access its SharePoint knowledge source. These events will appear in the Entra sign-in logs and will have Conditional Access policy evaluation recorded against them.

10. Close the InPrivate browser window.

---

### Task 2: Investigate Agent Sign-In Logs in Entra

1. Return to the **MOD Administrator** browser session at `https://entra.microsoft.com`.

2. In the left navigation pane, expand **Entra ID** and select **Monitoring & health**.

4. Select **Sign-in logs**.

5. On the **Sign-in logs** page, select the **Service principal sign-ins** tab.

6. In the filter bar, select **+ Add filters**.

7. Select **Is Agent** as the filter field and then select **Yes**.

9. Select **Apply** to apply the filter.

10. Review the sign-in entries returned in the filtered view.

11. Locate an entry associated with the **Zava HR Assistant** identity.

12. Expand the entry and select one log to open sign-in detail panel.

13. On the detail panel, select the **Conditional Access** tab.

14. Review the **Conditional Access policies** evaluated for this sign-in event.

15. Confirm that **Zava - Block Unapproved Agent Identities** appears in the evaluated policies list with a result of **Not applied** — because the Zava HR Assistant has the `HR_Approved` attribute and is excluded from the block policy.

16. Confirm that **Zava - Block High Risk Agent Identities** appears with a result of **Report-only: Not applied** — because no risk signal is present.

17. Close the detail panel.

---

### Task 3: Verify CA Policy Association from the Agent Identity Panel

1. In the left navigation pane, select **Entra agents**.

2. On the **All agent identities (Preview)** page, select **Zava HR Assistant (Microsoft Copilot Studio)**.

3. On the **Overview (Preview)** page, on the right panel under **Policies & ID Governance**, locate **CA policies**.

4. Select **View** next to **CA policies**.

5. On the **CA policies** page, confirm that both policies are listed:

   | Policy Name | State |
   |---|---|
   | Zava - Block Unapproved Agent Identities | On |
   | Zava - Block High Risk Agent Identities | Report-only |

6. Confirm that the policy evaluation result for **Zava - Block Unapproved Agent Identities** reflects **Not applied** for this agent identity due to the attribute exclusion.

---

## Summary

In this lab, you created a custom security attribute set named **AgentAttributes** with an **AgentApprovalStatus** attribute containing five predefined governance values. You assigned the appropriate approval status to all three Zava agent identities — HR_Approved, Finance_Approved, and IT_Approved — establishing a structured agent classification model in Entra ID. You created the **Zava - Block Unapproved Agent Identities** Conditional Access policy targeting all agent identities and excluding those with approved attribute values. You used the What If tool in Report-only mode to validate that an approved agent is correctly excluded from the block policy, and confirmed the scope of untagged agents. You switched the policy to enforcement mode. You created the **Zava - Block High Risk Agent Identities** policy using Entra ID Protection agent risk signals and set it to Report-only pending risk signal generation. Adele Vance invoked the Zava HR Assistant to generate sign-in events, which you then investigated in the Service principal sign-in logs filtered by agent type. You confirmed Conditional Access policy evaluation is recorded against agent authentication events, and verified the CA policy association from the Entra agent identity panel. Zava's agent identities are now governed by Zero Trust Conditional Access controls.
