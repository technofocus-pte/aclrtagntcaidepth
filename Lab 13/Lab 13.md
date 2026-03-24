# 实验13：利用人机环路AI实施企业欺诈检测

**预计时长**：60分钟

**概述**

您是Contoso有限公司的AI工程师，负责实施人机参与（HITL）AI工作流程。在本实验室中，您将探索Contoso欺诈检测与响应工作流程，AI代理分析可疑活动，并将高风险行为转交人工分析师审核，同时使用实时React +
FastAPI仪表盘进行监控和交互。

实验室目标

你将在实验室执行以下任务。

- 任务一：利用Azure Agent Framework实现人机参与AI工作流程

## 任务0：设置代码 

1.  从 C：\Labfiles\Day 3 中，解压 **OpenAIWorkshop-Framework** 文件。

2.  点击 LabVM桌面上的 **Visual Studio Code。**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

3.  选择 **File** **(1)**并点击 **Open
    Folder** **(2)** 以打开**OpenAIWorkshop-Framework**文件夹。 

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

4.  进入 C：\Labfiles\Day 3\\**OpenAIWorkshop-Framework** 路径，选择
    **OpenAIWorkshop-Framework**，然后**选择Folder**。

5.  选择**“Yes, I trust the authors。”**

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image3.png)

6.  点击**省略号（...）（1）**然后是 **Terminal** **(2)** ，然后是 **New
    Terminal** **(3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

7.  输入以下命令，进入**应用程序**目录，并从 **pyproject.toml /
    uv.lock** 文件安装所有必需的依赖。

> cd agentic_ai/applications
>
> uv sync

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

**注意：**如果遇到任何错误，请执行以下命令

> +++pip install uv+++

+++uv sync+++

8.  该指令可能需要5到10分钟完成。 **与此同时，你可以继续进行任务1**。

## 任务1：利用Azure Agent Framework实现人机参与AI工作流程

在本实验室中，您将为Contoso的欺诈检测系统实施人工参与（HITL）工作流程。你将运行多代理欺诈检测，审核高风险警报，做人工决策，并实时可视化React +
FastAPI仪表盘的工作流程。

1.  在Visual Studio Code中，展开 **agentic_ai (1) \> workflow (2)\>
    fraud_detection
    (3)**，选择**fraud_detection_workflow.py（4）。**查看代码**（5）**。

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image6.png)

2.  在**fraud_detection（1）**下，右键点击**.env.sample（2），**然后选择
    **Rename (3)。**

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image7.png)

3.  将重命名为 .env，然后点击它打开文件。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image8.png)

4.  用你在上一个实验中复制的实际数值替换AZURE_OPENAI_API_KEY**（1）**和AZURE_OPENAI_ENDPOINT**（2）**的数值。

5.  把AZURE_OPENAI_CHAT_DEPLOYMENT加成**gpt-40-mini（3）**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.png)

- 进入 **Microsoft Foundry** 门户，选择 **Overview** **(1)**，选择
  **Azure OpenAI （2）**。复制 **Azure OpenAI key** **(3)**  和 **Azure
  OpenAI endpoint** **(4)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.png)

6.  选择 **File** **(1)** ，然后选择 **Save** **(2)**。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

7.  在Visual Studio代码窗口中，点击**省略号（...）（1）**然后是
    **Terminal** **(2)** ，然后是 **New Terminal** **(3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

8.  执行以下命令。

> cd mcp
>
> uv run python mcp_service.py

9.  让命令运行，打开一个新的终端。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

10. 请输入以下命令，用命令行运行工作流程。

> cd agentic_ai/workflow/fraud_detection
>
> uv run python fraud_detection_workflow.py
>
> ![A black screen with white text AI-generated content may be
> incorrect.](./media/image12.png)

**注意**：该命令可能需要5到10分钟完成。请等它结束。

11. 示例包含三个示例警报:

    - **警报1：多国登录**（严重程度高）

    - alert_id: "ALERT-001"

    - customer_id: 1

    - alert_type: "multi_country_login"

    - 描述：“美国和俄罗斯在2小时内尝试登录。”

严重程度：“高”

- **警报2：数据激增**（中等严重度）

- alert_id: "ALERT-002"

- customer_id: 2

- alert_type: "data_spike"

- 描述：“过去24小时内数据使用量增加了500%。”

严重程度：“中等”

- **警报3：异常指控**（严重程度高）

- alert_id: "ALERT-003"

- customer_id: 3

- alert_type: "unusual_charges"

- 描述：“三笔大额购买，总共5000美元，耗时10分钟。”

严重程度：“高”

12. 一旦运行成功，你可以看到下面的终端。根据风险严重度选择行动。如果风险严重度≥0.6，则需进行人工审核。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

13. 由于风险严重性较高，您可以输入2来锁定客户账户**（1）。**

    - 分析师笔记：三项分析均确认高风险。立即行动：锁定账户以防止未经授权访问。**（2）**

    - 输入分析师ID（默认：analyst_cli）：按**Enter （3）**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

14. 一旦工作流程完成，你会收到这样的输出。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.png)

15. 命令成功后，**删除所有现有的正在运行的终端会话**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image16.png)

## Contoso欺诈检测与响应工作流程的实时工作流程可视化界面

您将使用实时工作流程可视化工具界面来监控并与Contoso欺诈检测与响应工作流程交互。您将启动所有服务（MCP服务器、后端、前端），选择示例提醒，观察实时工作流执行，审查高风险欺诈警报，提交分析师决策，并实时监控事件流。

1.  开一个新航站楼。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

2.  启动所有服务（3 terminal）:

    - Terminal 1 - MCP服务器:

> cd mcp
>
> uv run mcp_service.py

- Terminal 2 - FastAPI 后端:

> cd agentic_ai/workflow/fraud_detection
>
> uv run --prerelease allow backend.py
>
> ![A screen shot of a computer program AI-generated content may be
> incorrect.](./media/image17.png)

- Terminal 3 - React前端:

> cd agentic_ai/workflow/fraud_detection/ui
>
> npm run dev
>
> **注意**：如果出现任何错误，先执行 +++npm install+++
> 命令，然后重新运行 +++npm run dev+++ 命令。
>
> ![A computer screen with white text AI-generated content may be
> incorrect.](./media/image18.png)

- **ctrl + 点击** http://localhost:3000 在浏览器中打开应用

> ![A screen shot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

3.  查看实时工作流程可视化界面。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

4.  您可以通过“**Select Alert**”下拉菜单查看示例警报 。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image21.png)

**注意**：只有在第二个终端（backend.py）连接打开后，你才能通过下拉菜单看到警报。确保连接是开启的。

5.  **选择警报**：从3个示例警报中选择（ALERT-001、ALERT-002、ALERT-003）（**1）**

    - 点击 **Start Workflow (2**开始处理

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

6.  **观看实时更新**: 节点在执行器运行时会变色

    - 🔵 蓝色 = 运行

    - 🟢 绿色 = 完工

    - ⚪ 灰色 = 闲置

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

7.  **分析师审查**：当发现高风险欺诈时，会出现审查小组。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

8.  **提交决策**：选择动作并添加备注

    - 你的决定：如果严重程度较高，选择 **Lock Account (1)**

    - 分析师注：输入“高风险”，三项分析均确认。立即行动：锁定账户以防止未经授权访问。**（2）**

    - 选择 **SUBMIT WORKFLOW (3)**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

9.  **监控事件**：右侧面板显示完整的事件流。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

**摘要**

在本实验室中，你实施了利用Azure Agent
Framework实现了欺诈检测的人工流程（HITL）。你探讨了 AI
代理如何分析可疑活动，将高风险案件转交给人工分析师，以及如何与实时React +
FastAPI仪表盘交互以监控工作流程执行和提交决策。
