# 大规模管理、保护和监控AI代理

**概述**

该实践实验室专注于利用Azure AI Agent Service SDK和Microsoft
Foundry大规模管理、保护和监控AI代理。参与者将深入探讨企业AI部署中必不可少的生产级实践，首先是AgentOps——通过OpenTelemetry集成和Azure应用洞察来观察和管理AI代理的学科。
研讨会通过可配置的内容安全过滤器，通过检测和阻止仇恨言论、暴力和敏感信息等有害输出，实施Microsoft的六大基本原则，包括公平、可靠性、隐私和问责制，强调负责任
AI
的重要性。此外，参与者还将构建复杂的人工参与（HITL）工作流程，例如一个欺诈检测系统，专业AI代理分析可疑活动，并将高风险案件智能地引导至人工分析师，以实现关键决策。在整个实验室中，您将使用多代理系统，这些系统在检索、验证和编排任务中协作，积累端到端追踪、自定义指标可视化、性能监控仪表盘和实时工作流程管理的实践经验。到研讨会结束时，参与者将掌握在企业环境中部署、监控和管理
AI
代理的关键技能，确保其在大规模安全、伦理且高效运行的同时，遵守组织政策和监管要求。

**目标**

到这个实验结束时，你就能:

- **启用可观察性和监控**:
  利用OpenTelemetry集成Azure应用洞察，为AI代理实现端到端追踪和遥测，捕捉代理行为、性能指标和执行跟踪

- **可视化代理指标**: 在 Application Insights
  中创建自定义仪表盘和工作簿，实时监控代理性能、响应时间、令牌使用情况、路由准确性和系统健康状况

- **实施 Responsible AI 实践**: 在 Microsoft Foundry
  中配置内容安全过滤器，以检测并阻止有害输出（仇恨言论、暴力、敏感内容），并确保
  AI 行为符合伦理和合规

- **构建人机作流程**:
  设计并部署欺诈检测系统，让AI代理分析警报，并将高风险案件转交给人工分析师进行审核和决策

- **监控多智能体系统**:
  跟踪代理间通信，追踪多个专业代理间分布式工作流，并识别复杂代理编排中的瓶颈或故障

组件说明

- **Microsoft Foundry**:
  一个基于云的平台，用于开发、部署和管理具有集中治理、可观察性和合规功能的AI模型，适用于企业AI应用。

- **Azure AI Hub**: 顶级 Azure
  资源为团队提供一个集中、安全且协作的环境，以构建、管理和部署 AI
  应用，共享资源和治理策略。

- **Azure AI 搜索**:
  一项基于矢量的搜索服务，通过索引和检索相关文档，实现检索增强生成（RAG），以提升基于信息的AI生成响应。

- **Azure AI 服务**: 一组基于云的 AI
  服务，提供预构建且可定制的视觉、语言、语音和决策能力的API和模型。

- **OpenTelemetry**:
  一个面向分布式追踪、指标和日志的开放标准，原生集成于 Microsoft Agent
  Framework，用于捕捉代理执行跟踪、性能指标和错误跟踪。

- **内容安全过滤器**: Microsoft Foundry
  内置过滤系统，能够自动检测并屏蔽仇恨言论、暴力、性内容和敏感信息（PII）等类别的有害输出。

- **LLMs与嵌入**：大型语言模型提供自然语言理解和生成，而嵌入则是用于文本相似度、搜索和知识检索的向量表示，应用于AI应用。

# 实验10：先决条件——建立知识索引和工单系统

**预计时长**：30分钟

**概述**

在这个先修实验室中，你将建立一个由 AI
驱动的工作流程所需的基础组件，能够检索企业知识并自动创建支持工单。重点是准备一个可搜索的知识库，使AI代理能够使用MCP（模型上下文协议）工具查询这些知识，并集成工单系统以实现下游作。

通过完成这些任务，您将建立核心基础设施，使客服能够:

- 从索引数据中检索相关信息

- 在对话或工作流程中结合上下文运用这些信息

- 通过在外部服务中创建工单来升级问题

这种安排确保后续实验室顺利运行，反映真实的企业场景。

实验室目标

你将在实验室执行以下任务。

- 任务1：准备知识索引

- 任务2：设置Freshworks用于工单管理

## 任务1：创建Azure资源

在这个任务中，你将创建完成该实验室所需的所有Azure资源。

### 任务1.1：创建存储账户

1.  使用以下凭据登录 Azure 门户 +++https://portal.azure.com+++
    并选择存储账户。

- 用户名 - +++@lab.CloudPortalCredential(User1).Username+++

- TAP - <+++@lab.CloudPortalCredential(User1).TAP>+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image1.png)

2.  选择 **Create**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

3.  输入以下信息，选择 **Review + create**。在下一界面选择“Create”。

- 存储账户名称 - +++aistorage@lab.LabInstance.Id+++

- 首选存储类型 – 选择 **Azure Blob Storage or Azure Data Lake Storage
  Gen2**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image3.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image4.png)

4.  资源创建后，选择 **“Go to resource**”。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

5.  选择 **Upload**，选择**Create new**容器以创建新容器。命名为
    +++**datasets**+++，然后选择 **Ok**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

![A screenshot of a login box AI-generated content may be
incorrect.](./media/image7.png)

6.  选择“**Browse for files**”，从**C:\Labfiles\Day
    2**中选择策略文件，点击 **Upload**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

![A screenshot of a upload box AI-generated content may be
incorrect.](./media/image9.png)

现在，存储账户已成功创建并加载了策略文档。

### 任务1.2：创建Foundry资源

在此任务中，您将创建一个 Foundry 资源，访问 Microsoft Foundry 是必要的。

1.  在Azure门户（+++https：//portal.azure.com+++）主页，选择
    **Foundry**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.png)

2.  从左侧窗格选择**Foundry**，然后选择**Create** Foundry资源。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

3.  输入以下信息，选择 **Review + create**。

- 名称 – <+++agentic-@lab.LabInstance.Id>+++

- 默认项目名称 – <+++agentic-ai-project-@lab.LabInstance.Id>+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.png)

4.  验证后选择**Create**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

5.  确保资源已经建立。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

6.  打开 **<agentic-ai-project-@lab.LabInstance.Id>** ，选择 **“Go to
    Foundry portal**”。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.png)

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image16.png)

7.  在 Microsoft Foundry 中，从左侧面板选择 Models + 端点。选择 +
    **Deploy model** -\> **Deploy base model**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.png)

8.  搜索 +++gpt-4o-mini+++，选择并点击确认以部署该模型。

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image18.png)

9.  在部署窗口中选择 **Deploy**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.png)

10. 同样，搜索 +++text-embedding-ada-002+++ 并部署。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image21.png)

在这个任务中，你已经成功创建了Foundry资源，并在其中部署了一个聊天和一个嵌入模型。

### 任务1.3：创建应用洞察

在此任务中，您将创建应用洞察资源，这是监控所必需的。

1.  在Azure门户的主页，选择 **“Subscriptions”** 并选择分配的订阅。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

2.  从左侧面板选择 **Resource providers**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

3.  搜索 +++Operational+++，选择 **Microsoft.OperationalInsights** 旁的
    3 个点，点击 **Register**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

4.  在 Microsoft Foundry 的左侧面板中，选择 **“Monitoring**”。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

5.  选择 **Create New** -\>，输入名称为 [as
    <+++agent-insights-@lab.LabInstance.Id>+++，然后选择](mailto:+++agent-insights-@lab.LabInstance.Id+++，然后选择)
    **Create**。

![A screenshot of a application AI-generated content may be
incorrect.](./media/image27.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.png)

在这个任务中，你创建了应用洞察资源。

### 任务1.4：创建搜索资源

在AI代理能够准确回答企业问题之前，必须访问可信的数据源。Azure AI
搜索通过索引政策、合同和手册等文档，实现检索增强生成（RAG）。索引就像一个可搜索的目录，将内容拆分成块，添加元数据，并使代理在对话中检索到正确的信息。

在此任务中，利用 Azure AI Search 索引上传的文档，创建可搜索的知识库。

1.  在 Azure 门户的主页，选择 **Foundry**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.png)

2.  从左侧窗格选择**“AI Search**”，然后选择 **+ Create。**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

3.  输入以下信息，选择 **Review + create**。

- 服务名称 - +++ai-knowledge-@lab.LabInstance.Id+++

- 地区 - East US2

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image32.png)

4.  验证通过后选择 **Create**。创建资源后选择“Go to resource”。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image34.png)

5.  选择 **Import data (new)** 。

![A screenshot of a search engine AI-generated content may be
incorrect.](./media/image35.png)

6.  在“**Azure Blob Storage**”下选择 **Choose data source**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.png)

7.  在下一格，选择**RAG**选项，因为我们正在构建基于检索的代理。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image37.png)

> 以下是这些选项的用途 -

1.  **关键词搜索：**用于基于精确关键词的传统搜索体验。它会索引文本，让用户通过关键词匹配找到信息，无需AI推理。

2.  **RAG（检索增强生成）：**结合文档检索与 AI
    生成。它会接收文本（以及简单的OCR图像），因此AI代理能够提供扎实、具上下文感知的回答。

3.  **多模态RAG：**扩展RAG以处理复杂的视觉内容，如图表、表格、工作流程或图表。它使
    AI 能够解读文本和视觉元素，提供更丰富、基于洞察的回答。

&nbsp;

8.  在 **datasets** **under Blob containe**r 中的 **Storage account**
    下选择 <aistorage@lab.LabInstance.Id>，然后选择“**Next**”。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

9.  请选择以下详情，然后选择 **“Next**”。

- Kind – Azure AI Foundry （预览版）

- Azure AI Foundry/Hub 项目 – <agentic-ai-project-@lab.LabInstance.Id>

- 模型部署 – text-embedding-002-ada

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.png)

10. 在接下来的界面选择“**Next**”，直到出现 **Review and create** 界面。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image40.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.png)

11. 在“**Review and create**”界面选择 **Create**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.png)

12. 在“创建成功”对话框中选择 **Close**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.png)

你已经成功将数据集导入 Azure AI
搜索并创建了可搜索索引。在下一个任务中，你将创建一个 AI
代理，并将该索引作为其知识来源连接起来。

# 任务2：设置Freshworks用于工单管理

在此任务中，您将搭建并配置Freshworks，以启用工单管理和多代理系统的企业集成。

**Freshworks**
是一个基于云的客户服务和互动平台，旨在提升客户支持运营和用户满意度。它提供一套工具，包括工单管理、在线聊天、帮助中心创建和客户自助服务。Freshworks
支持全渠道通信，使企业能够通过集中界面管理电子邮件、聊天、电话和社交媒体上的客户互动。其自动化功能有助于简化工作流程、分配工单，并提供绩效跟踪的分析。现在你要创建Freshworks账户。

1.  复制URL并粘贴到虚拟机内浏览器的新标签页，打开**Freshworks**门户。

    - URL:

> +++https://www.freshworks.com/freshdesk/lp/home/?tactic_id=3387224&utm_source=google-adwords&utm_medium=FD-Search-Brand-India&utm_campaign=FD-Search-Brand-India&utm_term=freshdesk&device=c&matchtype=e&network=g&gclid=EAIaIQobChMIuOK90qvLjQMV_dQWBR3JAi9VEAAYASAAEgK87_D_BwE&audience=kwd-30002131023&ad_id=282519464145&gad_source=1&gad_campaignid=671502402+++

2.  在门户中，选择 **Start free trial** 即可开始免费试用。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.png)

3.  在下一页输入这些信息，并点击“**Try it free (6)”**:

    - **名字:** LODS

    - **姓氏:** User1

    &nbsp;

    - **工作邮箱:** **+++@lab.CloudPortalCredential(User1).Username+++**

    &nbsp;

    - **公司名称:** Zava

    - **组织规模:** 选择**1-10**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image45.png)

4.  在下一栏填写这些信息，点击**“Next (4)”**:

    - **What industry are you from ?:** 从列表中选择**Software and
      internet (1)**

    - **How many employees are there in your company?:** 选择 **1-10
      (2)**

    - 选择 **I'm trying customer service software for the first time
      (3)**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image46.png)

5.  完成后，复制给出的URL，粘贴到虚拟机内浏览器的新标签页中打开**Outlook**。

    - URL:

> +++https://go.microsoft.com/fwlink/p/?LinkID=2125442&clcid=0x409&culture=en-us&country=us+++

6.  在“pick an account”面板中，选择你被分配给这个实验的账户。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.png)

7.  在Freshworks验证邮件中，打开并点击“**Activate Account**”。

> ![A screenshot of a computer screen AI-generated content may be
> incorrect.](./media/image48.png)

**注意：**如果您找不到Freshworks的激活邮件，请稍等几分钟，因为邮件发送可能会有延迟。如果邮件过了一段时间还没到，可以考虑在新的私密/无痕窗口重新激活免费试用的步骤。另外，检查垃圾邮件或垃圾邮件文件夹，因为邮件可能被过滤到了那里。

8.  在下一栏， **Enter password (1)** ，**Confirm password (2)**
    输入相同的密码。点击 **Activate your account (3)**。

> ![A screenshot of a login screen AI-generated content may be
> incorrect.](./media/image49.png)

9.  进入门户后，点击右上角的 **Profile (1)** 图标，选择 **Profile
    settings (2)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.png)

10. 在个人资料页面，点击“**View API Key** ”即可获取 API 密钥。

![A screenshot of a web page AI-generated content may be
incorrect.](./media/image51.png)

**注意：**如果您找不到该选项，请使用**CTRL + -**来最小化屏幕大小。

11. 在下一格，填写 **CAPTCHA**。

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image52.png)

12. 请将API Key 复制到记事本，你将会继续使用它。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image53.png)

13. 请在浏览器标签页复制显示的
    **账户URL**，并将数值复制到记事本。你还会继续使用它。

![](./media/image54.png)

**摘要**

完成这个先修实验，你为端到端的代理工作流程奠定了基础。你准备了一个可搜索的知识索引，使客服通过基于**Azure
AI Search**的MCP工具查询这些数据，并集成
**了Freshworks**实现自动化工单管理。

这一基础确保代理能够获取准确的上下文，做出明智决策，并高效升级问题，为即将到来的实验室中更高级的代理驱动场景做好准备。

你已经成功完成了这个实验。请点击“Next \>\>”以继续阅读
