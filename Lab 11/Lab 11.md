# Lab 11: AgentOps – Observability and Management

**Estimated Duration**: 60 Minutes

**Overview**

In this lab, you will focus on AgentOps, the discipline of monitoring,
governing, and managing AI agents in production environments. You’ll
explore how to enable observability and telemetry using the Microsoft
Agent Framework’s built-in integration with Application Insights
using **OpenTelemetry**.

About OpenTelemetry in Microsoft Agent Framework

The Microsoft Agent Framework natively integrates with OpenTelemetry,
the open standard for distributed tracing, metrics, and logging. It
provides end-to-end visibility into agent behavior by automatically
capturing telemetry data such as span traces, tool calls, model
responses, and workflow performance. Using this integration, developers
can export observability data directly to Azure Monitor, Application
Insights, or any other OpenTelemetry-compatible backend. This
standardized approach helps track every agent action across complex
multi-agent systems, enabling performance tuning, troubleshooting, and
compliance auditing with minimal configuration.

Lab Objectives

You'll perform the following tasks in this lab.

- Task 1: Enable Observability of Agent with OpenTelemetry

- Task 2: Visualize Agent Metrics

- Task 3: Monitor Agent-specific metrics in Foundry Portal

## Task 1: Enable Observability of Agent with OpenTelemetry

In this task, you’ll integrate OpenTelemetry and Agent Framework
observability into your project. You’ll configure telemetry exporters,
initialize tracing with setup_observability(), and capture detailed
spans for each stage of your workflow, including agent routing, Azure AI
Search retrieval, and ticket creation. This enables unified visibility
into agent behavior and cross-system correlation using trace IDs in
Application Insights.

1.  Instead of modifying the previous code again, you’ll work in a new
    folder that already contains the updated observability-enabled
    files. Understand how telemetry, tracing, and monitoring are
    integrated using Microsoft Agent Framework Observability and
    Application Insights.

2.  In Visual Studio Code, before openening new folder, select
    the .env file and copy the content and keep it safely in a notepad.

3.  Once done, click on **file** option from top menu and select **Open
    Folder**.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image1.png)

4.  In the open folder pane, navigate to C:\telemetry-codefiles and
    click on select folder.

5.  Once opened, the files in the explorer menu look similar to this.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

6.  Please go through the code files, review how the opentelemetry
    implemented in all agents, and how the tracing is happening.

> **Integration Overview**
>
> Integrated OpenTelemetry tracing throughout the agent workflow using
> the agent_framework.observability package.

- Imported get_tracer() and used OpenTelemetry spans to capture
  structured telemetry for each critical operation.

- Wrapped key functions (e.g., classification, routing, RAG, ticket
  creation) in spans with contextual attributes.

- Added unified startup observability setup using setup_observability()
  to configure exporters and metrics pipelines.

- Recorded custom attributes such as query text, routing decisions, and
  fallback methods for deeper visibility.

- Enhanced error handling to record exception traces and link each
  workflow execution to a trace ID for cross-system correlation.

> **File Enhancements**
>
> main.py – End-to-End Tracing and Metrics

- Configured OpenTelemetry tracing pipeline and exporter setup.

- Wrapped multi-agent orchestration inside spans for complete workflow
  visibility.

- Added spans for sub-steps: routing, data retrieval (RAG), agent
  responses, and ticket creation.

> planner_agent.py – Enhanced Routing Observability

- Added a tracer instance (get_tracer()) to monitor classification
  logic.

- Captured raw LLM responses, confidence scores, and fallback keyword
  metrics as span attributes.

- Differentiated between AI-based and heuristic classification with
  labeled spans (SpanKind.INTERNAL).

> azure_search_tool.py – RAG Observability

- Added spans for Azure Search API calls to measure latency and success
  rates.

- Logged retrieved document counts and payload sizes as custom metrics.

- Captured search errors and performance data within OpenTelemetry
  traces.

> freshdesk_tool.py – Ticket Creation Observability

- Added API call spans to track ticket creation duration and response
  status.

- Logged ticket IDs, tags, and requester details for traceable audit
  logs.

- Monitored external API latency and error responses for better incident
  tracking.

7.  Once reviewed, right-click on **.env.example (1)** file and
    select **Rename (2)** to rename the file.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

8.  Once done, rename the file from **.env.example** --\> **.env** to
    make this environment file active for this agent.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image4.png)

9.  Now, select the .env file and paste the content that you've copied
    earlier.

10. In the Azure Portal, navigate to **agenticai** resource group, and
    from the resource list select **ai-knowledge-** Search service.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

11. Select **Keys (1)** from the left menu, under Settings, and copy
    the **Query key (2)** using the copy option as shown.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

12. Once copied, paste it safely in a notepad, select **Indexes** from
    the left menu under Search Management, and copy the **Index Name
    (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

13. In the Visual Studio Code pane, select the **.env** file, as you
    have to add AI Search keys for connection.

> \# Azure AI Search (MCP)
>
> AZURE_SEARCH_ENDPOINT=https://ai-knowledge--@lab.LabInstance.Id.search.windows.net/
>
> AZURE_SEARCH_API_KEY=\[Query_Key\]
>
> AZURE_SEARCH_INDEX=\[Index_Name\]

**Note:** Please replace the Query_Key and Index_Name values with the
ones you have copied earlier.

14. Add the content of the .env file with the below content.

> AZURE_OPENAI_ENDPOINT=https://agentic-
> @lab.LabInstance.Id.cognitiveservices.azure.com/
>
> AZURE_OPENAI_API_KEY=\<Replace with Azure OpenAI key\>
>
> AZURE_OPENAI_RESPONSES_DEPLOYMENT_NAME=gpt-4o-mini
>
> AZURE_OPENAI_API_VERSION=2025-03-01-preview

15. Add the following Foundry project key variables to the .env file.

> \# Azure AI Project Configuration
>
> AZURE_AI_PROJECT_ENDPOINT=**\<Microsoft Foundry endpoint\>**
>
> AZURE_AI_MODEL_DEPLOYMENT_NAME=gpt-4o-mini
>
> Find the Microsoft Foundry project endpoint from the Overview page and
> replace **\<Microsoft Foundry endpoint\>** with that value.
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image8.png)

![](./media/image9.png)

16. Once done, add the following App Insights variables to the same
    file.

> \# Observability and Monitoring Configuration
>
> APPLICATIONINSIGHTS_CONNECTION_STRING=**\<Connection string\>**
>
> ENABLE_OTEL=true
>
> ENABLE_SENSITIVE_DATA=true
>
> Open the Application insight resource from the Azure portal, copy the
> connection string and replace **\<Connection string\>** with the value
> copied.
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image10.png)

17. In the .env file, add the following content and add the API key and
    Account URL of Freshdesk that you copied earlier.

> \# Freshdesk Configuration
>
> FRESHDESK_DOMAIN=\[Domain_URL\]
>
> FRESHDESK_API_KEY=\[API_Key\]

18. Final .env file should look like the given image.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

19. Once done, select **File** **(1)** and then
    click **Save** **(2)** to save the file.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image12.png)

20. Select the **... (1)** option from the top menu to extend the menu.
    Select **Terminal (2)** and click on **New Terminal (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

21. In **VS Code** Terminal, run the Azure CLI sign-in command:

+++az login+++

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image14.png)

22. On the **Sign in** window, select **Work or school account** and
    click **Continue**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.png)

23. On the **Sign into Microsoft** tab, and login using the below
    credentials.

- Username - <+++@lab.CloudPortalCredential(User1).Username>+++

- TAP - +++@lab.CloudPortalCredential(User1).TAP+++

24. When prompted with the sign-in options, select **No, this app
    only** to continue without linking other desktop apps.

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image16.png)

25. Type **1** and hit enter in the **Select a subscription and
    tenant**.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image17.png)

26. Once the terminal is open, execute the command,

> +++pip install -r requirements.txt+++ to install all the required
> packages.

27. Run the command given below to test out the working of the search
    tool.

+++python main.py+++

> ![A screenshot of a computer screen AI-generated content may be
> incorrect.](./media/image18.png)

## Task 2: Visualize Agent Metrics

In this task, you’ll use Azure Application Insights to visualize agent
telemetry data. You’ll explore custom metrics for response time, routing
accuracy, and ticket creation success. Then, you’ll build interactive
Azure Monitor dashboards to display key performance indicators and
trends. This helps identify bottlenecks, measure efficiency, and ensure
the healthy operation of your deployed agents in real time.

1.  Navigate to Azure Portal, open your resource group, and from the
    resource list, select **agent-insights-** app insight resource.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.png)

2.  Once in the overview page, you can see some of the default metrics
    shown.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

3.  From the left menu, select **Search (1)**, click on **See all data
    in last 24 hours (2)**.

![A screenshot of a search engine AI-generated content may be
incorrect.](./media/image21.png)

4.  Once opened, from bottom, review the **Traces (1)** and then **click
    on View as individual items (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

5.  Once done, you will be able to see all the communication details
    that happened with the agent, as well as all the transactions that
    took place within the given time range. You can also adjust the time
    range to explore more.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

6.  Explore and review these transations, you can open a detailed view
    just by clicking on them. Review how you can see all the details,
    like agents, messages, and retrieval details.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

7.  Next, select **Failures (1)**, Review **failed requests (2)** to
    gain a centralized view of all unsuccessful executions and identify
    the underlying causes through detailed trace analysis.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

8.  Next, select **Performance (1)** and check on the **operations and
    response times (2)**, based on which you can determine the
    performance SLA of the agent.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

9.  Now, under monitoring from the left menu, select **Metrics**. You
    can explore the custom metrics that are published through span.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.png)

10. Once selected, under **Metric Namespace (1)**,
    select azure.applicationinsights **(2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.png)

11. Now, under metrics, select **gen_ai.client.operation.duration and
    set the aggregation to avg (1)**. Check the **line chart (2)** to
    review the **Response Time** metric, which agent took to reply to
    the user.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image30.png)

12. In a similar way, select **gen_ai.client.token.usage and set the
    aggregation to avg (1)**. Check the **line chart (2)** to review the
    token usage from the agent.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

13. Next, select **Logs (1)** from left menu, cancel the **Queries hub
    (2)** pane.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image32.png)

14. Once closed, click on **tables** option, hover over
    the **customMetrics** parameter, you'll see a **Run** option, click
    on that.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

![A close-up of a message AI-generated content may be
incorrect.](./media/image34.png)

15. Once the query runs successfully, you will see all the custom
    metrics listed below as query results.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image35.png)

16. Next, select **Workbooks (1)** from the left menu and click on
    the **Empty (2)** workbook under Quick start.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.png)

17. Once opened, click on **+ Add (1)** and select **Add metric (2)**.

![A screenshot of a phone AI-generated content may be
incorrect.](./media/image37.png)

18. Once the metric pane is opened, click on the **Add metric** option.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

19. Now, select **Metric** as gen_ai.client.token.usage **(1)**,
    provide **Display name** as Token Usage **(2)** and click on **Save
    (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.png)

20. Again click on **Add metric** option.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

21. Now, select **Metric** as gen_ai.client.operation.duration **(1)**,
    provide **Display name** as Response Time **(2)** and click
    on **Save (3)**.

![A screenshot of a screenshot of a metric settings AI-generated content
may be incorrect.](./media/image40.png)

22. Once selected, both the metrics, click on **Run Metrics**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.png)

23. Now change the **Visualization** to **Area Chart** to get the
    similar visualization. You can explore many other options of
    visualization, and also the time range.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.png)

24. Once the edit is completed, click on **Done editing**. This will
    save this card to your workbook.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.png)

25. Now, click on **+ Add (1)** again and select **Add query (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.png)

26. In the query pane, add the following **query (1)**, and click
    on **Run Query (2)**.

+++customMetrics+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.png)

27. Check the results once the query runs successfully. Once reviewed,
    click on **Done Editing**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image46.png)

28. Once done, click on **Done editing (1)** from the top menu, and then
    click on **Save (2)** icon.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image48.png)

29. On the Save As pane, enter Title as agent-workbook **(1)**, then
    click **Save As (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image49.png)

30. Since this is a lab environment, the available data may be limited
    for comprehensive monitoring. However, you can enhance visibility by
    adding custom metrics from your agents and creating purpose-built
    monitoring dashboards focused on specific objectives, such as the
    following:

- **Agent Performance Dashboard**

> **Metrics Displayed:**

- Agent response times (avg, P95)

- Success rates by agent type

- Request volume trends

- Error rate alerts

> **Business Questions Answered:**

- Which agents perform best?

- Are we meeting SLA targets?

- What's causing system slowdowns?

&nbsp;

- **User Experience Dashboard**

> **Metrics Displayed:**

- End-to-end request latency

- Ticket creation rates

- Knowledge retrieval success

- User satisfaction proxy metrics

> **Business Questions Answered:**

- Are users getting fast responses?

- How often do requests become support tickets?

- Is the knowledge base helping users?

## Task 3: Monitor Agent-specific metrics in Foundry Portal

In this task, you’ll use Azure Application Insights to visualize agent
telemetry data. You’ll explore custom agent-specific metrics from the
Microsoft Foundry Portal.

1.  As you have already connected Application Insights to the Microsoft
    Foundry portal, you can navigate back to your Foundry portal and
    visualize the working of your agent.

2.  Navigate back to your resource group, from the resource list,
    select **agent-** foundry resource.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.png)

3.  In the next pane, click on **Go to Foundry portal**. You will now be
    navigated to the Microsoft Foundry portal, where you will be
    creating your first agent.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image51.png)

4.  Before testing the agent, connect Application Insights to enable
    detailed logs and trace visibility.

5.  In Microsoft Foundry portal, select **Monitoring (1)** from left
    menu, select **agent-insights- (2)** and click on **Connect (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image52.png)

6.  Now, navigate to the **Monitoring** pane, where you have connected
    application insights before, and select the **Resource usage** tab
    and review all the metrics and values.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image53.png)

7.  Select **Tracing (1)** from the left menu, click on any of
    the **Trace (2)**, and review the detailed traces of agent
    interactions.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image54.png)

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image55.png)

**Summary**

In this lab, you configured observability and monitoring for your
enterprise agents. Using OpenTelemetry tracing, you captured detailed
execution data for every workflow step, and by integrating with Azure
Application Insights, you created dashboards to visualize performance
metrics and agent health.

You have successfully completed this lab. Kindly click Next \>\> to
proceed further.
