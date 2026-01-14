# Manage, Securing, and Monitoring AI Agents at Scale

**Overview**

This hands-on lab provides a focused on managing, securing, and
monitoring AI agents at scale using the Azure AI Agent Service SDK and
Microsoft Foundry. Participants will dive deep into production-grade
practices essential for enterprise AI deployments, beginning with
AgentOps—the discipline of observing and governing AI agents through
OpenTelemetry integration and Azure Application Insights. The workshop
emphasizes the importance of Responsible AI by implementing Microsoft's
six foundational principles, including fairness, reliability, privacy,
and accountability, through configurable content safety filters that
detect and block harmful outputs such as hate speech, violence, and
sensitive information. Additionally, participants will build
sophisticated human-in-the-loop (HITL) workflows, exemplified through a
fraud detection system where specialized AI agents analyze suspicious
activity and intelligently route high-risk cases to human analysts for
critical decision-making. Throughout the lab, you'll work with
multi-agent systems that collaborate across retrieval, validation, and
orchestration tasks, gaining hands-on experience with end-to-end
tracing, custom metrics visualization, performance monitoring
dashboards, and real-time workflow management. By the end of this
workshop, participants will have mastered the essential skills for
deploying, monitoring, and governing AI agents in enterprise
environments, ensuring they operate safely, ethically, and efficiently
at scale while maintaining compliance with organizational policies and
regulatory requirements.

**Objectives**

By the end of this lab, you will be able to:

- **Enable Observability and Monitoring**: Implement end-to-end tracing
  and telemetry for AI agents using OpenTelemetry integrated with Azure
  Application Insights, capturing agent behavior, performance metrics,
  and execution traces

- **Visualize Agent Metrics**: Create custom dashboards and workbooks in
  Application Insights to monitor agent performance, response times,
  token usage, routing accuracy, and system health in real-time

- **Implement Responsible AI Practices**: Configure content safety
  filters in Microsoft Foundry to detect and block harmful outputs (hate
  speech, violence, sensitive content) and ensure ethical, compliant AI
  behavior

- **Build Human-in-the-Loop Workflows**: Design and deploy fraud
  detection systems where AI agents analyze alerts and route high-risk
  cases to human analysts for review and decision-making

- **Monitor Multi-Agent Systems**: Track agent-to-agent communication,
  trace distributed workflows across multiple specialized agents, and
  identify bottlenecks or failures in complex agent orchestrations

Explanation of Components

- **Microsoft Foundry**: A cloud-based platform for developing,
  deploying, and managing AI models with centralized governance,
  observability, and compliance features for enterprise AI applications.

- **Azure AI Hub**: A top-level Azure resource providing a central,
  secure, and collaborative environment for teams to build, manage, and
  deploy AI applications with shared resources and governance policies.

- **Azure AI Search**: A vector-based search service enabling
  Retrieval-Augmented Generation (RAG) by indexing and retrieving
  relevant documents to improve AI-generated responses with grounded
  information.

- **Azure AI Services**: A collection of cloud-based AI services
  offering pre-built and customizable APIs and models for vision,
  language, speech, and decision-making capabilities.

- **OpenTelemetry**: An open standard for distributed tracing, metrics,
  and logging natively integrated into the Microsoft Agent Framework to
  capture agent execution traces, performance metrics, and error
  tracking.

- **Content Safety Filters**: Built-in filtering system in Microsoft
  Foundry that automatically detects and blocks harmful outputs across
  categories like hate speech, violence, sexual content, and sensitive
  information (PII).

- **LLMs and Embeddings**: Large Language Models provide natural
  language understanding and generation, while embeddings are vector
  representations used for text similarity, search, and knowledge
  retrieval in AI applications.

# Lab 10: Prerequisites - Setting Up Knowledge Index and Ticketing System

**Estimated Duration**: 30 Minutes

**Overview**

In this prerequisite lab, you will set up the foundational components
necessary for an AI-driven workflow that can retrieve enterprise
knowledge and automatically create support tickets. The focus is on
preparing a searchable knowledge base, enabling AI agents to query that
knowledge using an MCP (Model Context Protocol) tool, and integrating a
ticketing system for downstream action.

By completing these tasks, you will establish the core infrastructure
that allows agents to:

- Retrieve relevant information from indexed data

- Use that information contextually during conversations or workflows

- Escalate issues by creating tickets in an external service

This setup ensures that subsequent labs run smoothly and reflect a
real-world enterprise scenario.

Lab Objectives

You'll perform the following tasks in this lab.

- Task 1: Prepare Knowledge Index

- Task 2: Setting up Freshworks for Ticket Management

## Task 1: Create the Azure resources

In this task, you will create all the Azure resources that are required
to perform this lab.

### Task 1.1: Create Storage account

1.  Login to the Azure portal at +++https://portal.azure.com+++ using
    the below credentials and select Storage accounts.

- Username - +++@lab.CloudPortalCredential(User1).Username+++

- TAP - <+++@lab.CloudPortalCredential(User1).TAP>+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image1.png)

2.  Select **Create**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

3.  Enter the below details and select **Review + create**. Select
    Create in the next screen.

- Storage account name - +++aistorage@lab.LabInstance.Id+++

- Preferred storage type – Select **Azure Blob Storage or Azure Data
  Lake Storage Gen2**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image3.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image4.png)

4.  Once the resource is created, select **Go to resource**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

5.  Select **Upload**, select **Create new** to create a new container.
    Name it as +++**datasets**+++ and then select **Ok**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

![A screenshot of a login box AI-generated content may be
incorrect.](./media/image7.png)

6.  Select **Browse for files**, select the policy files from
    **C:\Labfiles\Day 2** and click **Upload**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

![A screenshot of a upload box AI-generated content may be
incorrect.](./media/image9.png)

Now, the Storage account is create successfully and loaded with the
policy documents.

### Task 1.2: Create Foundry resource

In this task, you will create a Foundry resource which is required to
access the Microsoft Foundry.

1.  From the Home page of the Azure
    portal(+++https://portal.azure.com+++), select **Foundry**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.png)

2.  Select **Foundry** from the left pane, and then select **Create** to
    create the Foundry resource.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

3.  Enter the below details and select **Review + create**.

- Name – <+++agentic-@lab.LabInstance.Id>+++

- Default project name – <+++agentic-ai-project-@lab.LabInstance.Id>+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.png)

4.  Select **Create** once validated.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

5.  Ensure that the resource is created.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

6.  Open the **<agentic-ai-project-@lab.LabInstance.Id>** and select
    **Go to Foundry portal**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.png)

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image16.png)

7.  In the Microsoft Foundry, select Models + endpoints from the left
    pane. Select + **Deploy model** -\> **Deploy base model**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.png)

8.  Search for +++gpt-4o-mini+++, select it and click on Confirm to
    deploy the model.

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image18.png)

9.  Select **Deploy** in the deployment window.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.png)

10. Similarly, search for +++text-embedding-ada-002+++ and deploy it.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image21.png)

In this task, you have successfully created the Foundry resource and
deployed a chat and an embedding model in it.

### Task 1.3: Create Application insights

In this task, you will create an Application insights resource, which is
required for monitoring.

1.  From the Home page of the Azure portal, select **Subscriptions** and
    select the assigned subscription.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

2.  Select **Resource providers** from the left pane.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

3.  Search for +++Operational+++, select the 3 dots next to
    **Microsoft.OperationalInsights** and click **Register**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

4.  From the left pane of the Microsoft Foundry, select **Monitoring**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

5.  Select **Create New** -\> provide the name as
    <+++agent-insights-@lab.LabInstance.Id>+++ and then select
    **Create**.

![A screenshot of a application AI-generated content may be
incorrect.](./media/image27.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.png)

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
incorrect.](./media/image30.png)

2.  Select **AI Search** from the left pane and then select **+
    Create.**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

3.  Enter the below details, select **Review + create**.

- Service name - +++ai-knowledge-@lab.LabInstance.Id+++

- Region - East US2

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image32.png)

4.  Select **Create** once the validation passes. Select Go to resource
    once the resource is created.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image34.png)

5.  Select **Import data (new)**.

![A screenshot of a search engine AI-generated content may be
incorrect.](./media/image35.png)

6.  Select the **Azure Blob Storage** under **Choose data source**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.png)

7.  In the next pane, select the **RAG** option as we are building a
    retrieval-based agent.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image37.png)

> Here is what each of these options for -

1.  **Keyword Search:** Used for traditional search experiences based on
    exact keywords. It indexes text so users can find information
    through keyword matching, without AI reasoning.

2.  **RAG (Retrieval-Augmented Generation):** Combines document
    retrieval with AI generation.It ingests text (and simple OCR images)
    so an AI agent can provide grounded, context-aware answers.

3.  **Multimodal RAG:** Extends RAG to handle complex visual content
    like diagrams, tables, workflows, or charts. It enables AI to
    interpret both text and visual elements for richer, insight-based
    responses.

&nbsp;

8.  Select the <aistorage@lab.LabInstance.Id> under **Storage account**
    and **datasets** **under Blob containe**r and select **Next**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

9.  Select the below details and select **Next**.

- Kind – Azure AI Foundry (Preview)

- Azure AI Foundry/Hub project –
  <agentic-ai-project-@lab.LabInstance.Id>

- Model deployment – text-embedding-002-ada

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.png)

10. Select **Next** in the next screens until the **Review and create**
    screen appears.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image40.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.png)

11. Select **Create** in the **Review and create** screen.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.png)

12. Select **Close** in the Create succeeded dialog.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.png)

You’ve successfully ingested the dataset into Azure AI Search and
created a searchable index. In the next task, you’ll create an AI agent
and connect this index as its knowledge source.

# Task 2: Setting up Freshworks for Ticket Management

In this task, you will set up and configure Freshworks to enable ticket
management and an enterprise integration for your multi-agent system.

**Freshworks** is a cloud-based customer service and engagement platform
designed to improve customer support operations and enhance user
satisfaction. It offers a suite of tools for ticket management, live
chat, help center creation, and customer self-service. Freshworks
supports omnichannel communication, enabling businesses to manage
customer interactions across email, chat, phone, and social media from a
centralized interface. Its automation features help streamline
workflows, assign tickets, and provide analytics for performance
tracking. Now you will set up the Freshworks account.

1.  Copy the URL and paste it in a new tab in your browser inside the VM
    to open the **Freshworks** portal.

    - URL:

> +++https://www.freshworks.com/freshdesk/lp/home/?tactic_id=3387224&utm_source=google-adwords&utm_medium=FD-Search-Brand-India&utm_campaign=FD-Search-Brand-India&utm_term=freshdesk&device=c&matchtype=e&network=g&gclid=EAIaIQobChMIuOK90qvLjQMV_dQWBR3JAi9VEAAYASAAEgK87_D_BwE&audience=kwd-30002131023&ad_id=282519464145&gad_source=1&gad_campaignid=671502402+++

2.  In the portal, select **Start free trial** to start the free trial.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.png)

3.  In the next pane, provide these details and click on **Try it free
    (6)**:

    - **First name:** LODS

    - **Last name:** User1

    &nbsp;

    - **Work
      email:** **+++@lab.CloudPortalCredential(User1).Username+++**

    &nbsp;

    - **Company name:** Zava

    - **Organization size:** Select **1-10**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image45.png)

4.  In the next pane, provide these details and click on **Next (4)**:

    - **What industry are you from ?:** from the list, select **Software
      and internet (1)**

    - **How many employees are there in your company?:** select **1-10
      (2)**

    - select **I'm trying customer service software for the first time
      (3)**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image46.png)

5.  Once done, copy the URL given and paste it in a new tab in your
    browser inside VM to open **Outlook**.

    - URL:

> +++https://go.microsoft.com/fwlink/p/?LinkID=2125442&clcid=0x409&culture=en-us&country=us+++

6.  In the pick an account pane, select the account that you are
    assigned for this lab.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.png)

7.  In the Freshworks verification email, open and click on **Activate
    Account**.

> ![A screenshot of a computer screen AI-generated content may be
> incorrect.](./media/image48.png)

**Note:** If you're unable to locate the activation email from
Freshworks, please wait a few minutes, as there might be a delay in
email delivery. If the email doesn't arrive after some time, consider
reinitiating the steps to activate your free trial in a new
private/incognito window. Additionally, check your spam or junk folder,
as the email might have been filtered there.

8.  In the next pane, provide as **Enter password (1)** and provide the
    same password for **Confirm password (2)**. Click on **Activate your
    account (3)**.

> ![A screenshot of a login screen AI-generated content may be
> incorrect.](./media/image49.png)

9.  Once you are in the portal, click on the **Profile (1)** icon in the
    top right corner and select **Profile settings (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.png)

10. In the profile page, click on **View API Key** to get the API Keys.

![A screenshot of a web page AI-generated content may be
incorrect.](./media/image51.png)

**Note:** If you are unable to find this option, please minimize the
screen size using **CTRL + -**.

11. In the next pane, complete the **CAPTCHA**.

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image52.png)

12. Please copy the API Key to a notepad, you will be using this
    further.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image53.png)

13. From the browser tab, please copy the **Account URL** as shown and
    copy the value to Notepad. You will be using this further.

![](./media/image54.png)

**Summary**

By completing this prerequisite lab, you have set up the essential
foundation for an end-to-end agent workflow. You prepared a searchable
knowledge index, enabled agents to query that data through an MCP tool
built on **Azure AI Search**, and integrated **Freshworks** for
automated ticket management.

This foundation ensures that agents can retrieve accurate context, make
informed decisions, and escalate issues efficiently preparing the
environment for more advanced agent-driven scenarios in the upcoming
labs.

You have successfully completed this lab. Kindly click Next \>\> to
proceed further
