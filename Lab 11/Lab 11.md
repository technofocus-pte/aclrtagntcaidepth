
# Lab 11: AgentOps – Observability and Management

**Estimated Duration**: 60 Minutes

**Overview**

In this lab, you will focus on AgentOps, the discipline of monitoring,
governing, and managing AI agents in production environments. You’ll
explore how to enable observability and telemetry using the Microsoft
Agent Framework’s built-in integration with Application Insights
using **OpenTelemetry**.

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
initialize tracing with setup_observability(), and capture detailed
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
    the .env file and copy the content and keep it safely in a notepad.

3.  Once done, click on **file** option from top menu and select **Open
    Folder**.

    ![A screenshot of a computer program AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image1.png)

4.  In the open folder pane, navigate to **C:\Lab Files\Day 3\Enterprise-Agent-Code-files** and
    click on select folder.

5.  Once opened, the files in the explorer menu look similar to this.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image2.png)

6.  Please go through the code files, review how the opentelemetry
    implemented in all agents, and how the tracing is happening.

	> **Integration Overview**
	>
	integrated OpenTelemetry tracing throughout the agent workflow using
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

7.  Once reviewed, right-click on **.env.sample** file and
    select **Rename** to rename the file.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image3.png)

8.  Rename the file from **.env.example** --\> **.env** to
    make this environment file active for this agent.

    ![A screen shot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image4.png)

10. In the Azure Portal, navigate to **agenticai** resource group, and
    from the resource list select **ai-knowledge-** Search service.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image5.png)
 
11. Select **Keys** from the left menu, under Settings, and copy
    the **Query key** using the copy option as shown. Save it in a notepad.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image6.png)

12. Once copied, paste it safely in a notepad, select **Indexes** from
    the left menu under Search Management, and copy the **Index Name**. Save it in a notepad.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image7.png)

13. Populate the **.env** file, with the below content, replace the place holders of the Query_Key and the Index_name with the values copied and saved above.

	```
	# Azure AI Search (MCP)
	AZURE_SEARCH_ENDPOINT=https://ai-knowledge-@lab.LabInstance.Id.search.windows.net/
	AZURE_SEARCH_API_KEY=[Query_Key]
	AZURE_SEARCH_INDEX=[Index_Name]
	```

14. Add the content of the .env file with the below content.

	```
	AZURE_OPENAI_ENDPOINT=https://agentic-@lab.LabInstance.Id.cognitiveservices.azure.com/
	AZURE_OPENAI_API_KEY=<Replace with Azure OpenAI key>
	AZURE_OPENAI_RESPONSES_DEPLOYMENT_NAME=gpt-4o-mini
	AZURE_OPENAI_API_VERSION=2025-03-01-preview
	```

15. Add the following Foundry project key variables to the .env file.

	```
	# Azure AI Project Configuration
	AZURE_AI_PROJECT_ENDPOINT=<Microsoft Foundry endpoint>
	AZURE_AI_MODEL_DEPLOYMENT_NAME=gpt-4o-mini
	```

	Find the Microsoft Foundry project endpoint from the Overview page and replace **\<Microsoft Foundry endpoint\>** with that value.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image8.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image9.png)

16. Once done, add the following App Insights variables to the same
    file.

	```
	# Observability and Monitoring Configuration
	APPLICATIONINSIGHTS_CONNECTION_STRING=<Connection string>
	ENABLE_OTEL=true
	ENABLE_SENSITIVE_DATA=true

	```

	Open the Application insight resource from the Azure portal, copy the connection string and replace **< Connection string >** with the value copied.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image10.png)

17. In the .env file, add the following content and add the API key and
    Account URL of Freshdesk that you copied earlier.

    ```
    # Freshdesk Configuration
    FRESHDESK_DOMAIN=[Domain_URL]
    FRESHDESK_API_KEY=[API_Key]
    
    ```

18. Final .env file should look like the given image.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/im4.png)

19. Once done, select **File**  and then
    click **Save**  to save the file.

    ![A screenshot of a computer menu AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image12.png)

20. Select the **...** option from the top menu to extend the menu.
    Select **Terminal** and click on **New Terminal**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image13.png)

21. In **VS Code** Terminal, run the Azure CLI sign-in command:

	+++az login+++

    ![A screen shot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image14.png)

22. On the **Sign in** window, select **Work or school account** and
    click **Continue**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image15.png)

23. On the **Sign into Microsoft** tab, and login using the below
    credentials.

	- Username - +++@lab.CloudPortalCredential(User1).Username+++

	- TAP - +++@lab.CloudPortalCredential(User1).AccessToken+++

24. When prompted with the sign-in options, select **No, this app
    only** to continue without linking other desktop apps.

    ![A screenshot of a computer error AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image16.png)

25. Type **1** and hit enter in the **Select a subscription and
    tenant**.

    ![A screenshot of a computer program AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image17.png)

26. Rename the **requirements.txt.txt** to **requirements.txt**.

27. Once the terminal is open, execute the command,

    +++pip install -r requirements.txt+++ to install all the required packages.

28. Run the command given below to test out the working of the search
    tool.

	+++python main.py+++

    ![A screenshot of a computer screen AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image18.png)

## Task 2: Visualize Agent Metrics

In this task, you’ll use Azure Application Insights to visualize agent
telemetry data. You’ll explore custom metrics for response time, routing
accuracy, and ticket creation success. Then, you’ll build interactive
Azure Monitor dashboards to display key performance indicators and
trends. This helps identify bottlenecks, measure efficiency, and ensure
the healthy operation of your deployed agents in real time.

1.  Navigate to Azure Portal, open your resource group, and from the
    resource list, select **agent-insights-** app insight resource.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image19.png)

2.  Once in the overview page, you can see some of the default metrics
    shown.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image20.png)

3.  From the left menu, select **Search**, click on **See all data
    in last 24 hours**.

    ![A screenshot of a search engine AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image21.png)

4.  Once opened, from bottom, review the **Traces** and then **click
    on View as individual items**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image22.png)

5.  Once done, you will be able to see all the communication details
    that happened with the agent, as well as all the transactions that
    took place within the given time range. You can also adjust the time
    range to explore more.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image23.png)

6.  Explore and review these transations, you can open a detailed view
    just by clicking on them. Review how you can see all the details,
    like agents, messages, and retrieval details.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image24.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image25.png)

7.  Next, select **Failures**, Review **failed requests** to
    gain a centralized view of all unsuccessful executions and identify
    the underlying causes through detailed trace analysis.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image26.png)

8.  Next, select **Performance** and check on the **operations and
    response times**, based on which you can determine the
    performance SLA of the agent.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image27.png)

9.  Now, under monitoring from the left menu, select **Metrics**. You
    can explore the custom metrics that are published through span.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image28.png)

10. Once selected, under **Metric Namespace**,
    select azure.applicationinsights .

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image29.png)

11. Now, under metrics, select **gen_ai.client.operation.duration and
    set the aggregation to avg**. Check the **line chart** to
    review the **Response Time** metric, which agent took to reply to
    the user.

    ![A screen shot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image30.png)

12. In a similar way, select **gen_ai.client.token.usage and set the
    aggregation to avg**. Check the **line chart** to review the
    token usage from the agent.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image31.png)

13. Next, select **Logs** from left menu, cancel the **Queries hub
   ** pane.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image32.png)

14. Once closed, click on **tables** option, hover over
    the **customMetrics** parameter, you'll see a **Run** option, click
    on that.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image33.png)

    ![A close-up of a message AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image34.png)

15. Once the query runs successfully, you will see all the custom
    metrics listed below as query results.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image35.png)

16. Next, select **Workbooks** from the left menu and click on
    the **Empty** workbook under Quick start.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image36.png)

17. Once opened, click on **+ Add** and select **Add metric**.

    ![A screenshot of a phone AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image37.png)

18. Once the metric pane is opened, click on the **Add metric** option.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image38.png)

19. Now, select **Metric** as gen_ai.client.token.usage ,
    provide **Display name** as Token Usage  and click on **Save**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image39.png)

20. Again click on **Add metric** option.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image38.png)

21. Now, select **Metric** as gen_ai.client.operation.duration ,
    provide **Display name** as Response Time  and click
    on **Save**.

    ![A screenshot of a screenshot of a metric settings AI-generated content
may be incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image40.png)

22. Once selected, both the metrics, click on **Run Metrics**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image41.png)

23. Now change the **Visualization** to **Area Chart** to get the
    similar visualization. You can explore many other options of
    visualization, and also the time range.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image42.png)

24. Once the edit is completed, click on **Done editing**. This will
    save this card to your workbook.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image43.png)

25. Now, click on **+ Add** again and select **Add query**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image44.png)

26. In the query pane, add the following **query**, and click
    on **Run Query**.

	+++customMetrics+++

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image45.png)

27. Check the results once the query runs successfully. Once reviewed,
    click on **Done Editing**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image46.png)

28. Once done, click on **Done editing** from the top menu, and then
    click on **Save** icon.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image47.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image48.png)

29. On the Save As pane, enter Title as agent-workbook , then
    click **Save As**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image49.png)

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
    select **agent-** foundry resource.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image50.png)

3.  In the next pane, click on **Go to Foundry portal**. You will now be
    navigated to the Microsoft Foundry portal, where you will be
    creating your first agent.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image51.png)

4.  Before testing the agent, connect Application Insights to enable
    detailed logs and trace visibility.

5.  In Microsoft Foundry portal, select **Monitoring** from left
    menu, select **agent-insights-@Lab.Labinstance.id** and click on **Connect**.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image52.png)

6.  Now, navigate to the **Monitoring** pane, where you have connected
    application insights before, and select the **Resource usage** tab
    and review all the metrics and values.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image53.png)

7.  Select **Tracing** from the left menu, click on any of
    the **Trace**, and review the detailed traces of agent
    interactions.

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image54.png)

    ![A screenshot of a computer AI-generated content may be
incorrect.](https://raw.githubusercontent.com/technofocus-pte/aclrtagntcaidepth/refs/heads/main/Lab%2011/media/image55.png)

**Summary**

In this lab, you configured observability and monitoring for your
enterprise agents. Using OpenTelemetry tracing, you captured detailed
execution data for every workflow step, and by integrating with Azure
Application Insights, you created dashboards to visualize performance
metrics and agent health.

You have successfully completed this lab. Kindly click Next \>\> to
proceed further.


