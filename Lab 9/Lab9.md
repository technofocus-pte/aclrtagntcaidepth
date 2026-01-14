# Lab 9: Implementing Single and Multi-Agent Workflows using Azure AI Framework

**Estimated Duration**: 45 Minutes

**Overview**

You are an AI Engineer at Contoso Ltd., tasked with developing
intelligent agent workflows using the Azure AI Framework. In this lab,
you will create a single-agent system that integrates with external
tools using MCP, and then design multi-agent workflows where multiple
specialized agents collaborate or hand off tasks dynamically based on
user intent.

Lab Objectives

You'll perform the following tasks in this lab.

- Task 1: Build and Test an Azure OpenAI Chat Agent

- Task 2: Creating a Single-Agent Workflow with Tool Integration

- Task 3: Multi-Agent Workflow Design

  - Task 3.1: Orchestrating Multi-Agent Workflows

  - Task 3.2: Handoff Pattern Multi-Agent System

## Task 0: Lab environment setup

1.  From C:\Labfiles\Day 2, extract the **OpenAIWorkshop-Framework**
    file.

2.  Click on the **Visual Studio Code** from the LabVM desktop.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

3.  Select **File** **(1)** and click **Open Folder** **(2)** to open
    the **OpenAIWorkshop-Framework** folder.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

4.  Navigate to C:\Labfiles\Day 2\\**OpenAIWorkshop-Framework** path,
    select **OpenAIWorkshop-Framework**  and then **Select Folder**.

5.  Select **Yes, I trust the authors**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

6.  Click on the **ellipsis(...)** **(1)** then **Terminal** **(2)** and
    then **New Terminal** **(3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

7.  Enter the below command to navigate to
    the **applications** directory and install all required dependencies
    from the **requirements.txt** file.

> cd agentic_ai/applications
>
> pip install -r requirements.txt

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image5.png)

8.  The command may take 5–10 minutes to complete.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image6.png)

## Task 1: Build and Test an Azure OpenAI Chat Agent

In this task, you will build and test a simple Azure OpenAI chat agent
in Visual Studio Code. You’ll configure environment variables, connect
the agent to your deployed model, and observe how it generates dynamic
responses based on different prompts.

1.  Navigate back to the **Visual Studio Code**.

2.  Make sure the pip install -r requirements.txt command has completed
    successfully. If it’s still running, please wait until it finishes.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image6.png)

3.  From the **Explorer**, expand **agentic_ai** **(1)
    \>** **applications** **(2)**. Right click
    on .env.sample **(3)** and the **Rename** **(4)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

4.  Rename the file to .env and click on it to open the file.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

5.  Replace the value
    of AZURE_OPENAI_API_KEY **(1)** and AZURE_OPENAI_ENDPOINT **(2)** with
    the actual values. Fetch them from Microsoft Foundry **Overview**
    page.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image9.png)

6.  Add the AZURE_OPENAI_CHAT_DEPLOYMENT as **gpt-4o-mini (3)**

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image10.png)

7.  Select **File (1)** and then **Save(2)**.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

8.  Right-click on **application** **(1)** folder and then **New
    file** **(2)** to create a new file to configure a simple agent.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.png)

9.  Name the agent file as +++simple_agent_test.py+++.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

10. Copy and paste the following code into the file.

> import asyncio
>
> import os
>
> from dotenv import load_dotenv
>
> from agent_framework.azure import AzureOpenAIChatClient
>
> from azure.identity import AzureCliCredential
>
> \# Load .env file (same folder or specify full path)
>
> load_dotenv(dotenv_path=".env")
>
> \# Retrieve values from .env
>
> endpoint = os.getenv("AZURE_OPENAI_ENDPOINT")
>
> deployment_name = os.getenv("AZURE_OPENAI_CHAT_DEPLOYMENT")
>
> api_version = os.getenv("AZURE_OPENAI_API_VERSION")
>
> print("Using Azure OpenAI endpoint:", endpoint)
>
> print("Deployment name:", deployment_name)
>
> print("API version:", api_version)
>
> \# ✅ Correct parameter name is deployment_name (not deployment)
>
> agent = AzureOpenAIChatClient(
>
> api_key=os.getenv("AZURE_OPENAI_API_KEY"),
>
> endpoint=endpoint,
>
> deployment_name=deployment_name,
>
> api_version=api_version
>
> ).create_agent(
>
> instructions="You are a helpful and funny assistant who tells short
> jokes.",
>
> name="Joker"
>
> )
>
> async def main():
>
> result = await agent.run("Tell me a joke about the cloud.")
>
> print("\nAgent response:\n", result.text)
>
> asyncio.run(main())

![A computer screen shot of a program AI-generated content may be
incorrect.](./media/image14.png)

11. Select **File** **(1)** and then **Save** **(2)**.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

12. Right click on simple_agent_test.py **(1)** and then select **Open
    in Integrated Terminal** **(2)**.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image15.png)

13. Run the command given below to run the agent and observe the output
    to understand how the agent worked.

+++python simple_agent_test.py+++

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image16.png)

14. Let’s modify the instruction to observe how the agent responds.
    Provide the instruction as Tell me a joke about the
    Earth **(1)** (line no. 31), then **Save** the file. Then run the
    command below **(2)**, and review the agent’s response **(3)**.

+++python simple_agent_test.py+++

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image17.png)

15. This demonstrates how the agent’s response varies based on the
    instruction provided, highlighting its ability to adapt to different
    prompts.

## Task 2: Creating a Single-Agent Workflow with Tool Integration

In this task, you’ll build and test a single-agent workflow that
integrates with external tools using the MCP (Model Context Protocol).
You’ll configure environment variables, run the MCP server, backend, and
frontend locally, and observe how the agent leverages MCP tools to
process user queries and deliver intelligent, context-aware responses.

1.  On the Visual Studio Code, expand **agents** **(1)
    \>** **agent_framework** **(2) \>** **single_agent** **(3)** and
    view the Single-Agent Workflow with MCPStreamableHTTPTool tool
    integrated **(4)**.

    - MCPStreamableHTTPTool allows the agent to invoke external
      HTTP-based services via the MCP server and include tool outputs in
      the conversation.

    - Passed to ChatAgent and used automatically based on instructions
      and user prompts

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image18.png)

2.  Go through the code to understand how it is integrated:

    - In the \_maybe_create_tools method:

> ![A screen shot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

- This creates a streamable HTTP tool connected to your MCP server.

- It allows the agent to make HTTP calls to external services (through
  MCP) as part of its workflow.

&nbsp;

- The tool is passed to the ChatAgent during initialization:

> ![A screen shot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

- The agent can then use this tool whenever a user prompt triggers a
  tool call.

- Streaming support with WebSocket: When a tool/function is called
  during a streamed conversation, it broadcasts the tool name and turn
  via \_chat_async_streaming.

3.  Navigate to the .env file **(1)**, add the following Environment
    variable to your .env file to specify **Single agent workflow** to
    run **(2)**:

+++AGENT_MODULE=agents.agent_framework.single_agent+++

- Add DISABLE_AUTH=true **(3)** environment variable, it is used to
  disable authentication in the application. It allows easier local
  development and testing.

> +++DISABLE_AUTH=true+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

4.  Select **File (1)** and then **Save(2)**.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

5.  Now you will start the **MCP server, backend,** and **React
    frontend** to run the full agent environment locally, allowing the
    UI to interact with agents and tools.

6.  In the Visual Studio Code Window, click on the **ellipses (...)
    (1)**, then **Terminal (2)**, and then **New Terminal (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

7.  Wait for completion of the previous step, and proceed to the next
    step.

8.  **Start the MCP Server (Terminal 1)**: (mcp directory is at project
    root level)

    - Run the below command to launch the **MCP server**, which exposes
      APIs that agents can call as tools. (Server runs
      on [http://localhost:8000](http://localhost:8000/))

> cd mcp
>
> uv run python mcp_service.py
>
> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image21.png)
>
> Note: If you face any error, please run the below given commands:

+++pip install uv+++

+++uv run python mcp_service.py+++

9.  Let the command run, open a new terminal.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

10. **Start the Backend (Terminal 2)**:

    - Run the below command to start the backend server that hosts your
      agent workflows, session management, and API endpoints.

> cd agentic_ai/applications
>
> uv run python backend.py

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image22.png)

- Runs locally at: [http://localhost:7000](http://localhost:7000/).

- This is the core application logic that the frontend will communicate
  with. Make sure the **Connection is open**.

> ![A screen shot of a computer AI-generated content may be
> incorrect.](./media/image23.png)

11. Let the command run, open a new terminal.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

12. **Start the React Frontend (Terminal 3)**:

    - Enter the command given below to navigate to
      the react-frontend directory.

> +++cd agentic_ai/applications/react-frontend+++

- Enter the below command to launch the **React frontend** for your
  agent UI. Provides a user interface to interact with the agents and
  see their responses in real time.

> +++npm start+++

- Compilation may take some time. Please ignore the warnings and wait
  until it completes. Once the **webpack compiled successfully**, the
  Agent application runs locally
  at: [http://localhost:3000](http://localhost:3000/).

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image24.png)

13. Once all three terminals are running, the agent application will
    launch in your browser, which you can use to interact with the agent
    and test its capabilities.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

**Note**: Ensure all three terminals are running. If any of them stop,
please rerun the respective command. If all three aren't active, you may
encounter a connection error.

14. Send the below prompt in the chat **(1)** and view the
    response **(2)**:

+++Customer 251, what's my billing summary?+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

**Note**: Ensure all three terminals are running. If any of them stop,
please rerun the respective command. If all three aren't active, you may
encounter a connection error.

15. View the output, It was the ChatAgent (self.\_agent) that
    interpreted the prompt, possibly called the **MCP tool**, and
    generated the output.

    - The agent interpreted your request as a billing inquiry
      for **Customer 251**.

    - It used the **MCP tool** to fetch structured billing data.

    - The agent is working as intended — it dynamically integrates tool
      outputs and AI reasoning to answer user-specific questions.

16. After completing your testing, return to VS Code and terminate all
    running terminal sessions. This ensures that the upcoming
    multi-agent workflow runs without any interference.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

## Task 3: Multi-Agent Workflow Design

In this task, you will design and implement advanced multi-agent
workflows that demonstrate different coordination patterns. You’ll begin
by orchestrating multiple specialized agents through a central manager
to handle complex queries collaboratively, and then explore a
handoff-based system where control shifts dynamically between
domain-specific agents based on user intent.

### Task 3.1: Orchestrating Multi-Agent Workflows

In this task, you will orchestrate a multi-agent workflow where a
central orchestrator coordinates multiple specialized agents to
collaboratively process complex user queries and generate accurate,
tool-based responses.

1.  Navigate to **agent (1) \> agent_framework (2) \> multi_agent (3) \>
    magentic_group (4)** and view the code **(5)**.

    - This code represents a **multi-agent orchestration** framework
      because it defines a system where multiple specialized agents
      collaborate under the guidance of a central orchestrator.

> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image28.png)

- \_create_participants initializes multiple specialist agents
  (CRM/Billing, Product/Promotions, Security/Authentication).

- Each agent:

  - Has a specific domain and set of tools.

  - Only communicates with the orchestrator, not directly with the user.

  - Provides factual, tool-backed responses.

- Here are the agents used in this Multi-Agent workflow

  - **CRM & Billing Agent** – Handles customer accounts, subscriptions,
    billing, invoices, payments, and related queries using factual
    tool-backed data.

  - **Product & Promotions Agent** – Provides product availability,
    promotions, discounts, eligibility, and terms using structured
    sources.

  - **Security & Authentication Agent** – Manages security incidents,
    authentication issues, account lockouts, and risk mitigation
    guidance using logs and tools.

2.  Navigate to .env file **(1)**, comment out the single agent
    variable **(2)** and enter the below command to add **Orchestrating
    Multi-Agent** variable **(3)**.

+++AGENT_MODULE=agents.agent_framework.multi_agent.magentic_group+++

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image29.png)

3.  Select **File (1)** and then **Save(2)**.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

4.  Now launch the full agent application by starting its three core
    components by following the steps:

5.  In the Visual Studio Code Window, click on the **ellipses (...)
    (1)**, then **Terminal (2)**, and then **New Terminal (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

6.  **Start the MCP Server (Terminal 1)**: (mcp directory is at project
    root level)

    - Run the below command to launch the **MCP server**, which exposes
      APIs that agents can call as tools. (Server runs
      on [http://localhost:8000](http://localhost:8000/))

> cd mcp
>
> uv run python mcp_service.py

7.  Let the command run, open a new terminal.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

8.  **Start the Backend (Terminal 2)**:

    - Run the below command to start the backend server that hosts your
      agent workflows, session management, and API endpoints.

> cd agentic_ai/applications
>
> uv run python backend.py

- This is the core application logic that the frontend will communicate
  with. Make sure the **Connection is open**.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image23.png)

9.  Let the command run, open a new terminal.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

10. **Start the React Frontend (Terminal 3)**:

    - Enter the command given below to navigate to
      the react-frontend directory.

> +++cd agentic_ai/applications/react-frontend+++

- Enter the below command to launch the **React frontend** for your
  agent UI. Provides a user interface to interact with the agents and
  see their responses in real time.

> +++npm start+++

- Once the **webpack compiled successfully**, Agent application runs
  locally at: [http://localhost:3000](http://localhost:3000/).

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

11. Send the below prompt in the chat and view the response in the left
    pane:

+++Customer 251, what's my billing summary?+++

12. The orchestrator is like the manager or router. It reads the user
    query and decides which specialized agent should handle it. It uses
    the context and keywords (like “billing”, “promotion”, “login”) to
    make this decision.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.png)

13. Orchestrator assigns the task to a domain agent. The orchestrator
    sends the query to one of these internal agents:

    - crm_billing – billing, invoices, payments

    - product_promotions – products, discounts, offers

    - security_authentication – security, login, account lockouts

14. For your query (“billing summary”), the orchestrator routes it
    to **crm_billing**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

- The domain agent uses connected tools. Each agent has access to
  specific tools (APIs) via the MCP server.

- Example: crm_billing can call get_customer_detail,
  get_billing_summary, get_invoice_payment etc.

- The agent calls the right tool, fetches structured data, and forms a
  factual response.

15. After completing your testing, return to VS Code and terminate all
    running terminal sessions. This ensures that the upcoming
    multi-agent workflow runs without any interference.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

### Task 3.2: Handoff Pattern Multi-Agent System

In this task, you will explore a handoff-based multi-agent system, where
conversations seamlessly transition between specialized agents (like
Billing, Promotions, or Security) based on user intent, ensuring smooth,
context-aware interactions across domains.

- **How It Works**

  - User interacts with a domain agent directly — e.g., the CRM &
    Billing agent.

  - An intent classifier checks whether the user’s new message belongs
    to another domain (like promotions or security).

  - If so, the system automatically transfers (“handoffs”) the
    conversation to the appropriate specialist agent.

  - Each agent has filtered tools relevant to its domain (billing,
    promotions, or security).

  - Handoff happens smoothly, with context transfer so the new agent
    understands the conversation history.

1.  Expand **agents (1) \> agent_framework (2) \> multi_agent (3) \>
    handoff_multi_domain_agent (4)** and view the Code **(5)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image32.png)

2.  Navigate to .env file **(1)**, comment out the Orchestrating
    Multi-Agent variable **(2)** and enter the below command to
    add **Handoff Pattern Multi-Agent System** variable **(3)**.

+++AGENT_MODULE=agents.agent_framework.multi_agent.handoff_multi_domain_agent+++

- Enter the command given below to control how much past conversation
  context is passed during a handoff. -1 refers it transfers all
  previous conversation turns **(4)**.

> +++HANDOFF_CONTEXT_TRANSFER_TURNS=-1+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

3.  Select **File (1)** and then **Save(2)**.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

4.  Now launch the full agent application by starting its three core
    components by following the steps:

5.  In the Visual Studio Code Window, click on the **ellipses (...)
    (1)**, then **Terminal (2)**, and then **New Terminal (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

6.  **Start the MCP Server (Terminal 1)**: (mcp directory is at project
    root level)

    - Run the below command to launch the **MCP server**, which exposes
      APIs that agents can call as tools. (Server runs
      on [http://localhost:8000](http://localhost:8000/))

> cd mcp
>
> uv run python mcp_service.py

7.  Let the command run, open a new terminal.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

8.  **Start the Backend (Terminal 2)**:

    - Run the below command to start the backend server that hosts your
      agent workflows, session management, and API endpoints.

> cd agentic_ai/applications
>
> uv run python backend.py

- This is the core application logic that the frontend will communicate
  with. Make sure the **Connection is open**.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image23.png)

9.  Let the command run, open a new terminal.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

10. **Start the React Frontend (Terminal 3)**:

    - Enter the command given below to navigate to
      the react-frontend directory.

> +++cd agentic_ai/applications/react-frontend+++

- Enter the below command to launch the **React frontend** for your
  agent UI. Provides a user interface to interact with the agents and
  see their responses in real time.

> +++npm start+++

- Once the **webpack compiled successfully**, Agent application runs
  locally at: [http://localhost:3000](http://localhost:3000/).

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

11. Send the below prompt in the chat and view the response in the left
    pane:

+++Customer 251, what's my billing summary?+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image34.png)

- Here, Intent classifier routes to crm_billing domain

- get_billing_summary tool is called for customer 251

12. You can provide the following query for continuation with respect to
    billing:

+++Yes, I would like to view the invoice details+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image35.png)

**Note**: If you get a response as I was unable to retrieve the invoice
details because the referenced numbers are invoice IDs, not subscription
IDs. Please provide the subscription ID, or let me know if you need
details for a specific invoice so I can assist you correctly. Provide
the following prompt.

+++Yes, I would like to view the invoice details for customer 251+++

13. Let's now try a query related to another domain to test how the
    handoff works.

14. Enter the following query related to Product & Promotions and view
    the response.

+++Are there any promotions available for my subscription plan+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image36.png)

- Since the previous conversation was handled by the CRM & Billing
  Specialist, the system detects a domain change. It decides to hand off
  the conversation to the Product & Promotions Specialist.

- The system optionally transfers previous conversation context (like
  which customer we’re discussing) to the new agent, depending on the
  HANDOFF_CONTEXT_TRANSFER_TURNS setting.

- The Product & Promotions Specialist only has access to tools relevant
  to promotions, plans, and product information (e.g., get_promotions,
  get_eligible_promotions).

15. After completing your testing, return to VS Code and terminate all
    running terminal sessions. This ensures that the upcoming
    multi-agent workflow runs without any interference.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

**Summary**

In this lab, you created a single-agent workflow that integrates with
external tools using MCP and explored multi-agent designs where multiple
specialized agents collaborate or hand off conversations based on user
intent. You configured environment variables, launched the full agent
environment, and tested how agents intelligently respond to
domain-specific queries.
