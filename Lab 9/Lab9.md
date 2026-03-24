# 实验9：利用Azure AI框架实现单代理和多代理工作流程

**预计时长**：45分钟

**概述**

您是Contoso有限公司的AI工程师，负责利用Azure
AI框架开发智能代理工作流程。在本实验室中，你将创建一个单代理系统，通过MCP与外部工具集成，然后设计多代理工作流程，使多个专业代理根据用户意图动态协作或交付任务。

实验室目标

你将在实验室执行以下任务。

- 任务1：构建并测试一个Azure OpenAI聊天代理

- 任务2：创建带有工具集成的单代理工作流程

- 任务3：多代理工作流程设计

  - 任务3.1：协调多智能体工作流程

  - 任务3.2：切换模式多智能体系统

## 任务0：实验室环境搭建

1.  从 C：\Labfiles\Day 2 中，解压 **OpenAIWorkshop-Framework** 文件。

2.  点击 LabVM桌面上的**Visual Studio Code** 。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

3.  选择 **Open Folder** **(2)** 并点击 **Open
    Folder** **(2)** 以打开**OpenAIWorkshop-Framework**文件夹。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

4.  进入 C：\Labfiles\Day 2\\**OpenAIWorkshop-Framework** 路径，选择
    **OpenAIWorkshop-Framework**，然后**选择文件夹**。

5.  选择**“Yes, I trust the authors”**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

6.  点击 **省略号（...）（1）**然后是 **Terminal** **(2)**，然后是 **New
    Terminal** **(3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

7.  输入以下命令，导航到**应用程序**目录，**并从requirements.txt**文件安装所有必需的依赖
    。

> cd agentic_ai/applications
>
> pip install -r requirements.txt

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image5.png)

8.  该指令可能需要5到10分钟完成。

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image6.png)

## 任务1：构建并测试一个Azure OpenAI聊天代理

在这个任务中，你将用Visual Studio Code构建并测试一个简单的Azure
OpenAI聊天代理。你将配置环境变量，将代理连接到已部署的模型，并观察它如何根据不同提示生成动态响应。

1.  返回**Visual** **Studio Code**。

2.  确保 pip install -r requirements.txt
    命令已成功完成。如果还在运行，请等它结束。

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image6.png)

3.  从**Explorer**中展开 **agentic_ai** **(1)
    \>** **applications** **(2)**。右键点击 .env.sample **（3）** 和
    **Rename （4）** 。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

4.  把文件重命名为.env，然后点击它打开文件。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

5.  将AZURE_OPENAI_API_KEY**（1）**和AZURE_OPENAI_ENDPOINT**（2）**的值替换为实际值。从Microsoft
    Foundry **Overview**页面获取。

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image9.png)

6.  把AZURE_OPENAI_CHAT_DEPLOYMENT 加成 **gpt-40-mini（3）**

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image10.png)

7.  选择 **File (1)**，然后选择 **Save(2)**。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

8.  右键点击 **application** **(1)** 文件夹，然后点击 **New
    file** **(2)** 创建新文件，配置一个简单的代理。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.png)

9.  将代理文件命名为 +++simple_agent_test.py+++。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

10. 复制并粘贴以下代码到文件中。

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

11. 选择 **File** **(1)** ，然后选择 **Save** **(2)**。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

12. 右键点击**simple_agent_test.py（1）**，然后选择 **Open in Integrated
    Terminal** **(2)**中打开。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image15.png)

13. 执行以下命令运行代理并观察输出，以了解代理的工作原理。

+++python simple_agent_test.py+++

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image16.png)

14. 我们修改指令，观察代理的反应。将指令提供“Tell me a joke about the
    Earth”
    **(1)** （第31行），然后**Save** 文件。然后执行下面的命令**（2），**并查看代理的响应**（3）。**

+++python simple_agent_test.py+++

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image17.png)

15. 这表明代理的响应会根据所提供的指令而变化，凸显其适应不同提示的能力。

## 任务2：创建带有工具集成的单代理工作流程

在此任务中，您将构建并测试一个单代理工作流程，并与使用MCP（模型上下文协议）的外部工具集成。你将配置环境变量，本地运行MCP服务器、后端和前端，并观察代理如何利用MCP工具处理用户查询，提供智能且具上下文感知的响应。

1.  在Visual Studio Code中，展开 **agents** **(1)
    \>** **agent_framework** **(2)
    \>** **single_agent** **(3)** ，并查看集成MCPStreamableHTTPTool工具**（4）**的单代理工作流程。

    - MCPStreamableHTTPTool 允许代理通过 MCP 服务器调用基于 HTTP
      的外部服务，并在对话中包含工具输出。

    - 传递到ChatAgent，并根据指令和用户提示自动使用。

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image18.png)

2.  仔细阅读代码，了解它是如何集成的:

    - 在_maybe_create_tools法中:

> ![A screen shot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

- 这会创建一个可流式的 HTTP 工具，连接到你的 MCP 服务器。

- 它允许代理通过MCP向外部服务发送HTTP调用，作为其工作流程的一部分。

&nbsp;

- 工具在初始化时传递给ChatAgent:

> ![A screen shot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

- 当用户提示触发工具调用时，代理就可以使用该工具。

- WebSocket
  的流媒体支持：当在流式对话中调用工具/功能时，它会通过_chat_async_streaming广播工具名称和转机。

3.  进入 .env 文件（1），在 .env 文件中添加以下 Environment
    变量以指定运行 **Single agent workflow**  **(2)** :

+++AGENT_MODULE=agents.agent_framework.single_agent+++

- 添加 DISABLE_AUTH=true **（3）**
  环境变量，用于禁用应用程序中的认证。它使本地开发和测试更加便捷。

> +++DISABLE_AUTH=true+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

4.  选择 **File (1)** ，然后选择 **Save(2)**。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

5.  现在你将启动**MCP服务器、后端**和**React前端**，在本地运行完整的代理环境，允许UI与代理和工具交互。

6.  在Visual Studio代码窗口中，点击**省略号（...）（1），**然后是
    **Terminal (2)**，然后是 **New Terminal (3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

7.  等待上一步完成后，再进行下一步。

8.  **启动MCP服务器（Terminal 1）**: (mcp directory is at project root
    level)

    - 执行以下命令启动**MCP服务器**，该服务器会暴露代理可以调用的API，作为工具调用。
      (服务器运行在 [http://localhost:8000](http://localhost:8000/))

> cd mcp
>
> uv run python mcp_service.py
>
> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image21.png)
>
> 注意：如果遇到任何错误，请执行以下命令:

+++pip install uv+++

+++uv run python mcp_service.py+++

9.  让命令运行，打开一个新的 terminal。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

10. **启动后端（Terminal 2）**:

    - 执行以下命令启动托管代理工作流、会话管理和API端点的后端服务器。

> cd agentic_ai/applications
>
> uv run python backend.py

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image22.png)

- 本地运行于: [http://localhost:7000](http://localhost:7000/).

- 这是前端通信的核心应用逻辑。确保**连接是开启**的。

> ![A screen shot of a computer AI-generated content may be
> incorrect.](./media/image23.png)

11. 让命令运行，打开一个新的 terminal。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

12. **启动React前端（Terminal 3）**:

    - 输入下面给出的命令，导航到 react-frontend 目录。

> +++cd agentic_ai/applications/react-frontend+++

- 输入以下命令即可启动代理界面的**React前端**。提供用户界面，可与代理互动并实时查看其响应。

> +++npm start+++

- 编译可能需要一些时间。请忽略警告，等待它完成。一旦**webpack成功编译完成**，代理应用程序将在本地运行于: [http://localhost:3000](http://localhost:3000/).

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image24.png)

13. 当三个终端都运行时，代理应用会在你的浏览器中启动，你可以用它与代理交互并测试其功能。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

**注意**：确保三个终端均在运行。如果有停止的指令，请重新执行相应的命令。如果这三个都不激活，可能会遇到连接错误。

14. 在聊天中发送以下提示（**1），**查看回复（**2）**:

+++Customer 251, what's my billing summary?+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

**注意**：确保三个终端均在运行。如果有停止的指令，请重新执行相应的命令。如果这三个都不激活，可能会遇到连接错误。

15. 查看输出，是 ChatAgent（self.\_agent）解释了提示，可能称为 **MCP
    工具**，并生成了输出。

    - 客服将您的请求理解为对**客户251的账单查询**。

    - 它使用 **MCP 工具**获取结构化计费数据。

    - 该智能体按预期工作——它动态集成工具输出和 AI
      推理，以回答用户特定的问题。

16. 完成测试后，返回 VS Code
    并终止所有正在运行的终端会话。这确保了即将到来的多智能体工作流程无干扰地运行。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

## 任务3：多代理工作流程设计

在这项任务中，你将设计和实施先进的多代理工作流程，展示不同的协调模式。您将首先通过中央管理器协调多个专业代理，协同处理复杂查询，然后探索基于切换的系统，该系统根据用户意图动态切换控制权。

### 任务3.1：协调多智能体工作流程

在这项任务中，你将协调一个多代理工作流程，中央编排者协调多个专业代理，协作处理复杂的用户查询，生成准确且基于工具的响应。

1.  导航到 **agent (1) \> agent_framework (2) \> multi_agent (3) \>
    magentic_group (4)**并查看代码**(5)**。 

    - 该代码代表了**多代理编排**框架，因为它定义了一个系统，多个专业代理在中央编排器的指导下协作。

> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image28.png)

- \_create_participants
  初始化多个专业代理（CRM/计费、产品/促销、安全/认证）。

- 每个代理人:

  - 有特定的领域和工具。

  - 只与编排器通信，不直接与用户沟通。

  - 提供事实性、工具支持的回答。

- 以下是多代理工作流程中使用的代理

  - **CRM与计费代理**–
    利用事实工具支持的数据处理客户账户、订阅、账单、发票、付款及相关查询。

  - **产品与促销代理**——通过结构化资源提供产品供应情况、促销、折扣、资格和条款。

  - **安全与认证代理**——通过日志和工具管理安全事件、认证问题、账户锁定及风险缓解指导。

2.  导航到.env文件**（1）**，注释单个代理变量（2），然后输入以下命令添加**编排多代理**变量**（3）**。

+++AGENT_MODULE=agents.agent_framework.multi_agent.magentic_group+++

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image29.png)

3.  选择 **File (1)** ，然后选择 **Save(2)**。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

4.  现在，按照步骤启动完整的代理应用，启动其三个核心组件:

5.  在Visual Studio代码窗口中，点击**省略号（...）（1），**然后是
    **Terminal (2)**，然后是 **New Terminal (3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

6.  **启动MCP服务器（Terminal 1）**: (MCP 目录位于项目根级)

    - 执行以下命令启动**MCP服务器**，该服务器会暴露代理可以调用的API，作为工具调用。
      (服务器运行在 [http://localhost:8000](http://localhost:8000/))

> cd mcp
>
> uv run python mcp_service.py

7.  让命令运行，打开一个新的终端。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

8.  **启动后端（Terminal 2）**:

    - 执行以下命令启动托管代理工作流、会话管理和API端点的后端服务器。

> cd agentic_ai/applications
>
> uv run python backend.py

- 这是前端通信的核心应用逻辑。确保**连接是开启**的。

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image23.png)

9.  让命令运行，打开一个新的终端。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

10. **启动React前端（Terminal 3）**:

    - 输入下面给出的命令，导航到 react-frontend 目录。

> +++cd agentic_ai/applications/react-frontend+++

- 输入以下命令即可启动代理界面的**React前端**。提供用户界面，可与代理互动并实时查看其响应。

> +++npm start+++

- 一旦**webpack成功编译完成**，代理应用程序将在: [http://localhost:3000](http://localhost:3000/).

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

11. 在聊天中发送以下提示，并在左侧面板查看回复:

+++Customer 251, what's my billing summary?+++

12. 编排器就像管理器或路由器。它读取用户查询并决定由哪个专业代理处理。它会根据上下文和关键词（如“计费”、“促销”、“登录”）来做出这个决定。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.png)

13. 编排器将任务分配给域代理。编排器将查询发送给这些内部代理之一:

    - crm_billing – 账单、发票、付款

    - product_promotions – 产品、折扣、优惠

    - security_authentication – 安全、登录、账户锁定

14. 针对你的查询（“计费摘要”），编排器会将其路由到**crm_billing**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

- 域代理使用连接工具。每个代理都可以通过MCP服务器访问特定的工具（API）。

- 例如：crm_billing可以呼叫get_customer_detail、get_billing_summary、get_invoice_payment等。

- 代理调用合适的工具，获取结构化数据，并生成事实回应。

15. 完成测试后，返回 VS Code
    并终止所有正在运行的终端会话。这确保了即将到来的多智能体工作流程无干扰地运行。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

### 任务3.2：切换模式多智能体系统

在这项任务中，你将探索一种基于切换的多代理系统，该系统根据用户意图，对话在专业代理（如计费、促销或安全）之间无缝切换，确保跨域的交互流畅且具上下文感知。

- **工作原理**

  - 用户直接与域代理交互——例如CRM和计费代理。

  - 意图分类器检查用户的新消息是否属于其他域名（如促销或安全）。

  - 如果是这样，系统会自动将对话（“切换”）给相应的专业代理。

  - 每个代理都有与其领域相关的筛选工具（计费、促销或安全）。

  - 切换过程很顺畅，有上下文转移，这样新客服就能理解对话历史。

1.  展开 **agents (1) \> agent_framework (2) \> multi_agent (3) \>
    handoff_multi_domain_agent (4)**  并查看代码**（5）**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image32.png)

2.  导航到.env文件**（1）**，注释“**Handoff Pattern Multi-Agent
    System** ”变量**（2）**，并输入以下命令添加切换模式多智能体系统变量**（3）**。

+++AGENT_MODULE=agents.agent_framework.multi_agent.handoff_multi_domain_agent+++

- 输入以下命令，控制切换过程中传递多少过去对话上下文。-1表示转移所有之前对话回合（**4）。**

> +++HANDOFF_CONTEXT_TRANSFER_TURNS=-1+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

3.  选择 **File (1)** ，然后选择 **Save(2)**。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

4.  现在，按照步骤启动完整的代理应用，启动其三个核心组件:

5.  在Visual Studio代码窗口中，点击**省略号（...）（1），**然后是
    **Terminal (2)**，然后是 **New Terminal (3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

6.  **启动MCP服务器（Terminal 1）**: (MCP 目录位于项目根级)

    - 执行以下命令启动**MCP服务器**，该服务器会暴露代理可以调用的API，作为工具调用。
      (服务器运行于http://localhost:8000)

> cd mcp
>
> uv run python mcp_service.py

7.  让命令运行，打开一个新的终端。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

8.  **启动后端（Terminal 2）**:

    - 执行以下命令启动托管代理工作流、会话管理和API端点的后端服务器。

> cd agentic_ai/applications
>
> uv run python backend.py

- 这是前端通信的核心应用逻辑。确保**连接是开启**的。

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image23.png)

9.  让命令运行，打开一个 new terminal。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

10. **启动React前端（Terminal 3）**:

    - 输入下面给出的命令，导航到 react-frontend 目录。

> +++cd agentic_ai/applications/react-frontend+++

- 输入以下命令即可启动代理界面的**React前端**。提供用户界面，可与代理互动并实时查看其响应。

> +++npm start+++

- 一旦**webpack成功编译完成**，代理应用程序将在: [http://localhost:3000](http://localhost:3000/)。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

11. 在聊天中发送以下提示，并在左侧面板查看回复:

+++Customer 251, what's my billing summary?+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image34.png)

- 这里，意图分类器会路由到crm_billing域

- get_billing_summary工具为客户251呼叫

12. 你可以就账单问题继续提出以下问题:

+++Yes, I would like to view the invoice details+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image35.png)

**注意**：如果你收到回复，因为我无法获取发票详情，因为引用的数字是发票ID，不是订阅ID。请提供订阅ID，或者如果你需要特定发票的详细信息，请告诉我，以便我能正确协助你。请提供以下提示。

+++Yes, I would like to view the invoice details for customer 251+++

13. 现在让我们尝试一个与另一个域名相关的查询，以测试切换的运作方式。

14. 输入以下与产品与促销相关的查询并查看回复。

+++Are there any promotions available for my subscription plan+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image36.png)

- 由于之前的对话由CRM和计费专家处理，系统检测到域名变更。它决定将对话交给产品与促销专员。

- 系统根据HANDOFF_CONTEXT_TRANSFER_TURNS设置，可以选择性地将之前的对话上下文（比如我们讨论的是哪位客户）转移到新的客服上。

- 产品与促销专员只能使用与促销、计划和产品信息相关的工具（例如get_promotions、get_eligible_promotions）。

15. 完成测试后，返回 VS Code
    并终止所有正在运行的终端会话。这确保了即将到来的多智能体工作流程无干扰地运行。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

**摘要**

在这个实验室中，你创建了一个单代理工作流程，通过MCP与外部工具集成，并探索了多代理设计，即多个专业代理根据用户意图协作或交接对话。你配置了环境变量，启动了完整的代理环境，并测试代理如何智能响应特定领域的查询。
