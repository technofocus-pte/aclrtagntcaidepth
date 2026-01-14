# Lab 13: Implementing Enterprise Fraud Detection with Human-in-the-Loop AI

**Estimated Duration**: 60 Minutes

**Overview**

You are an AI Engineer at Contoso Ltd., responsible for implementing
human-in-the-loop (HITL) AI workflows. In this lab, you will explore the
Contoso Fraud Detection & Response Workflow, where AI agents analyze
suspicious activity and route high-risk actions to human analysts for
review, using a real-time React + FastAPI dashboard for monitoring and
interaction.

Lab Objective

You'll perform the following tasks in this lab.

- Task 1: Implementing Human-in-the-Loop AI Workflows with Azure Agent
  Framework

## Task 0: Set up the code 

1.  From C:\Labfiles\Day 3, extract the **OpenAIWorkshop-Framework**
    file.

2.  Click on the **Visual Studio Code** from the LabVM desktop.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

3.  Select **File** **(1)** and click **Open Folder** **(2)** to open
    the **OpenAIWorkshop-Framework** folder.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

4.  Navigate to C:\Labfiles\Day 3\\**OpenAIWorkshop-Framework** path,
    select **OpenAIWorkshop-Framework**  and then **Select Folder**.

5.  Select **Yes, I trust the authors**.

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image3.png)

6.  Click on the **ellipsis(...)** **(1)** then **Terminal** **(2)** and
    then **New Terminal** **(3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

7.  Enter the below command to navigate to
    the **applications** directory and install all required dependencies
    from the **pyproject.toml / uv.lock** file.

> cd agentic_ai/applications
>
> uv sync

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

**Note:** If you face any error, please run the below given commands

> +++pip install uv+++

+++uv sync+++

8.  The command may take 5–10 minutes to complete. **Meanwhile, you can
    proceed with Task 1**.

## Task 1: Implementing Human-in-the-Loop AI Workflows with Azure Agent Framework

In this lab, you will implement a Human-in-the-Loop (HITL) workflow for
Contoso’s Fraud Detection system. You’ll run multi-agent fraud
detection, review high-risk alerts, make human decisions, and visualize
the workflow in real time using the React + FastAPI dashboard.

1.  From the Visual Studio Code, expand **agentic_ai (1) \> workflow
    (2)\> fraud_detection (3)**, select **fraud_detection_workflow.py
    (4)**. View the Code **(5)**.

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image6.png)

2.  Under **fraud_detection** **(1)**, right click
    on **.env.sample** **(2)** and then select **Rename** **(3)**.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image7.png)

3.  Rename as .env and click on it to open the file.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image8.png)

4.  Replace the value
    of AZURE_OPENAI_API_KEY **(1)** and AZURE_OPENAI_ENDPOINT **(2)** with
    the actual values that you have copied in the previous lab.

5.  Add the AZURE_OPENAI_CHAT_DEPLOYMENT as **gpt-4o-mini (3)**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.png)

- Navigate to **Microsoft Foundry** portal, select **Overview** **(1)**,
  select **Azure OpenAI** **(2)**. Copy the **Azure OpenAI
  key** **(3)** and **Azure OpenAI endpoint** **(4)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.png)

6.  Select **File** **(1)** and then **Save** **(2)**.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

7.  In the Visual Studio Code Window, click on
    the **ellipsis(...)** **(1)** then **Terminal** **(2)** and
    then **New Terminal** **(3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

8.  Run the below command.

> cd mcp
>
> uv run python mcp_service.py

9.  Let the command run, open a new terminal.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

10. Enter the command given below to run the Workflow with the command
    line.

> cd agentic_ai/workflow/fraud_detection
>
> uv run python fraud_detection_workflow.py
>
> ![A black screen with white text AI-generated content may be
> incorrect.](./media/image12.png)

**Note**: The command may take 5–10 minutes to complete. Please wait
until it finishes.

11. The example includes three sample alerts:

    - **Alert 1: Multi-Country Login** (High Severity)

    - alert_id: "ALERT-001"

    - customer_id: 1

    - alert_type: "multi_country_login"

    - description: "Login attempts from USA and Russia within 2 hours."

severity: "high"

- **Alert 2: Data Spike** (Medium Severity)

- alert_id: "ALERT-002"

- customer_id: 2

- alert_type: "data_spike"

- description: "Data usage increased by 500% in the last 24 hours."

severity: "medium"

- **Alert 3: Unusual Charges** (High Severity)

- alert_id: "ALERT-003"

- customer_id: 3

- alert_type: "unusual_charges"

- description: "Three large purchases totaling $5,000 in 10 minutes."

severity: "high"

12. Once the run succeeded, you can see the terminal as below. Select
    the action based on the risk severity. If the risk
    severity ≥0.6 human review is needed.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

13. As the risk severity is high, you can enter 2 to lock the customer
    account **(1)**

    - Enter analyst notes: High risk confirmed from all three analyses.
      Immediate action: locking account to prevent unauthorized
      access. **(2)**

    - Enter analyst ID (default: analyst_cli): Press **Enter** **(3)**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

14. Once the workflow is completed, you will receive an output like
    this.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.png)

15. Once the command is succeeded, **delete all the existing running
    terminal sessions**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image16.png)

## Real-Time Workflow Visualizer UI for Contoso Fraud Detection & Response Workflow

You will use the Real-Time Workflow Visualizer UI to monitor and
interact with the Contoso Fraud Detection & Response Workflow. You’ll
start all services (MCP server, backend, frontend), select sample
alerts, observe live workflow execution, review high-risk fraud alerts,
submit analyst decisions, and monitor event streams in real time.

1.  Open a new terminal.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

2.  Start All Services (3 terminals):

    - Terminal 1 - MCP Server:

> cd mcp
>
> uv run mcp_service.py

- Terminal 2 - FastAPI Backend:

> cd agentic_ai/workflow/fraud_detection
>
> uv run --prerelease allow backend.py
>
> ![A screen shot of a computer program AI-generated content may be
> incorrect.](./media/image17.png)

- Terminal 3 - React Frontend:

> cd agentic_ai/workflow/fraud_detection/ui
>
> npm run dev
>
> **Note**: If you get any error, execute the +++npm install+++ command
> and then rerun the +++npm run dev+++ command.
>
> ![A computer screen with white text AI-generated content may be
> incorrect.](./media/image18.png)

- **ctrl + click** on http://localhost:3000 to open the application in a
  browser

> ![A screen shot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

3.  View the Real-Time Workflow Visualizer UI.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

4.  You can see sample alerts from the **Select Alerts** drop-down.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image21.png)

**Note**: You will be able to see the alerts from the drop-down only
after the connection is open in the 2nd terminal (backend.py). Make sure
the connection is open.

5.  **Select Alert**: Choose from 3 sample alerts (ALERT-001, ALERT-002,
    ALERT-003) **(1)**

    - Cick on **Start Workflow (2)** to begin processing

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

6.  **Watch Live Updates**: Nodes change color as executors run

    - 🔵 Blue = Running

    - 🟢 Green = Completed

    - ⚪ Gray = Idle

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

7.  **Analyst Review**: When high-risk fraud is detected, a review panel
    appears.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

8.  **Submit Decision**: Choose action and add notes

    - Your Decision: If the severity is high, select **Lock Account
      (1)**

    - Analyst notes: Enter High risk confirmed from all three analyses.
      Immediate action: locking account to prevent unauthorized
      access. **(2)**

    - Select **SUBMIT WORKFLOW (3)**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

9.  **Monitor Events**: The right panel shows the complete event stream.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

**Summary**

In this lab, you implemented a human-in-the-loop (HITL) workflow for
fraud detection using the Azure Agent Framework. You explored how AI
agents analyze suspicious activity, route high-risk cases to human
analysts, and interact with a real-time React + FastAPI dashboard to
monitor workflow execution and submit decisions.
