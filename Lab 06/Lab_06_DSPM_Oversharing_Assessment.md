# Lab 06: DSPM (Preview) — Oversharing Assessment and Remediation

## Introduction

Microsoft Purview Data Security Posture Management (preview) is the unified front door for discovering, protecting, and investigating sensitive data risks across Zava's digital estate — including AI apps, agents, SharePoint sites, and user interactions. Unlike the classic DSPM for AI experience, the new DSPM (preview) combines traditional data security posture with AI observability into a single solution, organised around outcome-based security objectives.

In this lab, the data risk assessment scan is initiated at the very start of Day 3 before any other work begins, so results are available by the time learners reach Exercise 4. Signal generation exercises create realistic Copilot interaction events referencing sensitive Zava files. MOD Administrator and Patti Fernandes then use DSPM Objectives, one-click policies, assessment results, and the Activity Explorer to investigate and remediate oversharing risks across the Zava agent environment.

---

## Scenario

Zava's CISO has received a concern from the compliance team: the HR Assistant and Finance Agent may be surfacing sensitive employee and financial records to users who should not have access to that data. The security team needs to understand the full scope of data exposure, activate posture management policies, and apply remediation controls before the end of Day 3.

MOD Administrator will launch a custom data risk assessment against the Zava HR and Finance SharePoint sites, activate DSPM one-click policies, and use the Objectives dashboard to drive remediation. Adele Vance will generate realistic Copilot interaction signals referencing sensitive labelled files. Patti Fernandes will investigate the AI activities in DSPM Activity Explorer and review the oversharing findings from the assessment.

---

## Objectives

- Initiate a custom DSPM data risk assessment against Zava HR and Finance SharePoint sites at the start of Day 3.
- Generate realistic M365 Copilot interaction signals referencing sensitive labelled files as Adele Vance.
- Navigate the new DSPM (preview) experience and review the Posture dashboard.
- Activate DSPM one-click policies for risky AI usage detection and sensitive data protection.
- Review DSPM Objectives for oversharing and Copilot data exposure.
- Review data risk assessment results and apply remediation actions.
- Investigate Zava agent activity and sensitive data access in the Apps and agents dashboard.
- Review AI interaction events in Activity Explorer filtered to Adele Vance.
- Apply SharePoint Restricted Content Discovery to the Zava HR site.

---

## Lab Duration

Estimated time: **25 minutes**

---

> ⚠️ **IMPORTANT — Complete Task 1 of Exercise 1 before anything else on Day 3.**
> The data risk assessment scan can take 30–60 minutes to complete. It must be started first so results are available when you reach Exercise 4. Do not proceed to Exercise 2 until Task 1 of Exercise 1 is complete.

---

## Exercise 1: Initiate the Data Risk Assessment

### Task 00: Register and Entra app

2. Navigate to `https://entra.microsoft.com`. Sign in with **MOD Administrator** credentials if prompted.

3. In the left navigation pane, select **App registrations** > **+ New registration**.

	![](./media/image1.png)

5. Configure the following:
   - **Name:** `Purview DSPM Item Level Scan`
   - **Supported account types:** Select **Single tenant only - Contoso**

6. Select **Register**.

	![](./media/image2.png)

7. On the app registration overview page, copy and note the **Application (client) ID**.

	![](./media/image3.png)

8. In the left sub-navigation, select **API permissions**. Select **+ Add a permission**.

	![](./media/image4.png)

10. Select **Microsoft Graph**.

	![](./media/image5.png)

11. Select **Application permissions**.

	![](./media/image6.png)

12. Search for and add the following permissions:
    - `Application.Read.All`
    - `Directory.Read.All`
    - `Files.ReadWrite.All`
    - `SensitivityLabels.Read.All`
    - `Sites.ReadWrite.All`
    - `User.Read.All`
	- `SensitivityLabel.Read`

13. Select **Add permissions**.

	![](./media/image7.png)

14. Select **Grant admin consent for Contos**.

	![](./media/image8.png)

15. Select **Yes** to confirm.

	![](./media/image9.png)

19. Navigate to **Certificates & secrets** > **+ New client secret** > set expiry to **6 months** > **Add**.

	![](./media/image0.png)

20. Copy the **Value**.

	![](./media/image11.png)

21. Save the values as they can be only copied once and they will be needed in the upcoming lab 6.

---

### Task 1: Run a Custom Data Risk Assessment Against Zava SharePoint Sites

1. Open a browser and navigate to `https://purview.microsoft.com`. Sign in with **MOD Administrator** credentials if prompted.

3. In the left navigation pane, select **Solutions** > **DSPM (preview)**.

   > **Note:** Do not select **DSPM for AI (classic)** or **Data Security Posture Management (classic)**. The new experience is labelled **DSPM (preview)** and is a separate entry in the Solutions menu.

	![](./media/image12.png)

5. On the **DSPM (preview)** landing page, if prompted to complete initial setup tasks, select **Get started** and accept any required configuration to enable the solution. Allow the setup to complete before continuing.

6. In the left sub-navigation, select **Discover**. Under **Discover**, select **Data risk assessments**.

	![](./media/image13.png)

7. On the **Item-level scan not setup** notification select **Setup connection**.

	![](./media/image14.png)

8. On the Client Secret tab, enter the **Application ID** and **Client secret** value copied in Task 00. Then select **Authenticate**. Once successful, select **Save**.

	![](./media/image15.png)

8. On the **Data risk assessments** page, select **+ Create custom assessment**.

	![](./media/image16.png)

9. On the **Basic details** panel, configure the following:

   - **Assessment name:** Enter `Zava SharePoint Oversharing Assessment`.
   - **Description:** Enter `Custom assessment to identify potentially overshared sensitive items across Zava HR and Finance SharePoint sites.`

9. Select **Next**.

	![](./media/image17.png)

10. On the **Select scan level**, choose **Item-level**. 

	![](./media/image18.png)

10. Select next till you reach **Add data sources to assess**. Near **SharePoint**, select **Scope sites**.

	![](./media/image19.png)

11. In the SharePoint site selector, select **Include** > **From all sites**.

	![](./media/image20.png)

11. Search for and select the following two sites:

    - `HR`
    - `Operations`

12. Select **Done** twice to confirm the site selection.

	![](./media/image21.png)
	![](./media/image22.png)

14. Select **Next**.

	![](./media/image23.png)

14. Select **Save and Run**.

	![](./media/image24.png)

15. Select **Done**.

	![](./media/image25.png)

15. Confirm that the assessment appears in the **Data risk assessments** list with a status of **In progress** or **Queued**.

	![](./media/image26.png)

    > **Note:** The assessment will take 30–60 minutes to complete depending on the number of items in the selected SharePoint sites. Proceed immediately to Exercise 2. You will return to review the results in Exercise 4.

---

## Exercise 2: Generate Copilot Interaction Signals

In this exercise, Adele Vance generates realistic Microsoft 365 Copilot interaction events that reference sensitive labelled files across the Zava HR and Finance SharePoint sites. These interactions will surface in the DSPM Activity Explorer and Audit logs, creating the investigation data used in Exercises 5 and Lab 07.

### Task 1: Generate HR Data Interaction Signals as Adele Vance

1. Open a new **InPrivate** or **Incognito** browser window. Navigate to `https://copilot.microsoft.com`. Sign in with **Adele Vance** credentials from the **Resources** tab. Complete the authentication steps if necessary.

4. From the navigation select **All agants** > **Zava HR assistant** > **Add**.

	![](./media/image27.png)

4. In the input field, enter the following prompt:

   ```
   Summarise the contents of Zava_Employee_Records.xlsx from the HR SharePoint site
   ```

	![](./media/image28.png)

5. Wait for the response and note what Copilot returns.

6. Enter the following second prompt:

   ```
   Find all employee salary information across Zava HR documents
   ```

7. Wait for the response.

8. Enter the following third prompt:

   ```
   What does the Zava payroll report for Q1 2025 contain?
   ```

9. Wait for the response.

## Exercise 3: Explore the DSPM (Preview) Posture Dashboard and Activate One-Click Policies

### Task 1: Review the DSPM Posture Dashboard

1. Return to the **MOD Administrator** session in the Microsoft Purview portal at `https://purview.microsoft.com`.

2. In the left navigation pane, select **Solutions** > **DSPM (preview)**.

	![](./media/image29.png)

4. On the **DSPM (preview)** landing page, review the **Posture** dashboard.

	![](./media/image30.png)

5. Review the following sections and note their current values:

   - **Security Copilot suggested prompts** — note the prompt suggestions available.
   - **Top objectives to address** — note which objectives are listed as highest priority.
   - **Data use snapshot** — note the volume of sensitive data activity detected across the estate.
   - **30-day trending graph** — review whether the trend is improving or worsening.

---

### Task 2: Activate the Detect Risky AI Usage One-Click Policy

1. On the **DSPM (preview)** landing page, in the left sub-navigation, select **Tasks and actions**. Then select **Remediation actions**.

	![](./media/image31.png)

2. Select **Detect risky interactions in AI apps** to expand it.

	![](./media/image32.png)

5. Select **Create Policy** to enable the **DSPM for AI - Detect risky AI usage** Insider Risk Management policy.

6. Confirm that the policy status updates to **On** or **Active**. Close the tab.

	![](./media/image33.png)

   > **Note:** This Insider Risk Management policy detects risky prompts and responses in Microsoft 365 Copilot, agents, and other generative AI apps — including prompt injection attempts, accessing protected materials, and other high-risk interaction patterns. The Adele Vance interactions generated in Exercise 2 will be evaluated by this policy.

---

### Task 3: Activate the Sensitive Data Protection One-Click Policy

1. On the **Remediation actions** page, select **Safeguard sensitive data in Microsoft 365 Copilot interactions** to expand it.

	![](./media/image34.png)

3. Select **Get started** to enable this DLP policy.

	![](./media/image35.png)

5. On the data pane, select **view** near **Sensitive info types**.

	![](./media/image36.png)

6. Select **Credit Card Number**. Then select **Add**. 

	![](./media/image37.png)

6. Under actions select **Restrict user prompts from being processed**. Then select **Create policy**.

	![](./media/image38.png)

6. Return to the **Remediation actions** page and select **Safeguard sensitive data in Microsoft 365 Copilot interactions**. Then select **Get started**.

	![](./media/image39.png)

7. Select **Enforce policy** to enable the **Default DLP policy - Protect sensitive M365 Copilot interactions** policy.

	![](./media/image40.png)

8. Confirm the policy is active.

	![](./media/image41.png)

---

## Exercise 4: Review Data Risk Assessment Results and Apply Remediation
Assessments may take some time. You can come back to this excercise at the end of the labs.

### Task 1: Return to the Data Risk Assessment Results

1. In the left sub-navigation, select **Discover**. Select **Data risk assessments**.

3. On the **Data risk assessments** page, locate **Zava SharePoint Oversharing Assessment**.

4. Confirm the status shows **Completed**. If the status still shows **In progress**.

5. Select **Zava SharePoint Oversharing Assessment** to open the results.

---

### Task 2: Review Overshared Items

1. On the assessment results page, select the **Items** tab.

2. Review the list of potentially overshared items found across the Zava HR and Finance SharePoint sites.

3. Note the following for each item:

   - **File name**
   - **Sensitivity label** — confirm that HR-Data labelled files appear.
   - **Sharing scope** — note whether items are shared with **Everyone**, **All authenticated users**, or specific groups.
   - **Sensitive info types detected**

4. Locate **Zava_Employee_Records.xlsx** in the results and select it.

5. Review the item detail panel — note the sensitive info types detected, sharing permissions, and label applied.

6. Close the item detail panel.

---

### Task 3: Apply Remediation — Restrict Access by Label

1. On the assessment results page, select the **Protect** tab.

2. Locate the **Restrict access by label** remediation action.

3. Select **Restrict access by label**.

4. On the remediation panel, confirm that **Zava-Confidential/HR-Data** is listed as the label to restrict.

5. Review the action — this will create or reference a DLP policy that restricts access to items carrying the HR-Data label.

6. Select **Apply** or **Confirm** to activate the remediation.

7. Confirm that the remediation action status updates to **Applied**.

---

### Task 4: Apply Remediation — Enable SharePoint Restricted Content Discovery

1. On the **Protect** tab, locate the **Restrict all items** or **Enable Restricted Content Discovery** remediation action.

2. Select the action to open the configuration panel.

3. Review the description — SharePoint Restricted Content Discovery prevents items in the selected site from being surfaced in Microsoft 365 Copilot responses for users who do not have explicit access.

4. Confirm that the scope is set to the **Zava HR SharePoint site**.

5. Select **Apply** or **Enable** to activate Restricted Content Discovery for the Zava HR site.

6. Confirm that the action status updates to **Applied**.

   > **Note:** SharePoint Restricted Content Discovery is one of the most effective controls available to prevent AI agents and Copilot from surfacing content from a SharePoint site to users who lack explicit permission. This differs from DLP — it operates at the site discovery level rather than at the content classification level.

---

## Exercise 6: Investigate Agent Activity and AI Interactions

### Task 1: Review the Apps and Agents Dashboard

1. In the left sub-navigation, select **Discover**.

2. Select **Apps and agents**.

3. On the **Apps and agents** dashboard, review the list of AI apps detected across the tenant.

4. Locate **Copilot Studio** in the platform filter and apply it to show only Copilot Studio agents.

5. Confirm that the three Zava agents appear in the dashboard.

6. Select **Zava HR Assistant** to open its agent details.

7. On the agent details panel, review the following:

   - **Sensitive data accessed** — types and volume of sensitive content the agent has referenced.
   - **Policy coverage** — which Purview policies are protecting data accessed by this agent.
   - **Users** — which users have interacted with this agent.

8. Close the agent details panel.

---

### Task 2: Investigate AI Activities in Activity Explorer

6. In the left sub-navigation, select **Discover**.

7. Select **Activity explorer**.

8. On the **Activity explorer** page, select the **AI activities** tab.

9. In the filter bar, select **User** and enter `Adele Vance`.

10. Select **Apply** to filter results to Adele's interactions.

11. Review the interaction events listed in the filtered view.

12. Select an interaction event that references a sensitive file — for example, one referencing `Zava_Employee_Records.xlsx` or `Zava_Payroll_Q1_2025.xlsx`.

13. On the event detail panel, review the following fields:

    - **Date and time**
    - **User**
    - **Activity type**
    - **AI app**
    - **File referenced**
    - **Sensitivity label on file**
    - **DLP rule matched** — if applicable

14. Note whether the DLP policy **Zava - Block HR Data in M365 Copilot** appears as matched for any of the HR-labelled file interactions.

15. Close the event detail panel.

16. Remove the user filter and apply a filter for **Sensitivity label** set to **Zava-Confidential/HR-Data**.

17. Review the results — these show all AI interactions across the tenant that involved a file carrying the HR-Data label.

---

## Summary

In this lab, you initiated a custom DSPM data risk assessment against the Zava HR and Finance SharePoint sites at the start of Day 3, ensuring results were available for investigation later in the lab. As Adele Vance, you generated six realistic Microsoft 365 Copilot interaction events referencing sensitive labelled files including employee records, payroll data, financial projections, and expense reports — creating the AI activity signals needed for investigation throughout Day 3.

You explored the new DSPM (preview) Posture dashboard and reviewed its key metrics, top objectives, and Security Copilot suggested prompts. You activated three one-click policies: the DSPM for AI risky AI usage Insider Risk Management policy, the sensitive info detection DLP policy for Edge, and the Copilot interactions collection policy. You reviewed the Policies page to confirm all AI-related policies — including the manual Lab 04 DLP policy — are visible in a single governance view.

You reviewed the DSPM Objectives for Copilot data exposure and oversharing risk, noting recommended actions. You reviewed the data risk assessment results, identified overshared sensitive items in the Zava HR and Finance sites, and applied two remediation actions: restricting access by the HR-Data sensitivity label and enabling SharePoint Restricted Content Discovery on the Zava HR site. Finally, as Patti Fernandes, you investigated Adele Vance's Copilot interaction events in the DSPM Activity Explorer AI activities tab, reviewing file references, sensitivity labels, and DLP match records — building the evidence base for the Day 3 compliance review.
