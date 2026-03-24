# 实验11：AgentOps – 可观测性与管理

**预计时长**：60分钟

**概述**

在本实验室中，你将专注于AgentOps，即在生产环境中监控、管理和管理AI代理的学科。您将探索如何利用
Microsoft Agent Framework 内置的 Application Insights 集成，通过
**OpenTelemetry** 实现可观测性和遥测。

关于 Microsoft Agent Framework 中的 OpenTelemetry

Microsoft Agent
Framework原生集成了OpenTelemetry，OpenTelemetry是分布式追踪、指标和日志的开放标准。它通过自动捕获遥测数据（如跨度追踪、工具调用、模型响应和工作流性能）提供端到端的代理行为可视化。通过这种集成，开发者可以直接将可观测性数据导出到
Azure Monitor、Application Insights 或其他兼容 OpenTelemetry
的后端。这种标准化方法有助于跟踪复杂多智能体系统中的每一个代理作，实现性能调优、故障排除和合规审计，且配置极简。

实验室目标

你将在实验室执行以下任务。

- 任务1：启用OpenTelemetry的代理可观测性

- 任务2：可视化代理指标

- 任务3：监控Foundry门户中的代理特定指标

## 任务1：启用OpenTelemetry的代理可观测性

在这个任务中，你将把OpenTelemetry和Agent
Framework的可观测性集成到你的项目中。你将配置遥测导出器，使用setup_observability（）初始化追踪，并捕捉工作流程各阶段的详细数据，包括代理路由、Azure
AI
搜索检索和工单创建。这使得通过应用洞察中的跟踪ID实现对代理行为和跨系统关联的统一可视化。

1.  你不会再修改之前的代码，而是在一个已经包含已更新可观察性文件的新文件夹中工作。了解如何通过Microsoft代理框架的可观察性和应用洞察集成遥测、追踪和监控。

2.  在Visual Studio
    Code中，打开新文件夹前，先选择.env文件并复制内容，并安全地保存在记事本中。

3.  完成后，点击顶部菜单中的** file **选项，选择**“Open Folder**”。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image1.png)

4.  在打开文件夹的窗格中，进入C：\telemetry-codefiles，点击select文件夹。

5.  打开后，资源管理器菜单里的文件看起来和这个很像。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

6.  请仔细查看代码文件，查看所有代理中OpenTelemetry的实现情况，以及追踪是如何进行的。

> **集成概述**
>
> 通过 agent_framework.observability 包，集成了整个代理工作流程的
> OpenTelemetry 追踪。

- 导入了get_tracer（）并使用OpenTelemetry跨度捕获每个关键作的结构化遥测数据。

- 将关键函数（如分类、路由、RAG、工单创建）包裹在带有上下文属性的范围内。

- 增加了统一的启动可观察性设置，使用setup_observability（）来配置导出器和指标流水线。

- 记录自定义属性，如查询文本、路由决策和备用方法，以实现更深层次的可视化。

- 增强错误处理功能，记录异常痕迹，并将每个工作流执行关联到轨迹ID，实现跨系统关联。

> **文件增强**
>
> main.py – 端到端追踪与指标

- 配置了OpenTelemetry追踪流水线和导出器设置。

- 跨区内的多代理编排包包，实现完整的工作流程可视化。

- 新增了子步骤的跨度：路由、数据检索（RAG）、代理响应和工单创建。

> planner_agent.py – 增强的路由可观测性

- 新增了一个追踪实例（get_tracer（））用于监控分类逻辑。

- 捕捉了原始的LLM响应、信心评分和备用关键词指标作为跨度属性。

- 区分基于 AI 和带标签跨度的启发式分类（SpanKind.INTERNAL）。

> azure_search_tool.py – RAG 可观测性

- 增加了用于 Azure Search API 调用的跨度，以测量延迟和成功率。

- 记录检索的文档数量和有效载荷大小作为自定义指标。

- 在OpenTelemetry追踪中捕获搜索错误和性能数据。

> freshdesk_tool.py – 工单创建可观察性

- 新增了API调用范围，用于跟踪工单创建时间和响应状态。

- 记录工单ID、标签和请求者详情，以便可追溯审计日志。

- 监控外部API延迟和错误响应，以更好地跟踪事件。

7.  审核完成后，右键点击 **.env.example （1）** 文件，选择 **Rename
    (2)** 以重命名该文件。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

8.  完成后，将文件重命名为 **.env.example** --\>
    **.env**，使该环境文件为该代理激活。

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image4.png)

9.  现在，选择 .env 文件，粘贴你之前复制的内容。

10. 在 Azure 门户中，导航到 **agenticai** 资源组，从资源列表中选择
    **ai-knowledge-** Search Service。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

11. 在设置中左侧菜单选择 **Keys (1)** ，然后使用复制选项复制 **Query key
    (2)** 。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

12. 复制完成后，安全地粘贴到记事本，在搜索管理的左侧菜单中选择
    **Indexes**，复制 **Index Name (2)**。 

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

13. 在Visual Studio
    Code面板中，选择**.env**文件，因为你需要添加AI搜索键才能连接。

> \# Azure AI Search (MCP)
>
> AZURE_SEARCH_ENDPOINT=https://ai-knowledge--@lab.LabInstance.Id.search.windows.net/
>
> AZURE_SEARCH_API_KEY=\[Query_Key\]
>
> AZURE_SEARCH_INDEX=\[Index_Name\]

**注意：**请用之前复制的值替换Query_Key和Index_Name值。

14. 将.env文件的内容与以下内容添加。

> AZURE_OPENAI_ENDPOINT=https://agentic-
> @lab.LabInstance.Id.cognitiveservices.azure.com/
>
> AZURE_OPENAI_API_KEY=\<Replace with Azure OpenAI key\>
>
> AZURE_OPENAI_RESPONSES_DEPLOYMENT_NAME=gpt-4o-mini
>
> AZURE_OPENAI_API_VERSION=2025-03-01-preview

15. 将以下 Foundry 项目密钥变量添加到 .env 文件中。

> \# Azure AI Project Configuration
>
> AZURE_AI_PROJECT_ENDPOINT=**\<Microsoft Foundry endpoint\>**
>
> AZURE_AI_MODEL_DEPLOYMENT_NAME=gpt-4o-mini
>
> 从概览页面找到Microsoft Foundry项目端点，并用该值替换 **\<Microsoft
> Foundry endpoint\>**。
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image8.png)

![](./media/image9.png)

16. 完成后，将以下 App Insights 变量添加到同一个文件中。

> \# Observability and Monitoring Configuration
>
> APPLICATIONINSIGHTS_CONNECTION_STRING=**\<Connection string\>**
>
> ENABLE_OTEL=true
>
> ENABLE_SENSITIVE_DATA=true
>
> 从Azure门户打开应用洞察资源，复制连接字符串，并将**\<Connection
> string\>**替换为复制的值。
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image10.png)

17. 在.env文件中，添加以下内容，并添加你之前复制的Freshdesk的API密钥和账户URL。

> \# Freshdesk Configuration
>
> FRESHDESK_DOMAIN=\[Domain_URL\]
>
> FRESHDESK_API_KEY=\[API_Key\]

18. 最终的.env文件应该看起来像给的图片。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

19. 完成后，选择 **File** **(1)** ，然后点击 **Save** **(2)** 保存文件。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image12.png)

20. 选择......**（1）**顶部菜单中的扩展菜单选项。选择 **Terminal
    (2)** ，然后点击 **New Terminal (3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

21. 在**VS Code** Terminal中，运行Azure CLI登录命令:

+++az login+++

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image14.png)

22. 在 **Sign in** 窗口中，选择 **Work or school account** 并点击
    **Continue**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.png)

23. 在**“Sign into Microsoft** ” 标签页，使用以下凭证登录。

- 用户名 - <+++@lab.CloudPortalCredential(User1).Username>+++

- TAP - +++@lab.CloudPortalCredential(User1).TAP+++

24. 当被提示登录选项时，选择**“No, this app
    only** ”，这样可以继续，不链接其他桌面应用。

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image16.png)

25. 输入**1**，然后在“**Select a subscription and tenant**”中回车。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image17.png)

26. 终端打开后，执行命令，

> +++pip install -r requirements.txt+++ 以安装所有必需的包。

27. 请执行以下命令来测试搜索工具的工作原理。

+++python main.py+++

> ![A screenshot of a computer screen AI-generated content may be
> incorrect.](./media/image18.png)

## 任务2：可视化代理指标

在这个任务中，你将使用 Azure Application Insights
来可视化代理遥测数据。你将探索响应时间、路由准确性和工单创建成功的自定义指标。然后，你将构建交互式Azure
Monitor仪表盘，以显示关键绩效指标和趋势。这有助于识别瓶颈、衡量效率，并确保部署代理的实时健康运行。

1.  进入 Azure 门户，打开资源组，从资源列表中选择
    **agent-insights- **应用洞察资源。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.png)

2.  进入概览页面后，你可以看到显示的一些默认指标。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

3.  在左侧菜单中，选择 **Search (1)**，点击 **See all data in last 24
    hours (2)**。

![A screenshot of a search engine AI-generated content may be
incorrect.](./media/image21.png)

4.  打开后，从底部开始查看 **Traces (1)**，然后点击“**View as individual
    items (2)**”。 

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

5.  完成后，你将可以看到与经纪人的所有沟通细节，以及在指定时间范围内发生的所有交易。你也可以调整时间范围，探索更多。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

6.  探索和回顾这些翻译，你只需点击它们即可打开详细视图。查看如何查看所有细节，比如代理、消息和检索信息。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

7.  接下来，选择 **Failures (1)**，审查失败 **requests
    (2)** ，以集中视图查看所有失败执行，并通过详细的跟踪分析找出根本原因。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

8.  接下来，选择 **Performance (1)** ，检查
    **作和响应时间（2），**由此可以确定代理的性能SLA。 

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

9.  现在，在左侧菜单的监控中选择**“Metrics**”。你可以探索通过 SPAN
    发布的自定义指标。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.png)

10. 选中后，在 **Metric Namespace （1）** 下，选择
    azure.applicationinsights **（2）。**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.png)

11. 现在，在指标中，选择 **gen_ai.client.operation.duration and set the
    aggregation to avg (1)**。查看 **line chart (2)** 以查看 **Response
    Time** 指标，代理回复用户时采用了哪个指标。

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image30.png)

12. 同样地，选择 **gen_ai.client.token.usage and set the aggregation to
    avg (1)**。查看 **line chart (2)** ，查看代理的代币使用情况。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

13. 接着，从左侧菜单选择 **Logs (1)** ，取消 **Queries hub (2)** 面板。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image32.png)

14. 关闭后，点击 **tables** 选项，将鼠标悬停在 **customMetrics**
    参数上，你会看到一个**Run** 选项，点击它。 

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

![A close-up of a message AI-generated content may be
incorrect.](./media/image34.png)

15. 查询成功运行后，你会看到下面列出的所有自定义指标作为查询结果。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image35.png)

16. 接下来，从左侧菜单选择“**Workbooks (1)** ”，点击快速开始下的“**Empty
    (2)** 工作簿”。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.png)

17. 打开后，点击 **+ Add (1)** ，然后选择 **Add metric (2)**。

![A screenshot of a phone AI-generated content may be
incorrect.](./media/image37.png)

18. 打开公制面板后，点击**“Add metric”**选项。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

19. 现在，选择**Metric** 为 gen_ai.client.token.usage**（1）**，将
    **Display name** 作为令牌使用量**（2）**，点击 **Save (3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.png)

20. 再次点击 **Add metric** 选项。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

21. 现在，选择 **Metric** 为 gen_ai.client.operation.duration
    **（1）**，将 **Display name**设置为响应时间**（2）**，点击 **Save
    (3)**。

![A screenshot of a screenshot of a metric settings AI-generated content
may be incorrect.](./media/image40.png)

22. 选中这两个指标后，点击“**Run Metrics**”。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.png)

23. 现在把**可视化**改成**面积图**，获得类似的可视化效果。你可以探索许多其他可视化方式，以及时间范围。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.png)

24. 编辑完成后，点击**“Done
    editing**”。这样可以把这张卡保存到你的练习册里。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.png)

25. 现在，再次点击 **+ Add (1)** ，然后选择 **Add query (2)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.png)

26. 在查询窗格中，添加以下 **query (1)**，并点击 **Run Query (2)**。

+++customMetrics+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.png)

27. 查询成功运行后查看结果。审核完成后，点击 **Done Editing**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image46.png)

28. 完成后，点击顶部菜单中的“**Done editing (1)** ”，然后点击“**Save
    (2)**”图标。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image48.png)

29. 在“Save As”面板上，输入“标题为代理工作簿**（1）**，然后点击 **Save
    As (2)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image49.png)

30. 由于这是实验室环境，可用数据可能有限，难以进行全面监测。不过，你可以通过添加客服的自定义指标，并创建专门针对特定目标的监控仪表盘来提升可见性，例如以下内容:

- **代理性能仪表盘**

> **显示的指标:**

- 代理响应时间（平均，P95）

- 按代理类型划分的成功率

- 请求量趋势

- 错误率警报

> **商业问题解答:**

- 哪些代理表现最好？

- 我们是否达成了SLA目标？

- 是什么导致了系统变慢？

&nbsp;

- **用户体验仪表盘**

> **显示的指标:**

- 端到端请求延迟

- 工单生成率

- 知识检索成功

- 用户满意度代理指标

> **商业问题解答:**

- 用户回复是否很快？

- 请求多久会变成支持工单？

- 知识库对用户有帮助吗？

## 任务3：监控Foundry门户中的代理特定指标

在这个任务中，你将使用 Azure Application Insights
来可视化代理遥测数据。你将探索来自 Microsoft Foundry
门户的定制代理特定指标。

1.  既然你已经将 Application Insights 连接到 Microsoft Foundry
    门户，你可以返回 Foundry 门户，直观地可视化代理的工作过程。

2.  从资源列表中返回你的资源组，选择**agent-foundry**资源。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.png)

3.  在下一页，点击**“Go to Foundry
    portal**”。现在，您将被引导到Microsoft
    Foundry门户，在那里创建您的第一个代理。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image51.png)

4.  在测试代理之前，连接 Application
    Insights，以启用详细日志和跟踪可视化。

5.  在 Microsoft Foundry 门户中，从左侧菜单选择 **Monitoring
    (1)** ，选择 **agent-insights- (2)** ，点击 **Connect (3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image52.png)

6.  现在，进入你之前连接过应用洞察的 **Monitoring** 面板，选择
    **Resource usage** 标签，查看所有指标和数值。 

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image53.png)

7.  从左侧菜单选择 **Tracing (1)** ，点击任一的 **Trace
    (2)**，查看代理互动的详细跟踪。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image54.png)

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image55.png)

**摘要**

在这个实验室里，你为企业代理配置了可观察性和监控功能。通过OpenTelemetry追踪，你捕捉了每个工作流步骤的详细执行数据，并通过与Azure应用洞察集成，创建了用于可视化性能指标和座席健康状况的仪表盘。

你已经成功完成了这个实验。请点击“Next \>\>”继续。
