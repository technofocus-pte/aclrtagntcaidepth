# 实验6：开发具备代理间（A2A）通信的多智能体系统

**预计时长**：30分钟

**概述**

在这个实验室中，你将利用Microsoft代理框架构建一个多代理系统。你将定义不同的座席角色（规划师、人力资源、合规），部署它们，并配置A2A（代理间）通信，允许一个代理拨打其他代理电话。你将测试一个场景，即用户查询通过代理网络委派，然后检查跟踪和日志以确认路由是否正确。

Microsoft Agent Framework SDK
是构建智能、模块化代理的新官方开发套件，能够推理、执行作并与其他代理协作。它提供:

- 统一代理架构——取代AutoGen、语义内核和分片编排器

- Microsoft Foundry 内置支持 — 直接将代理部署到 Foundry 的代理服务中

- 通过MCP（模型上下文协议）工具——与数据、API、系统实现标准化集成

- 原生A2A通信——代理可以呼叫其他代理作为自主协作者

该SDK旨在支持企业级、生产准备的代理系统，从一开始就内置了可靠性、可观察性和治理功能。

实验室目标

你将在实验室执行以下任务。

- 任务1：打开预配置的VS Code项目

- 任务2：创建规划代理

- 任务3：创建人力资源与合规工作人员

- 任务4：定义A2A路由逻辑（代理图/工作流程）

- 任务5：测试多智能体对话并检查日志

## 任务1：打开预配置的VS Code项目

在这项任务中，你将复习预配置的文件夹结构，以了解代理定义、工作流程和工具的组织位置。这为你使用
Microsoft Agent Framework SDK 扩展系统做好准备。

1.  在LabVM桌面中，打开**Visual Studio Code**。

2.  打开 Visual Studio Code 后，点击 **File** **(1)** ，选择 **Open
    Folder** **(2)** 选项以打开代码文件文件夹。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

3.  进入打开文件夹后，进入 C：\Labfiles\Day
    2\Enterprise-Agent-Code-Files，点击 **select folder**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

4.  打开后会弹出一个窗口，点击“**Yes, I trust authors** **”**选项。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

5.  请查看企业代理的文件夹结构。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

6.  右键点击 **.env.example (1)** 文件，选择 **Rename
    (2)** 以重命名文件。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image5.png)

7.  完成后，将文件重命名为 **.env.example** --\>
    **.env**，使该环境文件为该代理激活。

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image6.png)

8.  将 .env 文件的内容替换为以下内容。

> AZURE_OPENAI_ENDPOINT=https://agentic-
> @lab.LabInstance.Id.cognitiveservices.azure.com/
>
> AZURE_OPENAI_API_KEY=**\<Replace with Azure OpenAI key\>**
>
> AZURE_OPENAI_RESPONSES_DEPLOYMENT_NAME=gpt-4o-mini
>
> AZURE_OPENAI_API_VERSION=2025-03-01-preview

在 Microsoft Foundry 概览页面，复制 API 密钥并替换

占位符 **\<Replace with Azure OpenAI key\>** 在env文件中。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

9.  完成后，选择 **File** **(1)** ，然后点击 **Save** **(2)** 保存文件。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image9.png)

## 任务2：创建规划代理

在此任务中，您将定义Planner代理，该代理能够解读用户查询并决定将任务委派给哪位专业代理。你将使用
Agent Framework SDK 配置代理，并根据特定角色的指令进行配置。

1.  在列表中，选择代理文件夹下的 **planner_agent.py。**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.png)

2.  添加以下 Python 代码来配置规划代理。

> import os
>
> import asyncio
>
> from agent_framework.azure import AzureOpenAIResponsesClient \# type:
> ignore
>
> async def build_planner_agent():
>
> client = AzureOpenAIResponsesClient(
>
> api_key=os.getenv("AZURE_OPENAI_API_KEY"),
>
> endpoint=os.getenv("AZURE_OPENAI_ENDPOINT"),
>
> deployment_name=os.getenv("AZURE_OPENAI_RESPONSES_DEPLOYMENT_NAME"),
>
> api_version=os.getenv("AZURE_OPENAI_API_VERSION"),
>
> )
>
> return client.create_agent(
>
> name="PlannerAgent",
>
> instructions=(
>
> "You are an intelligent routing agent. Analyze user queries and route
> them to the appropriate specialist. "
>
> "Available specialists:\n"
>
> "- HR: Employee policies, leave, benefits, working hours, performance,
> hiring\n"
>
> "- FINANCE: Reimbursements, expenses, budgets, travel costs, meal
> allowances, equipment purchases\n"
>
> "- COMPLIANCE: GDPR, data privacy, regulatory requirements, legal
> compliance, audits\n\n"
>
> "Return exactly one word: HR, FINANCE, or COMPLIANCE. "
>
> "Consider keywords like: money, cost, budget, reimburse, expense,
> payment, allowance → FINANCE\n"
>
> "Keywords like: leave, sick, vacation, policy, employee, benefits →
> HR\n"
>
> "Keywords like: GDPR, privacy, compliance, legal, audit, regulation →
> COMPLIANCE"
>
> ),
>
> )
>
> async def classify_target(planner_agent, user_query: str) -\> str:
>
> result = await planner_agent.run(
>
> "Analyze and route this query:\n\n"
>
> f"User query: {user_query}\n\n"
>
> "Return exactly one word: HR, FINANCE, or COMPLIANCE."
>
> )
>
> \# Extract the text content from the AgentRunResponse object
>
> text = str(result).strip().lower()
>
> \# Advanced classification with fallback logic
>
> if "finance" in text or "financial" in text:
>
> return "FINANCE"
>
> elif "hr" in text or "human" in text:
>
> return "HR"
>
> elif "compliance" in text or "legal" in text:
>
> return "COMPLIANCE"
>
> else:
>
> \# Fallback keyword analysis if agent response is unclear
>
> query_lower = user_query.lower()
>
> finance_keywords = \["reimburs", "expense", "cost", "budget", "money",
> "payment", "allowance", "travel", "meal", "flight", "hotel"\]
>
> hr_keywords = \["leave", "sick", "vacation", "employee", "benefit",
> "policy", "hire", "performance", "work"\]
>
> compliance_keywords = \["gdpr", "privacy", "compliance", "legal",
> "audit", "regulation", "data protection"\]
>
> finance_score = sum(1 for keyword in finance_keywords if keyword in
> query_lower)
>
> hr_score = sum(1 for keyword in hr_keywords if keyword in query_lower)
>
> compliance_score = sum(1 for keyword in compliance_keywords if keyword
> in query_lower)
>
> if finance_score \> hr_score and finance_score \> compliance_score:
>
> return "FINANCE"
>
> elif hr_score \> compliance_score:
>
> return "HR"
>
> else:
>
> return "COMPLIANCE"

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image11.png)

> **规划代理人的目的:**

- 该代理旨在分析用户问题，并决定由哪位专业代理（人力资源、财务或合规）负责响应。

> **使用 AzureOpenAIResponseClient 创建代理:**

- build_planner_agent（） 函数使用 Agent Framework SDK
  初始化规划器，基于基于 API 的凭证从环境变量加载。

> **LLM引导路由（主逻辑）:**

- 规划代理被指示根据查询中的关键字和上下文返回一个单词——HR、FINANCE 或
  COMPLIANCE。

> **classify_target（） 用于决策:**

- 该函数首先使用 await agent.run（）
  调用来询问规划器选择哪个专家。如果回答不清楚，则采用基于关键词的备选分析。

> **混合 AI+启发式策略:**

- 设计确保路由的可靠性，结合模型推理与手动关键词评分，使得即使AI输出模糊，规划器依然稳健。

3.  完成后，选择 **File** **(1)** ，然后点击 **Save** **(2)** 保存文件。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

## 任务3：创建工作代理

在此任务中，您将培养负责人力资源、财务和合规知识的领域专属代理。每位代理都将注册在代理登记处，以便通过A2A通信实现发现和委派。

1.  从列表中选择代理文件夹下的**hr_agent.py**，添加以下Python代码来配置人力资源代理。添加以下
    Python 代码来配置人力资源代理。

> import os
>
> import asyncio
>
> from agent_framework.azure import AzureOpenAIResponsesClient \# type:
> ignore
>
> async def build_hr_agent():
>
> client = AzureOpenAIResponsesClient(
>
> api_key=os.getenv("AZURE_OPENAI_API_KEY"),
>
> endpoint=os.getenv("AZURE_OPENAI_ENDPOINT"),
>
> deployment_name=os.getenv("AZURE_OPENAI_RESPONSES_DEPLOYMENT_NAME"),
>
> api_version=os.getenv("AZURE_OPENAI_API_VERSION"),
>
> )
>
> return client.create_agent(
>
> name="HRAgent",
>
> instructions=(
>
> "You are an expert HR policy specialist with deep knowledge of
> employment law and best practices. "
>
> "Answer questions about:\n"
>
> "- Leave policies (sick, vacation, parental, bereavement)\n"
>
> "- Employee benefits (health insurance, retirement, wellness
> programs)\n"
>
> "- Performance management and reviews\n"
>
> "- Hiring, onboarding, and termination procedures\n"
>
> "- Working hours, overtime, and flexible work arrangements\n"
>
> "- Employee relations and conflict resolution\n"
>
> "- Training and development programs\n\n"
>
> "Provide specific, actionable guidance with policy references where
> applicable. "
>
> "Be empathetic and professional in your responses."
>
> ),
>
> )

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image12.png)

> **人力资源代理的目的:**

- 该代理人作为专门的人力资源政策专家，接受过培训，能够回答与员工福利、休假结构、福利及工作场所程序相关的问题。

> **使用 Azure 响应客户端初始化代理:**

- build_hr_agent（） 函数通过 AzureOpenAIResponsesClient
  初始化代理，该客户端通过 API 密钥和存储在环境变量中的端点值进行认证。

> **领域专精:**

- 说明部分明确定义了人力资源代理的职责范围——包括休假类型、福利、入职、员工关系和绩效管理——确保只回复与人力资源相关的咨询。

> **专业与同理心的语气:**

- 该代理旨在模拟真实的人力资源沟通标准，提供准确、专业且富有同理心的指导，非常适合内部组织助理。

> **多智能体协作基金会:**

- 构建完成后，该人力资源代理将由规划代理调用，允许在检测到人力资源相关查询时实现多代理工作流程中的自动委派。

2.  完成后，选择 **File** **(1)** ，然后点击 **Save** **(2)** 保存文件。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

3.  从列表中选择代理文件夹下的**finance_agent.py**，添加以下 Python
    代码以配置合规代理。添加以下 Python 代码来配置财务代理。

> import os
>
> import asyncio
>
> from agent_framework.azure import AzureOpenAIResponsesClient \# type:
> ignore
>
> async def build_finance_agent():
>
> client = AzureOpenAIResponsesClient(
>
> api_key=os.getenv("AZURE_OPENAI_API_KEY"),
>
> endpoint=os.getenv("AZURE_OPENAI_ENDPOINT"),
>
> deployment_name=os.getenv("AZURE_OPENAI_RESPONSES_DEPLOYMENT_NAME"),
>
> api_version=os.getenv("AZURE_OPENAI_API_VERSION"),
>
> )
>
> return client.create_agent(
>
> name="FinanceAgent",
>
> instructions=(
>
> "You are a finance and reimbursement specialist. Answer questions
> about "
>
> "expense policies, reimbursement limits, budget approvals, travel
> expenses, "
>
> "meal allowances, equipment purchases, and financial procedures.
> Provide "
>
> "specific amounts, policies, and actionable guidance."
>
> ),
>
> )

![A computer screen shot of a program AI-generated content may be
incorrect.](./media/image13.png)

> 专业金融岗位:

- 该代理旨在处理所有与财务相关的事务，包括报销政策、差旅预算、津贴和购房审批。

> **通过 Agent Framework SDK 进行初始化:**

- build_finance_agent（） 函数使用 AzureOpenAIResponsesClient
  创建代理，利用安全环境变量的 API 密钥认证。

> **政策导向指令:**

- 代理人的指示明确将责任限制在财务程序上，确保关于成本、付款、预算和公司支出规则的准确回答。

> **精准与可作输出:**

- 与通用代理不同，该财务助理被要求提供具体的保单价值，如限额、资格或审批流程，使员工更实用。

> **支持规划师代表团（A2A）:**

- 当规划工具检测到与财务相关的关键词或查询时，该代理将自动被调用，实现系统内无缝的多代理协作。

4.  完成后，选择 **File** **(1)** ，然后点击 **Save** **(2)** 保存文件。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

5.  在列表中，选择代理文件夹下的**compliance_agent.py**，添加以下 Python
    代码来配置合规代理。添加以下 Python 代码来配置合规代理。

> import os
>
> import asyncio
>
> from agent_framework.azure import AzureOpenAIResponsesClient \# type:
> ignore
>
> async def build_compliance_agent():
>
> client = AzureOpenAIResponsesClient(
>
> api_key=os.getenv("AZURE_OPENAI_API_KEY"),
>
> endpoint=os.getenv("AZURE_OPENAI_ENDPOINT"),
>
> deployment_name=os.getenv("AZURE_OPENAI_RESPONSES_DEPLOYMENT_NAME"),
>
> api_version=os.getenv("AZURE_OPENAI_API_VERSION"),
>
> )
>
> return client.create_agent(
>
> name="ComplianceAgent",
>
> instructions=(
>
> "You are a senior compliance and legal specialist with expertise in
> multiple jurisdictions. "
>
> "Provide authoritative guidance on:\n"
>
> "- GDPR and data protection regulations (EU, UK, US state laws)\n"
>
> "- Privacy policies and data processing agreements\n"
>
> "- Regulatory compliance (SOX, HIPAA, PCI-DSS, ISO standards)\n"
>
> "- Risk assessment and audit requirements\n"
>
> "- Contract law and vendor agreements\n"
>
> "- Information security policies\n"
>
> "- Cross-border data transfers and adequacy decisions\n"
>
> "- Breach notification requirements\n\n"
>
> "Always provide factual, well-researched answers with relevant legal
> citations. "
>
> "Include practical implementation steps and potential risks. Use
> formal, professional tone."
>
> ),
>
> )

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image14.png)

> **代理的目的:**

- 该代理作为专门的法律和合规权威，负责处理与GDPR、监管框架、合同法、风险评估和安全标准相关的咨询。

> **代理初始化:**

- build_compliance_agent（）函数使用AzureOpenAIResponsesClient配合API密钥认证，通过Microsoft代理框架SDK注册合规代理。

> **说明中定义的监管专业知识:**

- 这些说明明确了合规范围——包括全球隐私法规（GDPR、HIPAA、SOX）、审计准备、法律协议和泄露协议——确保高信任度的响应。

> **音色与输出期望:**

- 该代理配置为以正式、权威的语气提供答案，包括法律引用或适用的实施建议。

> **在多智能体系统中的作用:**

- 在A2A委派过程中，规划代理人将法律或合规相关问题转发给该专家，确保企业决策工作流程的准确性和治理。

6.  完成后，选择 **File** **(1)** ，然后点击 **Save** **(2)** 保存文件。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

## 任务4：定义A2A路由逻辑（代理图/工作流程）

代理到代理（A2A）是Microsoft代理框架的核心功能，允许一个代理自主地将任务委派给另一个代理。

在此任务中，您将通过代理工作流程实现路由逻辑，使规划师能够根据查询意图自主呼叫人力资源或合规代理。这确立了真正的多代理协作。

1.  在列表中，选择代理文件夹下的 **main.py**，添加以下 Python 代码以配置
    A2A 通信流代理。添加以下 Python 代码来配置代理路由逻辑。

> import asyncio
>
> import time
>
> import logging
>
> from typing import Dict, Any
>
> from utils.env import load_env
>
> from agents.planner_agent import build_planner_agent, classify_target
>
> from agents.hr_agent import build_hr_agent
>
> from agents.compliance_agent import build_compliance_agent
>
> from agents.finance_agent import build_finance_agent
>
> \# Configure logging
>
> logging.basicConfig(level=logging.INFO, format='%(asctime)s -
> %(levelname)s - %(message)s')
>
> async def run_multi_agent(query: str, agents: Dict\[str, Any\]) -\>
> Dict\[str, Any\]:
>
> """
>
> Advanced multi-agent system with routing, timing, and comprehensive
> response handling.
>
> """
>
> start_time = time.time()
>
> try:
>
> \# Step 1: Route the query
>
> logging.info(f"Routing query: {query\[:50\]}...")
>
> target = await classify_target(agents\["planner"\], query)
>
> logging.info(f"Query routed to: {target}")
>
> \# Step 2: Get response from appropriate agent
>
> agent_mapping = {
>
> "HR": ("hr", "HRAgent"),
>
> "FINANCE": ("finance", "FinanceAgent"),
>
> "COMPLIANCE": ("compliance", "ComplianceAgent")
>
> }
>
> if target in agent_mapping:
>
> agent_key, agent_name = agent_mapping\[target\]
>
> answer = await agents\[agent_key\].run(query)
>
> else:
>
> \# Fallback to HR if routing unclear
>
> logging.warning(f"Unknown target '{target}', falling back to HR")
>
> answer = await agents\["hr"\].run(query)
>
> target = "HR"
>
> agent_name = "HRAgent"
>
> \# Step 3: Process response
>
> response_time = time.time() - start_time
>
> return {
>
> "query": query,
>
> "routed_to": target,
>
> "agent_name": agent_name,
>
> "answer": str(answer),
>
> "response_time": round(response_time, 2),
>
> "timestamp": time.strftime("%Y-%m-%d %H:%M:%S"),
>
> "success": True
>
> }
>
> except Exception as e:
>
> logging.error(f"Error processing query: {e}")
>
> return {
>
> "query": query,
>
> "routed_to": "ERROR",
>
> "agent_name": "ErrorHandler",
>
> "answer": f"I apologize, but I encountered an error processing your
> request: {str(e)}",
>
> "response_time": round(time.time() - start_time, 2),
>
> "timestamp": time.strftime("%Y-%m-%d %H:%M:%S"),
>
> "success": False
>
> }
>
> def format_response(result: Dict\[str, Any\]) -\> str:
>
> """Format the agent response for display."""
>
> status_icon = "✅" if result\["success"\] else "❌"
>
> formatted = f"""
>
> {status_icon} Agent Response Summary:
>
> ┌─ Routed to: {result\['routed_to'\]} ({result\['agent_name'\]})
>
> ├─ Response time: {result\['response_time'\]}s
>
> ├─ Timestamp: {result\['timestamp'\]}
>
> └─ Status: {'Success' if result\['success'\] else 'Error'}
>
> 💬 Answer:
>
> {result\['answer'\]}
>
> """
>
> return formatted
>
> async def run_interactive_mode(agents: Dict\[str, Any\]):
>
> """Interactive mode for real-time queries."""
>
> print("\n🤖 Enterprise Agent System - Interactive Mode")
>
> print("Available agents: HR, Finance, Compliance")
>
> print("Type 'quit' to exit, 'help' for commands\n")
>
> while True:
>
> try:
>
> query = input("Enter your question: ").strip()
>
> if query.lower() in \['quit', 'exit', 'q'\]:
>
> print("👋 Goodbye!")
>
> break
>
> elif query.lower() == 'help':
>
> print("""
>
> 📋 Available Commands:
>
> \- Ask any question about HR, Finance, or Compliance
>
> \- 'quit' or 'exit' - Exit the system
>
> \- 'help' - Show this help message
>
> 🎯 Example questions:
>
> \- "What's the travel reimbursement limit for meals?"
>
> \- "How many vacation days do employees get?"
>
> \- "Do we need GDPR compliance for EU customers?"
>
> """)
>
> continue
>
> elif not query:
>
> continue
>
> result = await run_multi_agent(query, agents)
>
> print(format_response(result))
>
> except KeyboardInterrupt:
>
> print("\n👋 Goodbye!")
>
> break
>
> except Exception as e:
>
> logging.error(f"Interactive mode error: {e}")
>
> print(f"❌ Error: {e}")
>
> async def run_batch_tests(agents: Dict\[str, Any\]):
>
> """Run predefined test queries."""
>
> test_queries = \[
>
> "How much reimbursement is allowed for international flights?",
>
> "Is employee data protected under GDPR?",
>
> "How many sick leave days do employees get?"
>
> \]
>
> print("🧪 Running batch tests...\n")
>
> for i, query in enumerate(test_queries, 1):
>
> print(f"{'='\*80}")
>
> print(f"TEST {i}/{len(test_queries)}: {query}")
>
> print(f"{'='\*80}")
>
> result = await run_multi_agent(query, agents)
>
> print(format_response(result))
>
> \# Small delay between queries for better readability
>
> if i \< len(test_queries):
>
> await asyncio.sleep(0.5)
>
> async def main():
>
> """Main application entry point with enhanced features."""
>
> print("🚀 Initializing Enterprise Agent System...")
>
> try:
>
> \# Load environment and build agents
>
> load_env()
>
> logging.info("Building agent network...")
>
> agents = {
>
> "planner": await build_planner_agent(),
>
> "hr": await build_hr_agent(),
>
> "compliance": await build_compliance_agent(),
>
> "finance": await build_finance_agent()
>
> }
>
> logging.info("✅ All agents initialized successfully")
>
> \# Check if running interactively or in batch mode
>
> import sys
>
> if len(sys.argv) \> 1 and sys.argv\[1\] == "--interactive":
>
> await run_interactive_mode(agents)
>
> else:
>
> await run_batch_tests(agents)
>
> except Exception as e:
>
> logging.error(f"System initialization failed: {e}")
>
> print(f"❌ Failed to start system: {e}")
>
> if \_\_name\_\_ == "\_\_main\_\_":
>
> asyncio.run(main())

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image15.png)

> **中央执行引擎:**

- 该脚本作为核心编排器，协调所有代理（规划师、人力资源、财务、合规），并利用
  Microsoft 代理框架管理多代理路由。

> **代理网络初始化:**

- 它加载环境设置，用等待 build\_\*\_agent（）
  构建每个代理，并将其注册到共享字典中以便委派。

> **高级A2A路由:**

- run_multi_agent（）功能通过规划器将用户查询转发给正确的专家，然后等待专业代理的回复。它记录了路由、时机、成功状态和最终答案。

> **多重执行模式:**

- 批处理模式：运行预定义的测试查询。

- 互动模式（--interactive）：支持实时聊天，支持实时测试和探索。

> **生产准备韧性:**

- 包括响应格式、时间戳、错误回退机制和日志记录——为后续的可观测性、遥测和代理运营奠定了坚实基础。

2.  完成后，选择 **File** **(1)** ，然后点击 **Save** **(2)** 保存文件。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

## 任务5：测试多智能体对话并检查日志

在此任务中，您将通过多代理系统运行端到端测试查询，并通过Microsoft
Foundry中的日志和遥测观察代理协作。

1.  你已经成功配置了多代理系统，包括规划代理和工作代理。现在，你要测试这个多智能体系统的运作。

> **注：**尽管多智能体系统现已配置LLM功能，但尚未集成MCP或访问外部知识源（如数据集或Azure
> AI搜索索引）。此时，代理将完全依赖其一般模型智能来回答问题。

2.  选择......**（1）**顶部菜单中的扩展菜单选项。选择 **Terminal
    (2)** ，然后点击 **New Terminal (3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image16.png)

3.  终端打开后，执行命令，

+++pip install -r requirements.txt+++ 以安装所有必需的包。

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image17.png)

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image18.png)

4.  安装成功完成后，执行以下命令运行代理并查看代码文件中测试提示的响应。

+++python main.py+++

> ![A computer screen shot of a program AI-generated content may be
> incorrect.](./media/image19.png)

检查**“Routed to**”参数，并查看代理如何确定并路由请求到相应的工作代 理。

5.  现在，通过添加 --interactive flag
    再次运行交互模式。这样你就可以输入问题，得到回复。当提示提出时，请提供以下提示作为问题。

    - 命令:

> +++python main.py –interactive+++

- 提示:

> +++How much reimbursement is allowed for international flights?+++

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image20.png)

6.  收到回复后，在下一个提示中添加q以退出或停止代理。

![A black screen with white text AI-generated content may be
incorrect.](./media/image21.png)

**摘要**

在这个实验室里，你用 Microsoft Agent Framework SDK
定义了三个代理（规划者、人力资源和合规），并注册了它们。你构建了一个路由流程，通过代理间调用委托用户查询。你测试了一个多代理场景，并检查日志以确认消息路由和执行流程是否正确。
