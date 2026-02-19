
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

2.  Click on the **Visual Studio Code** from the LabVM desktop.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2013/media/image1.png)

3.  Select **File**  and click **Open Folder**  to open
    the **OpenAIWorkshop-Framework** folder.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2013/media/image2.png)

4.  Navigate to **C:\Labfiles\Day 3\\OpenAIWorkshop-Framework** path,
    select **OpenAIWorkshop-Framework**  and then **Select Folder**.

5.  Select **Yes, I trust the authors**.

    ![A screenshot of a computer screen AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2013/media/image3.png)

6.  Click on the **ellipsis(...)**  then **Terminal**  and
    then **New Terminal** .

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2013/media/image4.png)

7.  Enter the below command to navigate to
    the **applications** directory and install all required dependencies
    from the **pyproject.toml / uv.lock** file.

	+++cd agentic_ai/applications+++

	+++pip install uv+++

	+++uv sync+++

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2013/media/im1.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2013/media/im2.png)

9.  The command may take 5–10 minutes to complete. **Meanwhile, you can
    proceed with Task 1**.

## Task 1: Implementing Human-in-the-Loop AI Workflows with Azure Agent Framework

In this lab, you will implement a Human-in-the-Loop (HITL) workflow for
Contoso’s Fraud Detection system. You’ll run multi-agent fraud
detection, review high-risk alerts, make human decisions, and visualize
the workflow in real time using the React + FastAPI dashboard.

1.  From the Visual Studio Code, expand **agentic_ai ** \> workflow
    **\> fraud_detection**, select **fraud_detection_workflow.py**. View the Code .

    ![A screenshot of a computer screen AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2013/media/image6.png)

2.  Under **fraud_detection** , right click
    on **.env.sample**  and then select **Rename** .

    ![A screenshot of a computer program AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2013/media/image7.png)

3.  Rename as .env and click on it to open the file.

    ![A screenshot of a computer program AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2013/media/image8.png)

4.  Replace the value
    of AZURE_OPENAI_API_KEY  and AZURE_OPENAI_ENDPOINT  with
    the actual values that you have copied in the previous lab.

5.  Add the AZURE_OPENAI_CHAT_DEPLOYMENT as **gpt-4o-mini**

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2013/media/image9.png)

	- Navigate to **Microsoft Foundry** portal, select **Overview** ,
	  select **Azure OpenAI** . Copy the **Azure OpenAI
	  key**  and **Azure OpenAI endpoint** .

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2013/media/image10.png)

6.  Select **File**  and then **Save** .

    ![A screenshot of a computer menu AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2013/media/image11.png)

7.  In the Visual Studio Code Window, click on
    the **ellipsis(...)**  then **Terminal**  and
    then **New Terminal** .

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2013/media/image4.png)

8.  Run the below command.

	```
	cd mcp
	uv run python mcp_service.py
	```

9.  Let the command run, open a new terminal.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2013/media/image4.png)

10. Enter the command given below to run the Workflow with the command
    line.

	```
	cd agentic_ai/workflow/fraud_detection
	uv run python fraud_detection_workflow.py
	```

    ![A black screen with white text AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2013/media/image12.png)

    >[!Note]: The command may take 5–10 minutes to complete. Please wait
until it finishes.

11. The example includes three sample alerts:

    - **Alert 1: Multi-Country Login** (High Severity)

    - alert_id: "ALERT-001"

    - customer_id: 1

    - alert_type: "multi_country_login"

    - description: "Login attempts from USA and Russia within 2 hours."

	severity: "high"

	- **Alert 2: Data Spike** (Medium Severity)

	- alert_id: "ALERT-002"

	- customer_id: 2

	- alert_type: "data_spike"

	- description: "Data usage increased by 500% in the last 24 hours."

	severity: "medium"

	- **Alert 3: Unusual Charges** (High Severity)

	- alert_id: "ALERT-003"

	- customer_id: 3

	- alert_type: "unusual_charges"

	- description: "Three large purchases totaling $5,000 in 10 minutes."

	severity: "high"

12. Once the run succeeded, you can see the terminal as below. Select
    the action based on the risk severity. If the risk
    severity ≥0.6 human review is needed.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2013/media/image13.png)

13. As the risk severity is high, you can enter 2 to lock the customer
    account 

    - Enter analyst notes: High risk confirmed from all three analyses.
      Immediate action: locking account to prevent unauthorized
      access. 

    - Enter analyst ID (default: analyst_cli): Press **Enter** 

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2013/media/image14.png)

14. Once the workflow is completed, you will receive an output like
    this.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2013/media/image15.png)

15. Once the command is succeeded, **delete all the existing running
    terminal sessions**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2013/media/image16.png)

## Real-Time Workflow Visualizer UI for Contoso Fraud Detection & Response Workflow

You will use the Real-Time Workflow Visualizer UI to monitor and
interact with the Contoso Fraud Detection & Response Workflow. You’ll
start all services (MCP server, backend, frontend), select sample
alerts, observe live workflow execution, review high-risk fraud alerts,
submit analyst decisions, and monitor event streams in real time.

1.  Open a new terminal.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2013/media/image4.png)

2.  Start All Services (3 terminals):

    - Terminal 1 - MCP Server:

	```
	cd mcp
	uv run mcp_service.py 
	```

	- Terminal 2 - FastAPI Backend:

	```
	cd agentic_ai/workflow/fraud_detection
	uv run --prerelease allow backend.py
	```

    ![A screen shot of a computer program AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2013/media/image17.png)

	- Terminal 3 - React Frontend:

	```
	cd agentic_ai/workflow/fraud_detection/ui
 	npm install
	npm run dev
	```

	>[!Note]: If you get any error, execute the +++npm install+++ command and then rerun the +++npm run dev+++ command.

    ![A computer screen with white text AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2013/media/image18.png)

	- **ctrl + click** on http://localhost:3000 to open the application in a
	  browser

    ![A screen shot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2013/media/image19.png)

3.  View the Real-Time Workflow Visualizer UI.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2013/media/image20.png)

4.  You can see sample alerts from the **Select Alerts** drop-down.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2013/media/image21.png)

    >[!Note]: You will be able to see the alerts from the drop-down only
	after the connection is open in the 2nd terminal (backend.py). Make sure
	the connection is open.

5.  **Select Alert**: Choose from 3 sample alerts (ALERT-001, ALERT-002,
    ALERT-003) 

    - Cick on **Start Workflow** to begin processing

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2013/media/image22.png)

6.  **Watch Live Updates**: Nodes change color as executors run

    - 🔵 Blue = Running

    - 🟢 Green = Completed

    - ⚪ Gray = Idle

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2013/media/image23.png)

7.  **Analyst Review**: When high-risk fraud is detected, a review panel
    appears.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2013/media/image24.png)

8.  **Submit Decision**: Choose action and add notes

    - Your Decision: If the severity is high, select **Lock Account**

    - Analyst notes: Enter High risk confirmed from all three analyses.
      Immediate action: locking account to prevent unauthorized
      access. 

    - Select **SUBMIT WORKFLOW**

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2013/media/image25.png)

9.  **Monitor Events**: The right panel shows the complete event stream.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2013/media/image26.png)

**Summary**

In this lab, you implemented a human-in-the-loop (HITL) workflow for
fraud detection using the Azure Agent Framework. You explored how AI
agents analyze suspicious activity, route high-risk cases to human
analysts, and interact with a real-time React + FastAPI dashboard to
monitor workflow execution and submit decisions.




