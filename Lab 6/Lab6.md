# 實驗6：開發具備代理間（A2A）通信的多智能體系統

**預計時長**：30分鐘

**概述**

在這個實驗室中，你將利用Microsoft代理框架構建一個多代理系統。你將定義不同的座席角色（規劃師、人力資源、合規），部署它們，並配置A2A（代理間）通信，允許一個代理撥打其他代理電話。你將測試一個場景，即用戶查詢通過代理網絡委派，然後檢查跟蹤和日誌以確認路由是否正確。

Microsoft Agent Framework SDK
是構建智能、模塊化代理的新官方開發套件，能夠推理、執行作並與其他代理協作。它提供:

- 統一代理架構——取代AutoGen、語義內核和分片編排器

- Microsoft Foundry 內置支持 — 直接將代理部署到 Foundry 的代理服務中

- 通過MCP（模型上下文協議）工具——與數據、API、系統實現標準化集成

- 原生A2A通信——代理可以呼叫其他代理作為自主協作者

該SDK旨在支持企業級、生產準備的代理系統，從一開始就內置了可靠性、可觀察性和治理功能。

實驗室目標

你將在實驗室執行以下任務。

- 任務1：打開預配置的VS Code項目

- 任務2：創建規劃代理

- 任務3：創建人力資源與合規工作人員

- 任務4：定義A2A路由邏輯（代理圖/工作流程）

- 任務5：測試多智能體對話並檢查日誌

## 任務1：打開預配置的VS Code項目

在這項任務中，你將複習預配置的文件夾結構，以瞭解代理定義、工作流程和工具的組織位置。這為你使用
Microsoft Agent Framework SDK 擴展系統做好準備。

1.  在LabVM桌面中，打開**Visual Studio Code**。

2.  打開 Visual Studio Code 後，點擊 **File** **(1)** ，選擇 **Open
    Folder** **(2)** 選項以打開代碼文件文件夾。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

3.  進入打開文件夾後，進入 C：\Labfiles\Day
    2\Enterprise-Agent-Code-Files，點擊 **select folder**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

4.  打開後會彈出一個窗口，點擊“**Yes, I trust authors** **”**選項。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

5.  請查看企業代理的文件夾結構。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

6.  右鍵點擊 **.env.example (1)** 文件，選擇 **Rename
    (2)** 以重命名文件。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image5.png)

7.  完成後，將文件重命名為 **.env.example** --\>
    **.env**，使該環境文件為該代理激活。

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image6.png)

8.  將 .env 文件的內容替換為以下內容。

> AZURE_OPENAI_ENDPOINT=https://agentic-
> @lab.LabInstance.Id.cognitiveservices.azure.com/
>
> AZURE_OPENAI_API_KEY=**\<Replace with Azure OpenAI key\>**
>
> AZURE_OPENAI_RESPONSES_DEPLOYMENT_NAME=gpt-4o-mini
>
> AZURE_OPENAI_API_VERSION=2025-03-01-preview

在 Microsoft Foundry 概覽頁面，複製 API 密鑰並替換

占位符 **\<Replace with Azure OpenAI key\>** 在env文件中。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

9.  完成後，選擇 **File** **(1)** ，然後點擊 **Save** **(2)** 保存文件。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image9.png)

## 任務2：創建規劃代理

在此任務中，您將定義Planner代理，該代理能夠解讀用戶查詢並決定將任務委派給哪位專業代理。你將使用
Agent Framework SDK 配置代理，並根據特定角色的指令進行配置。

1.  在列表中，選擇代理文件夾下的 **planner_agent.py。**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.png)

2.  添加以下 Python 代碼來配置規劃代理。

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

> **規劃代理人的目的:**

- 該代理旨在分析用戶問題，並決定由哪位專業代理（人力資源、財務或合規）負責響應。

> **使用 AzureOpenAIResponseClient 創建代理:**

- build_planner_agent（） 函數使用 Agent Framework SDK
  初始化規劃器，基於基於 API 的憑證從環境變量加載。

> **LLM引導路由（主邏輯）:**

- 規劃代理被指示根據查詢中的關鍵字和上下文返回一個單詞——HR、FINANCE 或
  COMPLIANCE。

> **classify_target（） 用於決策:**

- 該函數首先使用 await agent.run（）
  調用來詢問規劃器選擇哪個專家。如果回答不清楚，則採用基於關鍵詞的備選分析。

> **混合 AI+啟發式策略:**

- 設計確保路由的可靠性，結合模型推理與手動關鍵詞評分，使得即使AI輸出模糊，規劃器依然穩健。

3.  完成後，選擇 **File** **(1)** ，然後點擊 **Save** **(2)** 保存文件。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

## 任務3：創建工作代理

在此任務中，您將培養負責人力資源、財務和合規知識的領域專屬代理。每位代理都將註冊在代理登記處，以便通過A2A通信實現發現和委派。

1.  從列表中選擇代理文件夾下的**hr_agent.py**，添加以下Python代碼來配置人力資源代理。添加以下
    Python 代碼來配置人力資源代理。

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

> **人力資源代理的目的:**

- 該代理人作為專門的人力資源政策專家，接受過培訓，能夠回答與員工福利、休假結構、福利及工作場所程序相關的問題。

> **使用 Azure 響應客戶端初始化代理:**

- build_hr_agent（） 函數通過 AzureOpenAIResponsesClient
  初始化代理，該客戶端通過 API 密鑰和存儲在環境變量中的端點值進行認證。

> **領域專精:**

- 說明部分明確定義了人力資源代理的職責範圍——包括休假類型、福利、入職、員工關係和績效管理——確保只回復與人力資源相關的諮詢。

> **專業與同理心的語氣:**

- 該代理旨在模擬真實的人力資源溝通標準，提供準確、專業且富有同理心的指導，非常適合內部組織助理。

> **多智能體協作基金會:**

- 構建完成後，該人力資源代理將由規劃代理調用，允許在檢測到人力資源相關查詢時實現多代理工作流程中的自動委派。

2.  完成後，選擇 **File** **(1)** ，然後點擊 **Save** **(2)** 保存文件。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

3.  從列表中選擇代理文件夾下的**finance_agent.py**，添加以下 Python
    代碼以配置合規代理。添加以下 Python 代碼來配置財務代理。

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

> 專業金融崗位:

- 該代理旨在處理所有與財務相關的事務，包括報銷政策、差旅預算、津貼和購房審批。

> **通過 Agent Framework SDK 進行初始化:**

- build_finance_agent（） 函數使用 AzureOpenAIResponsesClient
  創建代理，利用安全環境變量的 API 密鑰認證。

> **政策導向指令:**

- 代理人的指示明確將責任限制在財務程序上，確保關於成本、付款、預算和公司支出規則的準確回答。

> **精准與可作輸出:**

- 與通用代理不同，該財務助理被要求提供具體的保單價值，如限額、資格或審批流程，使員工更實用。

> **支持規劃師代表團（A2A）:**

- 當規劃工具檢測到與財務相關的關鍵詞或查詢時，該代理將自動被調用，實現系統內無縫的多代理協作。

4.  完成後，選擇 **File** **(1)** ，然後點擊 **Save** **(2)** 保存文件。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

5.  在列表中，選擇代理文件夾下的**compliance_agent.py**，添加以下 Python
    代碼來配置合規代理。添加以下 Python 代碼來配置合規代理。

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

- 該代理作為專門的法律和合規權威，負責處理與GDPR、監管框架、合同法、風險評估和安全標準相關的諮詢。

> **代理初始化:**

- build_compliance_agent（）函數使用AzureOpenAIResponsesClient配合API密鑰認證，通過Microsoft代理框架SDK註冊合規代理。

> **說明中定義的監管專業知識:**

- 這些說明明確了合規範圍——包括全球隱私法規（GDPR、HIPAA、SOX）、審計準備、法律協議和洩露協議——確保高信任度的響應。

> **音色與輸出期望:**

- 該代理配置為以正式、權威的語氣提供答案，包括法律引用或適用的實施建議。

> **在多智能體系統中的作用:**

- 在A2A委派過程中，規劃代理人將法律或合規相關問題轉發給該專家，確保企業決策工作流程的準確性和治理。

6.  完成後，選擇 **File** **(1)** ，然後點擊 **Save** **(2)** 保存文件。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

## 任務4：定義A2A路由邏輯（代理圖/工作流程）

代理到代理（A2A）是Microsoft代理框架的核心功能，允許一個代理自主地將任務委派給另一個代理。

在此任務中，您將通過代理工作流程實現路由邏輯，使規劃師能夠根據查詢意圖自主呼叫人力資源或合規代理。這確立了真正的多代理協作。

1.  在列表中，選擇代理文件夾下的 **main.py**，添加以下 Python 代碼以配置
    A2A 通信流代理。添加以下 Python 代碼來配置代理路由邏輯。

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

> **中央執行引擎:**

- 該腳本作為核心編排器，協調所有代理（規劃師、人力資源、財務、合規），並利用
  Microsoft 代理框架管理多代理路由。

> **代理網絡初始化:**

- 它加載環境設置，用等待 build\_\*\_agent（）
  構建每個代理，並將其註冊到共享字典中以便委派。

> **高級A2A路由:**

- run_multi_agent（）功能通過規劃器將用戶查詢轉發給正確的專家，然後等待專業代理的回復。它記錄了路由、時機、成功狀態和最終答案。

> **多重執行模式:**

- 批處理模式：運行預定義的測試查詢。

- 互動模式（--interactive）：支持實時聊天，支持實時測試和探索。

> **生產準備韌性:**

- 包括響應格式、時間戳、錯誤回退機制和日誌記錄——為後續的可觀測性、遙測和代理運營奠定了堅實基礎。

2.  完成後，選擇 **File** **(1)** ，然後點擊 **Save** **(2)** 保存文件。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

## 任務5：測試多智能體對話並檢查日誌

在此任務中，您將通過多代理系統運行端到端測試查詢，並通過Microsoft
Foundry中的日誌和遙測觀察代理協作。

1.  你已經成功配置了多代理系統，包括規劃代理和工作代理。現在，你要測試這個多智能體系統的運作。

> **注：**儘管多智能體系統現已配置LLM功能，但尚未集成MCP或訪問外部知識源（如數據集或Azure
> AI搜索索引）。此時，代理將完全依賴其一般模型智能來回答問題。

2.  選擇......**（1）**頂部菜單中的擴展菜單選項。選擇 **Terminal
    (2)** ，然後點擊 **New Terminal (3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image16.png)

3.  終端打開後，執行命令，

+++pip install -r requirements.txt+++ 以安裝所有必需的包。

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image17.png)

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image18.png)

4.  安裝成功完成後，執行以下命令運行代理並查看代碼文件中測試提示的響應。

+++python main.py+++

> ![A computer screen shot of a program AI-generated content may be
> incorrect.](./media/image19.png)

檢查**“Routed to**”參數，並查看代理如何確定並路由請求到相應的工作代 理。

5.  現在，通過添加 --interactive flag
    再次運行交互模式。這樣你就可以輸入問題，得到回復。當提示提出時，請提供以下提示作為問題。

    - 命令:

> +++python main.py –interactive+++

- 提示:

> +++How much reimbursement is allowed for international flights?+++

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image20.png)

6.  收到回復後，在下一個提示中添加q以退出或停止代理。

![A black screen with white text AI-generated content may be
incorrect.](./media/image21.png)

**摘要**

在這個實驗室裡，你用 Microsoft Agent Framework SDK
定義了三個代理（規劃者、人力資源和合規），並註冊了它們。你構建了一個路由流程，通過代理間調用委託用戶查詢。你測試了一個多代理場景，並檢查日誌以確認消息路由和執行流程是否正確。
