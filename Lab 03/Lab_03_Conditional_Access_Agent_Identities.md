# Lab 03: Conditional Access for Zava Agent Identities

## Introduction

Zava's CISO has mandated that only reviewed and approved AI agents may access company resources. Any agent that has not been through the governance review process must be blocked automatically. Additionally, if any agent identity shows signs of compromise — such as anomalous token acquisition behaviour — it must be blocked immediately without manual intervention.

ODL User will implement both controls using Conditional Access for Agent Identities (Preview). Patti Fernandes will validate that policy evaluation is visible in sign-in logs. This lab establishes the Zava agent governance baseline that all subsequent security labs build upon.

Conditional Access for Agent Identities is a preview capability in Microsoft Entra ID that extends Zero Trust controls to AI agents. ODL User will create custom security attributes to classify the approval status of each Zava agent, build a Conditional Access policy that blocks all unapproved agent identities from accessing organisational resources, and create a second policy that blocks any agent identity exhibiting high-risk behaviour based on Entra ID Protection signals. The policies will first be validated in Report-only mode before being switched to enforcement. Patti Fernandes will investigate agent sign-in events to confirm Conditional Access policy evaluation.

---

## Objectives

- Create a custom security attribute set and approval status attribute for agent classification.
- Assign approval status attributes to all three Zava agent identities.
- Create a Conditional Access policy that blocks all unapproved agent identities.
- Validate the policy scope using the What If tool to confirm an untagged agent would be blocked.
- Switch the policy to enforcement mode.
- Create a second Conditional Access policy that blocks high-risk agent identities.
- Generate agent sign-in events by invoking the Zava HR Assistant as Patti Fernandes.
- Investigate Conditional Access policy evaluation in agent identity sign-in logs.

---

## Lab Duration

Estimated time: **30 minutes**

---

## Exercise 1: Create Custom Security Attributes for Agent Governance

### Task 1: Assign the Attribute Definition Administrator Role

1. Open a browser and navigate to `https://entra.microsoft.com`. Sign in with **ODL User** credentials if prompted. Under **Entra ID**, select **Roles & admins**.

	![](./media/l03-e1-t1-s1.png)

2. In the search bar, enter `Attribute Definition Administrator`.

	![](./media/l03-e1-t1-s2.png)
3. Select **Attribute Definition Administrator** by selecting its name.

	![](./media/l03-e1-t1-s3.png)

4. On the **Attribute Definition Administrator** page, select **+ Add assignments**.

5. On the **Add assignments** panel, select **ODL_User<inject key="Deployment ID" enableCopy="false"></inject>**. Click on **Add**.
   
11. Confirm the assignment appears in the list.

12. Navigate back to **Roles & admins**.

13. In the search bar, enter `Attribute Assignment Administrator` and repeat the steps to assign the role to **ODL_User<inject key="Deployment ID" enableCopy="false"></inject>**.

	![](./media/l03-e1-t1-s13.png)

---

### Task 2: Create the AgentAttributes Attribute Set

1. In the left navigation pane, expand **Entra ID** and select **Custom security attributes**. On the **Custom security attributes** page, select **+ Add attribute set**.

	![](./media/l03-e1-t2-s2.png)

3. On the **Add attribute set** panel, add the following and click on **Add**:
   - In the **Attribute set name** field, enter `AgentAttributes`.
   - In the **Description** field, enter `Attribute set for classifying AI agent approval and governance status`.
   - In the **Maximum number of attributes** field, leave the default value.

7. Confirm that **AgentAttributes** appears in the attribute set list.

	![](./media/l03-e1-t2-s7.png)

---

### Task 3: Create the AgentApprovalStatus Attribute

1. On the **Custom security attributes** page, select **AgentAttributes** to open the attribute set.

	![](./media/l03-e1-t3-s1.png)

2. On the **AgentAttributes** page, select **+ Add attribute**.

	![](./media/l03-e1-t3-s2.png)

3. On the **Add attribute** panel, configure the following fields:

   - **Attribute name:** Enter `AgentApprovalStatus`.
   - **Description:** Enter `Tracks the approval status of each AI agent identity in the Zava governance review process.`
   - **Data type:** Select **String**.
   - **Allow multiple values to be assigned:** Select **Yes**.
   - **Only allow predefined values to be assigned:** Select **Yes**.

4. Under **Predefined values**, select **+ Add value**.

	![](./media/l03-e1-t3-s4.png)

5. In the value field, enter `New`. Then, select **Add**.

	![](./media/l03-e1-t3-s5.png)

6. Select **+ Add value**.

	![](./media/l03-e1-t3-s6.png)

7. In the value field, enter `In_Review`. Then select **Add**.

	![](./media/l03-e1-t3-s7.png)

8. Select **+ Add value**.

	![](./media/l03-e1-t3-s8.png)

9. In the value field, enter `HR_Approved`. Then select **Add**.

	![](./media/l03-e1-t3-s9.png)

10. Select **+ Add value**.

	![](./media/l03-e1-t3-s10.png)

11. In the value field, enter `Finance_Approved`. Then select **Add**.

	![](./media/l03-e1-t3-s11.png)

12. Select **+ Add value**.

	![](./media/l03-e1-t3-s12.png)

13. In the value field, enter `IT_Approved`. Then select **Add**.

	![](./media/l03-e1-t3-s13.png)
14. Select **Save**.

	![](./media/l03-e1-t3-s14.png)

15. Confirm that **AgentApprovalStatus** appears in the attributes list under **AgentAttributes**.

	![](./media/l03-e1-t3-s15.png)

---

### Task 4: Assign HR_Approved to the Zava HR Assistant

1. In the left navigation pane, select **Agents**.

	![](./media/l03-e1-t4-s1.png)

2. On the **Agent identities** page, select **Zava HR Assistant (Microsoft Copilot Studio)**.

	![](./media/l03-e1-t4-s2.png)

3. On the **Overview** page, in the left sub-navigation, select **Custom security attributes**.

	![](./media/l03-e1-t4-s3.png)

4. On the **Custom security attributes** page, select **+ Add assignment**.

	![](./media/l03-e1-t4-s4.png)

5. On the **Add custom security attribute assignment** panel, configure the following:

   - **Attribute set:** Select **AgentAttributes**.
   - **Attribute name:** Select **AgentApprovalStatus**.
   - **Assigned values:** Select **Add value** > **HR_Approved** and select **Save**.

		![](./media/l03-e1-t4-s5.png)

		![](./media/l03-e1-t4-s6.png)

6. Select **Save** to apply the assignment.

	![](./media/l03-e1-t4-s6.1.png)

7. Confirm that **AgentApprovalStatus** appears with the value **HR_Approved** on the custom security attributes page.

8. Similarly assign the following attributes to respective agents.

   - **Zava Finance Agent (Microsoft Copilot Studio)**: New
   - **Zava IT Support Agent (Microsoft Copilot Studio)**: New
---

## Exercise 2: Create a Conditional Access Policy to Block Unapproved Agent Identities

### Task 1: Create the Policy and Configure Assignments

1. In the left navigation pane of the Microsoft Entra admin center, expand **Entra ID**, then select **Conditional Access**.

2. On the **Conditional Access** page, select **Policies**.

	![](./media/l03-e2-t1-s2.png)

3. On the **Policies** page, select **+ New policy**.

	![](./media/l03-e2-t1-s3.png)

4. On the **New Conditional Access policy** page, in the **Name** field, enter `Zava - Block Unapproved Agent Identities`.

5. Under **Assignments**, select **0 users or agents (Preview) selected** under **Users or agents**.

6. On the assignments panel, under **What does this policy apply to?**, select **Agents**.

7. Under **Include**, select **All agent identities (Preview)**.

	![](./media/l03-e2-t1-s7.png)

8. Under **Exclude**, click on **None** under **Select agent identities based on attributes**.

	![](./media/l03-e2-t1-s8.png)

9. Set **Configure** to **Yes**.

	![](./media/l03-e2-t1-s9.png)

10. In the expression configuration, under **Attribute**, select the attribute **AgentApprovalStatus**. Set **Operator** to **Contains**. Set **Value** to **HR_Approved**.Select **Done** to confirm the exclusion configuration.

	![](./media/l03-e2-t1-s11.png)

12. Under **Target resources**, select **No target resources selected**.

	![](./media/l03-e2-t1-s12.png)

13. Under **Include**, select **All resources (formerly 'All cloud apps')**.

	![](./media/l03-e2-t1-s13.png)

15. Under **Access controls**, on the **Grant** panel, confirm that **Block access** is selected.

16. For **Enable policy**, keep **Report-only**.

17. Select **Create** to save the policy.

	![](./media/l03-e2-t1-s15.png)
---

### Task 2: Validate the Policy Using the What If Tool

1. On the policy page, select **What If** to open the Report-only impact view.

   > **Note:** The What If tool allows you to simulate whether a specific identity would be affected by this policy without enforcing it.

	![](./media/l03-e2-t2-s1.png)

2. On the **What If** panel, under **Select identity type**, select **Agent identities (Preview)**.

	![](./media/l03-e2-t2-s2.png)

2. Select **Edit agent identity**.

	![](./media/l03-e2-t2-s3.png)

3. In the agent identity search field, search for and select **Zava Finance Agent (Microsoft Copilot Studio)**.

	![](./media/l03-e2-t2-s4.png)

4. Under **Target resource**, set **Select target type** to **Cloud apps**. Select **+ Select cloud app**.

5. In the search field, enter `Office 365 SharePoint Online`. Select **Office 365 SharePoint Online** from the results. Choose **Select** to confirm.

	![](./media/l03-e2-t2-s6.png)

6. Select **What if** to run the simulation.

	![](./media/l03-e2-t2-s7.png)

7. Review the results and confirm that the policy **Zava - Block Unapproved Agent Identities** shows as **Applied** — because the Zava Finance Agent is NOT excluded by the `HR_Approved` attribute.

	![](./media/l03-e2-t2-s8.png)

8. Return to the **Edit agent identity** link and change the agent to **Zava HR Assistant**.

	![](./media/l03-e2-t2-s9.png)

	![](./media/l03-e2-t2-s9.1.png)

8. Select **What if** to run the simulation.

	![](./media/l03-e2-t2-s10.png)

9. Review the results and confirm that the policy **Zava - Block Unapproved Agent Identities** shows as **Not applied** — because the Zava HR Assistant is excluded by the `HR_Approved` attribute.

	![](./media/l03-e2-t2-s11.png)

10. Select **Close** to exit the What If panel.

---

### Task 3: Switch the Policy to Enforcement Mode 

1. Navigate back to **Policies** and click on **Zava - Block Unapproved Agent Identities**

2. Under **Enable policy**, select **On**.

3. Select **Save** to apply the change.

4. On the **Policies** page, confirm that **Zava - Block Unapproved Agent Identities** shows a status of **On**.

	![](./media/l03-e2-t3-s1.png)

	>**Note:** If you receive the error “Security defaults must be disabled to enable Conditional Access policy”, click on Disable security defaults and turn off the Security Defaults option. Once disabled, proceed with enabling the Conditional Access policy.
	![](./media/l03-e2-t3-s4.png)
---

## Exercise 3: Create a Conditional Access Policy to Block High-Risk Agent Identities

### Task 1: Create the Policy and Configure Assignments

1. On the **Conditional Access** page, select **+ New policy**.

2. In the **Name** field, enter `Zava - Block High Risk Agent Identities`.

3. Under **Assignments**, select **0 users or agents (Preview) selected** under **Users or agents**.

	![](./media/l03-e3-t3-s2.png)

4. Under **What does this policy apply to?**, select **Agents**.

5. Under **Include**, select **All agent identities (Preview)**.

6. Under **Target resources**, select **No target resources selected**, then select **All resources (formerly 'All cloud apps')**.

	![](./media/l03-e3-t3-s6.png)

7. Under **Conditions**, select **0 Conditions selected**. Then select **Not Configured** under **Agent Risk**.

	![](./media/l03-e3-t3-s7.png)

8. On the **Agent risk** panel, set **Configure** to **Yes**. Under **Configure agent risk levels needed for policy to be enforced**, select **High**. Select **Done** to confirm the condition.

	![](./media/l03-e3-t3-s8.png)

9. Under **Access controls**, under **Grant**, make sure that **Block access** is selected.

10. Under **Enable policy**, select **Report-only**.

    > **Note:** This policy is set to Report-only because agent risk signals from Entra ID Protection require active agent usage over time before risk levels are generated. In a newly provisioned lab environment, no risk signals will be present yet. Report-only mode allows the policy to be evaluated against future sign-in events without blocking access prematurely. In a production environment, this policy would be switched to On once baseline risk signal data is established.

11. Select **Create** to save the policy.

	![](./media/l03-e3-t3-s10.png)

12. On the **Policies** page, confirm that **Zava - Block High Risk Agent Identities** appears with a status of **Report-only**.

---

## Exercise 4: Generate Agent Sign-In Events and Investigate Conditional Access Policy Evaluation

### Task 1: Invoke the Zava HR Assistant

1. Open a new **InPrivate** or **Incognito** browser window.

2. Navigate to `https://copilot.microsoft.com`.

3. Sign in with **Patti Fernandes** credentials from the **Resources** tab. (You can use **<inject key="User 02 UPN"></inject>** as your ID and the User Password from the Resources tab.)

4. In the Microsoft 365 Copilot chat interface, select **All agents** from the navigation. Search for and select **Zava HR Assistant**.

	![](./media/l03-e4-t1-s4.png)

5. Then select **Add**.

	![](./media/l03-e4-t1-s5.png)

6. In the chat input field, enter the following:

   ```
   What is Zava's leave policy?
   ```

7. Wait for the Zava HR Assistant to respond.

	![](./media/l03-e4-t1-s7.png)
8. Enter a second message in the chat input field:

   ```
   How do I submit a sick leave request?
   ```

9. Wait for the response.

	![](./media/l03-e4-t1-s8.png)

   > **Note:** These interactions generate agent sign-in events as the Zava HR Assistant authenticates to access its SharePoint knowledge source. These events will appear in the Entra sign-in logs and will have Conditional Access policy evaluation recorded against them.

10. Close the InPrivate browser window.

---

### Task 2: Investigate Agent Sign-In Logs in Entra

1. Return to the **ODL User** browser session at `https://entra.microsoft.com`.

2. In the left navigation pane, expand **Entra ID** and select **Monitoring & health**. Select **Sign-in logs**.

	![](./media/l03-e4-t2-s2.png)

3. On the **Sign-in logs** page, select the **Service principal sign-ins** tab.

	![](./media/l03-e4-t2-s3.png)

4. In the filter bar, select **+ Add filters**. Select **Is Agent** as the filter field.

	![](./media/l03-e4-t2-s4.png)

5.  Select **Yes** and then select **Apply** to apply the filter.

	![](./media/l03-e4-t2-s5.png)

6. Review the sign-in entries returned in the filtered view.

	![](./media/l03-e4-t2-s6.png)

---

## Summary

In this lab, you created a custom security attribute set named **AgentAttributes** with an **AgentApprovalStatus** attribute containing five predefined governance values. You assigned the **HR_Approved** approval status to the Zava HR Assistant, establishing a structured agent classification model in Entra ID. You created the **Zava - Block Unapproved Agent Identities** Conditional Access policy targeting all agent identities and excluding those with approved attribute values. You used the What If tool in Report-only mode to validate that an approved agent is correctly excluded from the block policy, then switched the policy to enforcement mode. You created the **Zava - Block High Risk Agent Identities** policy using Entra ID Protection agent risk signals and set it to Report-only pending risk signal generation. Patti Fernandes invoked the Zava HR Assistant to generate sign-in events, which you then investigated in the Service principal sign-in logs filtered by agent type. Zava's agent identities are now governed by Zero Trust Conditional Access controls.
