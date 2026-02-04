
# Lab 5 Design Scalable AI Agents with Microsoft Foundry and Agent Framework

**Overview**

In this hands-on lab spanned across 3 days for designing and building
scalable AI agents using Microsoft Foundry and the Microsoft Agent
Framework. Participants will begin by creating their first AI agent
through the Microsoft Foundry portal, where they will learn to upload
enterprise policy documents and ingest them into Azure AI Search to
prepare a searchable knowledge base. The workshop then progresses to
building multi-agent systems using the Microsoft Agent Framework SDK,
where multiple specialized agents collaborate through Agent-to-Agent
(A2A) communication patterns. Learners will extend their agent
capabilities by integrating external tools and data sources using the
Model Context Protocol (MCP), connecting to both Azure AI Search for
knowledge retrieval and external APIs like Freshdesk for ticket
management. The training advances to deploying agents into the Microsoft
Foundry Agent Service as persistent, cloud-hosted solutions with state
management and enterprise-grade reliability. Finally, participants will
implement advanced workflow patterns including orchestrated multi-agent
systems with centralized coordination and handoff-based systems where
conversations seamlessly transition between specialized agents based on
user intent and domain expertise.

**Objectives**

By the end of this lab, you will be able to:

- **Set Up AI Project and Perform Chat Completion from VS
  Code:** Configure a production-ready AI development environment by
  creating an Microsoft Foundry project, deploying GPT-4 and embedding
  models, and establishing secure connections from Visual Studio Code.
  You will validate the setup by executing chat completion calls,
  ensuring seamless integration between your local development
  environment and Azure AI services with proper authentication and
  project configuration.

- **Build a Health Insurance Plans Analyzer AI Agent:** Develop an
  intelligent AI Agent specialized in analyzing and visualizing health
  insurance data. You will create an agent that processes complex health
  benefit plan information and automatically generates comparative bar
  charts, demonstrating core AI agent capabilities including data
  interpretation, natural language understanding, code execution, and
  automated visualization generation for decision support.

- **Develop a Multi-Agent Collaborative System:** Design and implement
  an advanced multi-agent architecture where specialized AI agents work
  together to analyze health plan documents and generate comprehensive
  reports. You will build a Search Agent for intelligent document
  retrieval using Azure AI Search, a Report Agent for generating
  detailed analytical reports, a Validation Agent for ensuring
  compliance and accuracy, and an Orchestrator Agent for managing
  inter-agent communication and workflow coordination, showcasing
  enterprise-grade agent collaboration patterns.

**Prerequisites**

Participants should have:

- **Azure & Cloud Experience** - Familiarity with Azure Portal, Resource
  Groups, and Azure AI services

- **Programming Skills** - Basic Python knowledge (async/await,
  environment variables, API calls)

- **AI Concepts** - Understanding of LLMs, embeddings, RAG
  (Retrieval-Augmented Generation), and prompt engineering

- **Development Tools** - Proficiency with Visual Studio Code, terminal
  usage, and Git

- **Agent Framework Awareness** - Basic knowledge of agent
  architectures, tools, and orchestration patterns

Explanation of Components

- **Microsoft Foundry**: Microsoft Foundry is a cloud platform for
  developing, deploying, and managing enterprise AI agents. It provides
  managed Agent Service runtime, centralized project management, and
  Application Insights monitoring, ensuring enterprise-grade
  reliability, security, and observability throughout the agent
  lifecycle.

- **Microsoft Agent Framework SDK**: The official Python SDK for
  building intelligent, modular agents that replace AutoGen and Semantic
  Kernel. It features native Agent-to-Agent communication, Model Context
  Protocol integration, and Microsoft Foundry support, enabling
  production-ready enterprise agent systems with standardized tool
  usage.

- **Azure AI Search**: A vector-based search engine enabling
  Retrieval-Augmented Generation workflows. It provides hybrid retrieval
  combining vector similarity and keyword search, semantic ranking for
  improved relevance, and document indexing capabilities, ensuring
  agents deliver grounded, factually accurate responses from enterprise
  knowledge sources.

- **Model Context Protocol (MCP)**: A standardized interface enabling
  agents to access external knowledge and tools securely. MCP connects
  to enterprise data sources, external APIs like Freshdesk, and custom
  tools with structured schemas, ensuring reliable, auditable
  interactions and forming the foundation for extensible enterprise AI
  systems.

- **Chat Response Agent**: A single-turn, stateless agent model for
  local development and testing. It processes requests independently
  without retaining context, running within local environments and
  responding immediately. Ideal for prototyping core logic and
  validating behavior before advancing to production with persistent
  agents.

- **Persistent Agent**: A cloud-hosted, long-lived service in Microsoft
  Foundry maintaining state across conversations. It supports external
  tool integration via MCP, Agent-to-Agent collaboration, and
  enterprise-scale reliability with built-in monitoring, providing
  foundations for production applications requiring stateful, multi-turn
  conversational experiences.

- **Planner Agent**: An intelligent orchestrator analyzing user queries
  to route them to appropriate specialist agents. It uses AI reasoning
  and keyword heuristics to classify queries across domains like HR,
  Finance, or Compliance, ensuring optimal task distribution and serving
  as the central coordination point.

- **Worker Agents**: Domain specialists with expertise in specific areas
  like HR, Finance, or Compliance. Each agent has domain-specific
  instructions, specialized tools, and relevant knowledge sources. They
  collaborate with planner agents through A2A communication, delivering
  authoritative, accurate responses for complex domain-specific
  inquiries.

- **Azure OpenAI**: Enterprise-grade service providing access to
  advanced LLMs through secure API endpoints. It offers chat completion,
  embedding models, content filtering, and compliance features.
  Seamlessly integrates with Microsoft Foundry, enabling agents to
  leverage GPT-4 while maintaining data privacy and governance controls.

# Lab 5: Building a Retrieval-Augmented AI Agent with Microsoft Foundry

**Overview**

In this lab, you'll create your first AI Agent using the Microsoft
Foundry portal. You'll begin by uploading enterprise policy documents
and ingesting them into Azure AI Search to prepare a knowledge base.
Then, you'll configure the agent using the Microsoft Agent Framework to
enable retrieval-augmented generation (RAG). Finally, you'll test the
agent's responses and analyze execution logs to observe how it retrieves
and processes information.

**Lab Objectives**

You'll perform the following tasks in this lab.

- Task 1: Create the Azure resources

- Task 2: Create an AI Agent in Microsoft Foundry

- Task 3: Connect Azure AI Search for RAG

- Task 4: Test and Observe Agent Execution Logs

## Task 1: Create the Azure resources

In this task, you will create all the Azure resources that are required
to perform this lab.

### Task 1.1: Create Storage account

1.  Login to the Azure portal at +++https://portal.azure.com+++ using
    the below credentials and select **Storage accounts**.

	- Username - +++@lab.CloudPortalCredential(User1).Username+++

	- TAP - +++@lab.CloudPortalCredential(User1).AccessToken+++

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image1.png)

2.  Select **Create**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image2.png)

3.  Enter the below details and select **Review + create**. Select
    **Create** in the next screen.

	- Storage account name - +++aistorage@lab.LabInstance.Id+++

	- Preferred storage type - Select **Azure Blob Storage or Azure Data
	  Lake Storage Gen2**

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image3.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image4.png)

4.  Once the resource is created, select **Go to resource**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image5.png)

5.  Select **Upload**, select **Create new** to create a new container.
    Name it as +++**datasets**+++ and then select **Ok**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image6.png)

    ![A screenshot of a login box AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image7.png)

6.  Select **Browse for files**, select the policy files from
    **C:\Labfiles\Day 2** and click **Upload**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image8.png)

    ![A screenshot of a upload box AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image9.png)

Now, the Storage account is create successfully and loaded with the
policy documents.

### Task 1.2: Create Foundry resource

In this task, you will create a Foundry resource which is required to
access the Microsoft Foundry.

1.  From the Home page of the Azure
    portal(+++https://portal.azure.com+++), select **Foundry**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image10.png)

2.  Select **Foundry** from the left pane, and then select **Create** to
    create the Foundry resource.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image11.png)

3.  Enter the below details and select **Review + create**.

    - Name - +++agentic-@lab.LabInstance.Id+++

    - Default project name - +++agentic-ai-project-@lab.LabInstance.Id+++

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image12.png)

4.  Select **Create** once validated.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image13.png)

5.  Ensure that the resource is created.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image14.png)

6.  Open the **agentic-ai-project-@lab.LabInstance.Id** and select
    **Go to Foundry portal**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image15.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image16.png)

7.  In the Microsoft Foundry, select Models + endpoints from the left
    pane. Select + **Deploy model** -> **Deploy base model**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image17.png)

8.  Search for +++gpt-4o-mini+++, select it and click on Confirm to
    deploy the model.

    ![A screenshot of a chat AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image18.png)

9.  Select **Deploy** in the deployment window.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image19.png)

10. Similarly, search for +++text-embedding-ada-002+++ and deploy it.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image20.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image21.png)

In this task, you have successfully created the Foundry resource and
deployed a chat and an embedding model in it.

### Task 1.3: Create Application insights

In this task, you will create an Application insights resource, which is
required for monitoring.

1.  From the Home page of the Azure portal, select **Subscriptions** and
    select the assigned subscription.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image22.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image23.png)

2.  Select **Resource providers** from the left pane.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image24.png)

3.  Search for +++Operational+++, select the 3 dots next to
    **Microsoft.OperationalInsights** and click **Register**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image25.png)

4.  From the left pane of the Microsoft Foundry, select **Monitoring**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image26.png)

5.  Select **Create New** -> provide the name as
    +++agent-insights-@lab.LabInstance.Id+++ and then select
    **Create**.

    ![A screenshot of a application AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image27.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image28.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image29.png)

In this task, you have created the Application Insight resource.

### Task 1.4: Create Search resource

Before an AI Agent can answer enterprise questions accurately, it must
access trusted data sources. Azure AI Search enables Retrieval-Augmented
Generation (RAG) by indexing documents such as policies, contracts, and
manuals. An index acts like a searchable catalog that breaks content
into chunks, adds metadata, and enables the agent to retrieve the right
information during a conversation.

In this task, index the uploaded documents using Azure AI Search to
create a searchable knowledge base.

1.  From the Home page of the Azure portal, select **Foundry**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image30.png)

2.  Select **AI Search** from the left pane and then select **+
    Create.**

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image31.png)

3.  Enter the below details, select **Review + create**.

	- Service name - +++ai-knowledge-@lab.LabInstance.Id+++

	- Region - **@lab.CloudResourceGroup(AgenticAI).Location**
    
	**Note:** Please select a region that allows the Standard pricing tier

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image32.png)

4.  Select **Create** once the validation passes. Select Go to resource
    once the resource is created.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image33.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image34.png)

5.  Select **Import data (new)**.

    ![A screenshot of a search engine AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image35.png)

6.  Select the **Azure Blob Storage** under **Choose data source**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image36.png)

7.  In the next pane, select the **RAG** option as we are building a
    retrieval-based agent.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image37.png)

 Here is what each of these options for -

1.  **Keyword Search:** Used for traditional search experiences based on
    exact keywords. It indexes text so users can find information
    through keyword matching, without AI reasoning.

2.  **RAG (Retrieval-Augmented Generation):** Combines document
    retrieval with AI generation.It ingests text (and simple OCR images)
    so an AI agent can provide grounded, context-aware answers.

3.  **Multimodal RAG:** Extends RAG to handle complex visual content
    like diagrams, tables, workflows, or charts. It enables AI to
    interpret both text and visual elements for richer, insight-based
    responses.

8.  Select the **aistorage@lab.LabInstance.Id** under **Storage account**
    and **datasets** **under Blob container** and select **Next**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image38.png)

9.  Select the below details and select **Next**.

	- Kind - **Azure AI Foundry (Preview)**

	- Azure AI Foundry/Hub project -
	  **agentic-ai-project-@lab.LabInstance.Id**

	- Model deployment - **text-embedding-002-ada**

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image39.png)

10. Select **Next** in the next screens until the **Review and create**
    screen appears.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image40.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image41.png)

11. Select **Create** in the **Review and create** screen.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image42.png)

12. Select **Close** in the Create succeeded dialog.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image43.png)

You've successfully ingested the dataset into Azure AI Search and
created a searchable index. In the next task, you'll create an AI agent
and connect this index as its knowledge source.

## Task 2: Create an AI Agent in Microsoft Foundry

In this task, you will create a new AI Agent in Microsoft Foundry and
configure its core purpose, instructions, and model using the Microsoft
Agent Framework interface.

1.  Navigate back to your resource group, from the resource list,
    select **agentic-** foundry resource.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image44.png)

2.  In the next pane, click on **Go to Foundry portal**. You will now be
    navigated to the Microsoft Foundry portal, where you will be
    creating your first agent.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image45.png)

3.  Once navigated to Foundry Portal, select **Agents** from the
    left menu you will already see an agent **pre created**. If not
    created, then please click on the **+ New agent** option to get
    it created.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image46.png)

4.  Select the newly created **agent**, and a configuration pane will be
    opened on the right. Provide the following details.

	
	| Column 1 | Column 2 |
	| -------- | -------- |
	| **Agent name **  |     +++**EnterpriseAgent**+++     |
	| **Instructions **   |   +++You are an enterprise knowledge assistant. Retrieve relevant policy information before answering questions.+++       |

    ![A screenshot of a computer program AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image47.png)

5.  You've successfully created an agent in Microsoft Foundry. Next,
    it's time to enrich it with knowledge by connecting your indexed
    data in the upcoming task.

## Task 3: Connect Azure AI Search for RAG

In this task, you will integrate Azure AI Search with your agent using
the knowledge integration panel, enabling retrieval-augmented responses
through MCP (Model Context Protocol).

1.  In the same agent configuration pane, scroll down and click on **+
    Add** for **Knowledge** parameter.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image48.png)

2.  In the **Add knowledge** pane, select **Azure AI Search** as you
    have the index prepared in the AI Search resource.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image49.png)

3.  In the next pane, for **Azure AI Search resource
    connection** option, click on **drop-down arrow** and
    select **Connect other Azure AI Search resource**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image50.png)

4.  In the next pane, review that the correct AI Search resource is
    selected and click on **Add connection**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image51.png)

5.  In the **Adding Azure AI Search** step, configure the following
    details and click on **Connect** once completed.

	
	

	| **Detail** | **Value** |
	| -------- | -------- |
	| **Azure AI Search resource connection**   |     **AIknowledge@lab.LabInstance.Id**     |
	| **Azure AI Search index ** |    **rag index **     |
	| **Display name**   |    +++**knowledge-index**+++      |
	| S**earch type **   |    **Hybrid (vector + keword )**      |

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image52.png)

6.  The agent is now successfully enriched with knowledge using the
    Azure AI Search index, which acts as a searchable knowledge base for
    retrieving accurate information during conversations.

## Task 4: Test and Observe Agent Execution Logs

In this task, you will test your agent by asking policy-related
questions and reviewing structured logs to verify tool usage, search
calls, and grounded responses.

1.  Before testing the agent, connect Application Insights to enable
    detailed logs and trace visibility.

2.  In Microsoft Foundry portal, select **Monitoring** from left
    menu, select **agent-insights-@lab.LabInstance.Id** and click on **Connect**

    ![](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image53.png)

3.  Once done, select, **Agents** from left menu, and then choose
    the **EnterpriseAssistant** agent and click on **Try in
    playground**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image54.png)

4.  A chat panel will open where you can enter your prompts. The agent
    will now respond using the documents and datasets you've connected.

	Sample prompts -

	- +++What is the employee travel reimbursement policy?+++

	- +++Summarize the contract approval rules and cite the document.+++

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image55.png)

5.  Once the agent responds to questions, click on **Thread logs** from
    the top menu to check the logs and traces of the current thread.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image56.png)

6.  Explore and review these metrics, traces, and evaluations which
    showcase a detailed overiew on the agent log.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image57.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image58.png)

7.  Now, navigate to the **monitoring** pane, where you have connected
    application insights before, and select the **Resource usage** tab
    and review all the metrics and values.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%205/media/image59.png)

8.  You've successfully built a RAG-based agent powered by curated
    enterprise datasets. Next, you'll take this further by enabling
    multi-agent collaboration, where agents can delegate, reason, and
    work together intelligently.

Summary

In this lab, you successfully created your first AI Agent in Microsoft
Foundry and connected it to an indexed knowledge base. You uploaded
documents, ingested them into Azure AI Search, and enabled RAG through
Microsoft Agent Framework integration. By testing the agent and
reviewing execution logs, you gained firsthand experience in how agents
retrieve grounded information and generate enterprise-ready responses.



