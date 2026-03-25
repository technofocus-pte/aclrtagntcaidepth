# 實驗7：利用MCP構建AI驅動的工單管理系統

**預計時長**：60分鐘

**概述**

在本實驗室中，你將通過使用模型上下文協議（MCP）將企業代理系統與真實組織數據和外部服務連接來擴展。你將集成Azure
AI Search，提供基於上下文的知識庫響應，並連接Freshdesk
API，使客服能夠執行實際作，如創建人力資源或財務工單。

完成本實驗室後，您的代理將從靜態對話模型發展為智能、數據感知和行動驅動的助手，能夠安全地與企業系統交互。

實驗室目標

你將在實驗室執行以下任務。

- 任務1：構建Azure Search MCP工具

- 任務2：將工具附加到代理上，豐富提示，並運行測試

- 任務三：設置Freshworks用於工單管理

- 任務4：將代理連接到外部API（Freshdesk MCP集成）

## 任務1：構建Azure Search MCP工具

在這個任務中，你將用AI搜索憑證更新環境變量，創建一個異步工具類，查詢Azure
AI Search並返回頂部N個文檔片段，供代理使用這些內容作為上下文。

MCP是一種標準，使AI代理能夠通過結構化輸入/輸出合同安全地訪問外部知識和工具。它允許代理檢索事實信息、調用API，並以受控和可審計的方式執行作，構成了紮根且可擴展的企業AI系統的骨幹。

1.  在 Azure 門戶中，進入 **agenticai**，從資源列表中選擇
    **ai-knowledge-** Search Service。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

2.  在設置中左側菜單選擇 **Keys (1)** ，然後使用複製選項複製 **Query Key
    (2)** 。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

3.  複製完成後，安全地粘貼到記事本，在搜索管理的左側菜單中選擇 **Indexes
    (1)** ，複製 **Index Name (2)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

4.  你之前創建過多智能體系統，在 Visual Studio Code 面板中選擇 .env
    文件，因為你需要添加 AI 搜索鍵才能連接。

5.  在 .env 文件中，將這部分添加到 AI Foundry 鍵下方。用
    你之前複製的**\[Query_Key\]**和**\[Index_Name\]**值替換。

> AZURE_SEARCH_ENDPOINT=https://ai-knowledge--@lab.LabInstance.Id.search.windows.net/
>
> AZURE_SEARCH_API_KEY=\[Query_Key\]
>
> AZURE_SEARCH_INDEX=\[Index_Name\]

6.  完成後，請保存文件。點擊頂部菜單中的“**file (1)** ”選項，選擇 **save
    (2)** 以保存文件。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image4.png)

7.  完成後，點擊顯示的**“Create
    Folder**”選項，提示時輸入文件夾名稱作為工具。請在根目錄中創建一個新文件夾。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

8.  創建後，文件夾結構會類似這個。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

9.  現在，選擇之前創建的 **tools (1)** 文件夾，點擊顯示的“**Create file
    (2)** ”選項。這會在工具文件夾下創建一個文件。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

10. 請提供文件名稱，如同 azure_search_tool.py。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

11. 創建後，添加以下代碼片段，配置你的代理搜索工具。

> import os
>
> import sys
>
> import aiohttp
>
> import json
>
> import asyncio
>
> from typing import List
>
> from pathlib import Path
>
> \# Add parent directory to path for imports
>
> sys.path.append(str(Path(\_\_file\_\_).parent.parent))
>
> class AzureSearchTool:
>
> """
>
> Async MCP-like tool to query Azure Cognitive Search index.
>
> Reads endpoint/key/index from environment and returns joined content
> snippets.
>
> """
>
> def \_\_init\_\_(self):
>
> self.endpoint = os.getenv("AZURE_SEARCH_ENDPOINT")
>
> self.api_key = os.getenv("AZURE_SEARCH_API_KEY")
>
> self.index_name = os.getenv("AZURE_SEARCH_INDEX")
>
> if not all(\[self.endpoint, self.api_key, self.index_name\]):
>
> raise RuntimeError("Azure Search env vars not set
> (AZURE_SEARCH_ENDPOINT/KEY/INDEX).")
>
> if not self.endpoint.endswith('/'):
>
> self.endpoint += '/'
>
> self.api_version = "2023-11-01"
>
> async def search(self, query: str, top: int = 5) -\> str:
>
> """
>
> Query the Azure Search index and return concatenated content snippets.
>
> """
>
> url =
> f"{self.endpoint}indexes/{self.index_name}/docs/search?api-version={self.api_version}"
>
> headers = {"Content-Type": "application/json", "api-key":
> self.api_key}
>
> body = {"search": query, "top": top}
>
> async with aiohttp.ClientSession() as session:
>
> async with session.post(url, headers=headers, json=body) as resp:
>
> if resp.status != 200:
>
> text = await resp.text()
>
> raise RuntimeError(f"Azure Search error {resp.status}: {text}")
>
> data = await resp.json()
>
> docs = data.get("value", \[\])
>
> snippets: List\[str\] = \[\]
>
> for d in docs:
>
> snippet = d.get("content") or d.get("text") or d.get("description") or
> json.dumps(d)
>
> snippets.append(snippet.strip())
>
> return "\n\n".join(snippets) if snippets else "No results found."
>
> async def health_check(self):
>
> """Check if Azure Search service is accessible."""
>
> try:
>
> url =
> f"{self.endpoint}indexes/{self.index_name}?api-version={self.api_version}"
>
> headers = {"Content-Type": "application/json", "api-key":
> self.api_key}
>
> async with aiohttp.ClientSession() as session:
>
> async with session.get(url, headers=headers) as resp:
>
> return {
>
> "status": "healthy" if resp.status == 200 else "unhealthy",
>
> "status_code": resp.status,
>
> "endpoint": self.endpoint,
>
> "index": self.index_name
>
> }
>
> except Exception as e:
>
> return {
>
> "status": "error",
>
> "error": str(e),
>
> "endpoint": self.endpoint,
>
> "index": self.index_name
>
> }
>
> \# Example usage and testing
>
> async def main():
>
> """Test the Azure Search tool"""
>
> try:
>
> \# Load environment variables
>
> from utils.env import load_env
>
> load_env()
>
> \# Initialize search tool
>
> search_tool = AzureSearchTool()
>
> \# Health check
>
> health = await search_tool.health_check()
>
> print(f"Health Status: {json.dumps(health, indent=2)}")
>
> \# Test search
>
> test_queries = \[
>
> "travel reimbursement policy",
>
> "GDPR compliance requirements",
>
> "employee leave policies"
>
> \]
>
> for query in test_queries:
>
> print(f"\n{'='\*60}")
>
> print(f"Testing search: {query}")
>
> print('='\*60)
>
> result = await search_tool.search(query, top=3)
>
> print(result)
>
> except Exception as e:
>
> print(f"Error testing Azure Search tool: {e}")
>
> if \_\_name\_\_ == "\_\_main\_\_":
>
> import asyncio
>
> asyncio.run(main())

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image9.png)

> **AzureSearchTool 的目的:**

- 該工具提供了一個異步兼容MCP的界面，供代理查詢Azure認知搜索索引並檢索與用戶查詢相關的事實上下文片段。

> **基於環境的配置:**

- 該工具直接從環境變量讀取 Azure Search 憑證——端點、API
  密鑰和索引名稱，確保配置安全且靈活。

> **核心搜索功能（搜索方法）:**

- search（） 方法向 Azure Search REST API 發送異步 POST
  請求，獲取頂匹配文檔並將其內容字段串接為單一上下文字符串。

> **測試與診斷模式:**

- 該文件 built-in main()
  例程，可加載環境變量、進行實時健康檢查，並運行樣本查詢，以便在智能體集成前快速獨立驗證。

12. 完成後，選擇 **File** **(1)** ，然後點擊 **Save** **(2)** 保存文件。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image10.png)

13. 選擇......**（1）**頂部菜單中的擴展菜單選項。選擇 **Terminal
    (2)** ，然後點擊 **New Terminal (3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

14. 請執行以下命令來測試搜索工具的工作原理。

+++python .\tools\azure_search_tool.py+++

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image12.png)

15. 你已經成功創建了一個MCP工具，將你的Azure AI
    Search數據與代理連接起來，使其能夠從你的索引知識庫中獲取相關上下文。

## 任務2：將工具附加到代理上，豐富提示，並運行測試

在此任務中，您將將 AzureSearchTool
連接到人力資源/財務/合規代理，更新編排以獲取上下文，並在調用代理前進行，並運行批處理和交互測試。

1.  既然你已經創建了工具，現在你必須更改代理的編排方式，以便在給出響應前觸發搜索工具。

2.  在 Visual Studio Code 資源管理器中，打開 **main.py**
    文件，並用下面提供的代碼替換其現有內容。

> import asyncio
>
> import time
>
> import logging
>
> import re
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
> from tools.azure_search_tool import AzureSearchTool
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
> Advanced multi-agent system with routing, search context, and ticket
> creation capabilities.
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
> \# Step 2: Retrieve relevant context using Azure Search
>
> logging.info("Retrieving context from knowledge base...")
>
> context = await agents\["search_tool"\].search(query, top=3)
>
> \# Step 3: Create enriched prompt with context
>
> enriched_prompt = f"""
>
> Context from Knowledge Base:
>
> {context}
>
> ---
>
> User Question: {query}
>
> Please provide a comprehensive answer based PRIMARILY on the context
> information provided above.
>
> Use the knowledge base content as your primary source of truth. If the
> context contains relevant
>
> information, base your answer on that. Only supplement with general
> knowledge if the context
>
> doesn't cover the specific question.
>
> If no relevant information is found in the context, clearly state that
> and provide general guidance
>
> while recommending the user contact the appropriate department for
> specific details.
>
> """
>
> \# Step 4: Get response from appropriate agent
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
> answer = await agents\[agent_key\].run(enriched_prompt)
>
> else:
>
> \# Fallback to HR if routing unclear
>
> logging.warning(f"Unknown target '{target}', falling back to HR")
>
> answer = await agents\["hr"\].run(enriched_prompt)
>
> target = "HR"
>
> agent_name = "HRAgent"
>
> answer_text = str(answer)
>
> \# Step 6: Process response
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
> "answer": answer_text,
>
> "context_retrieved": len(context) \> 100, \# Simple check if context
> was found
>
> "ticket_created": False, \# Tickets only created in interactive mode
>
> "ticket_info": None,
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
> "context_retrieved": False,
>
> "ticket_created": False,
>
> "ticket_info": None,
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
> context_icon = "📚" if result.get("context_retrieved") else "📭"
>
> ticket_icon = "🎫" if result.get("ticket_created") else ""
>
> formatted = f"""
>
> {status_icon} Agent Response Summary:
>
> ┌─ Routed to: {result\['routed_to'\]} ({result\['agent_name'\]})
>
> ├─ Response time: {result\['response_time'\]}s
>
> ├─ Context retrieved: {context_icon} {'Yes' if
> result.get('context_retrieved') else 'No'}
>
> ├─ Ticket created: {ticket_icon} {'Yes' if
> result.get('ticket_created') else 'No'}
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
> \# Add ticket details if available
>
> if result.get("ticket_info") and
> result\["ticket_info"\].get("success"):
>
> ticket = result\["ticket_info"\]\["ticket"\]
>
> formatted += f"""
>
> 🎫 Ticket Details:
>
> ├─ ID: \#{ticket\['id'\]}
>
> ├─ Status: {ticket\['status'\]}
>
> ├─ Priority: {ticket\['priority'\]}
>
> └─ URL: {ticket\['url'\]}
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
> - Ask any question about HR, Finance, or Compliance
>
> - 'quit' or 'exit' - Exit the system
>
> - 'help' - Show this help message
>
> 🎯 Example questions:
>
> - "What's the travel reimbursement limit for meals?"
>
> - "How many vacation days do employees get?"
>
> - "Do we need GDPR compliance for EU customers?"
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
> """Run focused test queries with grounded data integration."""
>
> test_queries = \[
>
> "What is the travel reimbursement limit for hotel stays?",
>
> "How many vacation days are allowed per year?"
>
> \]
>
> print("🧪 Running focused batch tests with grounded data
> integration...\n")
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
> await asyncio.sleep(1.0) \# Longer delay for tool operations
>
> async def main():
>
> """Main application entry point with enhanced features and tool
> integration."""
>
> print("🚀 Initializing Enterprise Agent System with Tools...")
>
> try:
>
> \# Load environment and build agents
>
> load_env()
>
> logging.info("Building agent network...")
>
> \# Build core agents
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
> \# Initialize and attach tools
>
> logging.info("Initializing tools...")
>
> try:
>
> search_tool = AzureSearchTool()
>
> agents\["search_tool"\] = search_tool
>
> \# Test search tool
>
> health = await search_tool.health_check()
>
> if health\["status"\] == "healthy":
>
> logging.info("✅ Azure Search tool initialized successfully")
>
> else:
>
> logging.warning(f"⚠️ Azure Search tool health check failed: {health}")
>
> except Exception as e:
>
> logging.error(f"Failed to initialize Azure Search tool: {e}")
>
> \# Create mock search tool for testing
>
> class MockSearchTool:
>
> async def search(self, query, top=3):
>
> return f"📭 Mock search results for: {query}\n(Azure Search tool not
> configured)"
>
> agents\["search_tool"\] = MockSearchTool()
>
> \# Freshdesk integration removed - focusing on grounded search
> responses only
>
> logging.info("✅ All agents and tools initialized")
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
> \# Try to run with minimal configuration
>
> logging.info("Attempting to run with minimal configuration...")
>
> try:
>
> minimal_agents = {
>
> "planner": await build_planner_agent(),
>
> "hr": await build_hr_agent(),
>
> "compliance": await build_compliance_agent(),
>
> "finance": await build_finance_agent(),
>
> "search_tool": type('MockSearch', (), {'search': lambda self, q,
> top=3: f"Mock search for: {q}"})()
>
> }
>
> await run_batch_tests(minimal_agents)
>
> except Exception as minimal_error:
>
> print(f"❌ Even minimal configuration failed: {minimal_error}")
>
> if \_\_name\_\_ == "\_\_main\_\_":
>
> asyncio.run(main())

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image13.png)

> **更新主文字的目的:**

- 該版本將早期編排器擴展為通過MCP集成Azure AI
  Search，使每個代理的響應能夠基於上下文，使用企業數據而非通用的LLM推理。

> **搜索工具集成（新功能）:**

- AzureSearchTool實例被初始化並作為代理\[“search_tool”\]附加到代理的字典中。

- 系統運行前，會進行健康檢查以確認Azure AI搜索的連接性和索引準備情況。

> **上下文豐富邏輯（增強run_multi_agent）:**

- 對於每個查詢，系統現在都會從Azure Search獲取相關的文本片段（context =
  waititagents\[“search_tool”\].search（query））。

- 返回的上下文會直接嵌入提示中，然後再發送給專業代理。

- LLM明確指示主要基於這些上下文數據來回答，確保回答基於事實。

3.  完成後，選擇 **File** **(1)** ，然後點擊 **Save** **(2)** 保存文件。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image10.png)

4.  選擇......**（1）**頂部菜單中的擴展菜單選項。選擇 **Terminal
    (2)** ，然後點擊 **New Terminal (3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

5.  終端打開後，執行以下命令運行代理並查看代碼文件中測試提示的響應。

+++python main.py+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image14.png)

檢查上下**Context
retrieved**參數，並檢查代理如何從接地數據中獲取上下文。

6.  現在，通過添加 --interactive flag
    再次運行交互模式。這樣你就可以輸入問題，得到回復。當提示提出時，請提供以下提示作為問題。

+++python main.py –interactive+++

+++Is employee data protected under GDPR?+++

7.  收到回復後，在下一個提示中添加q以退出或停止代理。

![A black screen with white text AI-generated content may be
incorrect.](./media/image15.png)

## 任務3：設置Freshworks用於工單管理

在此任務中，您將搭建並配置Freshworks，以啟用工單管理和多代理系統的企業集成。

**Freshworks**
是一個基於雲的客戶服務和互動平臺，旨在提升客戶支持運營和用戶滿意度。它提供一套工具，包括工單管理、在線聊天、幫助中心創建和客戶自助服務。Freshworks
支持全渠道通信，使企業能夠通過集中界面管理電子郵件、聊天、電話和社交媒體上的客戶互動。其自動化功能有助於簡化工作流程、分配工單，並提供績效跟蹤的分析。現在你要創建Freshworks賬戶。

1.  複製URL並粘貼到虛擬機內瀏覽器的新標簽頁，打開**Freshworks**門戶。

    - URL:

+++https://www.freshworks.com/freshdesk/lp/home/?tactic_id=3387224&utm_source=google-adwords&utm_medium=FD-Search-Brand-India&utm_campaign=FD-Search-Brand-India&utm_term=freshdesk&device=c&matchtype=e&network=g&gclid=EAIaIQobChMIuOK90qvLjQMV_dQWBR3JAi9VEAAYASAAEgK87_D_BwE&audience=kwd-30002131023&ad_id=282519464145&gad_source=1&gad_campaignid=671502402+++

2.  在門戶中，選擇 **Start free trial** 即可開始免費試用。

![](./media/image16.png)

3.  在下一頁輸入這些信息，並點擊“**Try it free (6)”**:

    - **First name:** +++LODS+++

    - **Last name:** +++User1+++

    - **Work
      email:** **+++@lab.CloudPortalCredential(User1).Username+++**

    - **Company name:** Zava

    - **Organization size:** 選擇 **1-10**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image17.png)

4.  在下一欄填寫這些信息，點擊**“Next (4)”**:

    - **What industry are you from ?:** 從列表中選擇**Software and
      internet (1)**

    - **How many employees are there in your company?:** 選擇 **1-10
      (2)**

    - 選擇 **I'm trying customer service software for the first time
      (3)**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.png)

5.  完成後，複製下面提供的URL，粘貼到虛擬機內瀏覽器的新標簽頁中，打開**Outlook**。

    - URL:
      +++https://go.microsoft.com/fwlink/p/?LinkID=2125442&clcid=0x409&culture=en-us&country=us+++

6.  在“pick an account”面板中，選擇你被分配給這個實驗的賬戶。

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

7.  在Freshworks驗證郵件中，打開並點擊“**Activate Account**”。

> ![A screenshot of a computer screen AI-generated content may be
> incorrect.](./media/image20.png)
>
> **注意：**如果您找不到Freshworks的激活郵件，請稍等幾分鐘，因為郵件發送可能會有延遲。如果郵件過了一段時間還沒到，可以考慮在新的私密/無痕窗口重新激活免費試用的步驟。另外，檢查垃圾郵件或垃圾郵件文件夾，因為郵件可能被過濾到了那裡。

8.  在下一欄，**Enter password (1)** ，**Confirm password (2)**
    輸入相同的密碼。**Activate your account (3)**。

> ![A screenshot of a login screen AI-generated content may be
> incorrect.](./media/image21.png)

9.  進入門戶後，點擊右上角的 **Profile (1)** 圖標，選擇 **Profile
    settings (2)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

10. 在個人資料頁面，點擊**“View API Key**”即可獲取 API 密鑰。

![A screenshot of a web page AI-generated content may be
incorrect.](./media/image23.png)

**注意：**如果您找不到該選項，請使用**CTRL + -**來最小化屏幕大小。

11. 在下一格，填寫 **CAPTCHA**。

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image24.png)

12. 請將API密鑰複製到記事本，你將會繼續使用它。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

13. 請在瀏覽器標簽頁複製顯示的**Account
    URL**，並將數值複製到記事本。你還會繼續使用它。

![](./media/image26.png)

14. 從左側點擊左側菜單中的**“Tickets**”圖標，可以看到一些默認的工單。

![A screenshot of a social media post AI-generated content may be
incorrect.](./media/image27.png)

15. 完成後，進入Visual Studio Code窗格，打開.env文件。

16. 在 .env 文件中，添加以下內容，並添加你之前複製的密鑰和域名 URL。

> \# Freshdesk Configuration
>
> FRESHDESK_DOMAIN=\[Domain_URL\]
>
> FRESHDESK_API_KEY=\[API_Key\]

![A black and white text with red lines AI-generated content may be
incorrect.](./media/image28.png)

![A computer screen shot of a program AI-generated content may be
incorrect.](./media/image29.png)

17. 完成後，選擇 **File** **(1)** ，然後點擊 **Save** **(2)** 保存文件。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image10.png)

## 任務4：將代理連接到外部API（Freshdesk MCP集成）

在這個任務中，你將創建一個額外的MCP工具，通過其REST
API將代理連接到外部Freshdesk實例。該工具將允許客服人員，尤其是財務和人力資源，在用戶請求報銷、差旅批准或政策澄清等作時創建真實工單

1.  環境變量配置好後，就可以創建處理與 Freshdesk 集成的工具了。

2.  在資源管理器菜單中，選擇 **tools (1)**文件夾，點擊 **Create File
    (2)** 選項。 

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.png)

3.  請提供文件名，freshdesk_tool.py。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

4.  現在，選擇該文件並添加以下代碼片段來配置工具。

> import os
>
> import aiohttp
>
> import base64
>
> import ssl
>
> from typing import Dict, Any, Optional
>
> class FreshdeskTool:
>
> """
>
> Async Freshdesk tool to create tickets via Freshdesk REST API.
>
> """
>
> def \_\_init\_\_(self):
>
> self.domain = os.getenv("FRESHDESK_DOMAIN")
>
> self.api_key = os.getenv("FRESHDESK_API_KEY")
>
> self.default_priority = int(os.getenv("FRESHDESK_DEFAULT_PRIORITY",
> "1") or 1)
>
> self.default_group_id = os.getenv("FRESHDESK_DEFAULT_GROUP_ID") or
> None
>
> if not self.domain or not self.api_key:
>
> raise RuntimeError("Freshdesk domain/API key missing in environment.")
>
> self.base_url = f"https://{self.domain}/api/v2"
>
> auth_bytes = f"{self.api_key}:X".encode("utf-8")
>
> auth_header = base64.b64encode(auth_bytes).decode("utf-8")
>
> self.headers = {
>
> "Authorization": f"Basic {auth_header}",
>
> "Content-Type": "application/json"
>
> }
>
> async def create_ticket(self, subject: str, description: str,
> requester: Optional\[Dict\[str, str\]\] = None, tags: Optional\[list\]
> = None) -\> Dict\[str, Any\]:
>
> url = f"{self.base_url}/tickets"
>
> payload: Dict\[str, Any\] = {
>
> "subject": subject,
>
> "description": description,
>
> "priority": self.default_priority,
>
> "status": 2 \# 2 = Open status in Freshdesk
>
> }
>
> if self.default_group_id:
>
> try:
>
> payload\["group_id"\] = int(self.default_group_id)
>
> except ValueError:
>
> pass
>
> if tags:
>
> payload\["tags"\] = tags
>
> if requester:
>
> if requester.get("email"):
>
> payload\["email"\] = requester.get("email")
>
> if requester.get("name"):
>
> payload\["name"\] = requester.get("name")
>
> \# Create SSL context that allows insecure connections for testing
>
> ssl_context = ssl.create_default_context()
>
> ssl_context.check_hostname = False
>
> ssl_context.verify_mode = ssl.CERT_NONE
>
> connector = aiohttp.TCPConnector(ssl=ssl_context)
>
> async with aiohttp.ClientSession(connector=connector) as session:
>
> async with session.post(url, headers=self.headers, json=payload) as
> resp:
>
> data = await resp.json()
>
> if resp.status not in (200, 201):
>
> raise RuntimeError(f"Freshdesk API error {resp.status}: {data}")
>
> ticket = {
>
> "id": data.get("id"),
>
> "status": data.get("status"),
>
> "priority": data.get("priority"),
>
> "url": f"https://{self.domain}/helpdesk/tickets/{data.get('id')}"
>
> }
>
> return {"success": True, "ticket": ticket, "raw": data}
>
> async def health_check(self):
>
> """Check Freshdesk connectivity by fetching sample endpoint (accounts
> may not allow GETs; this is best-effort)."""
>
> try:
>
> url = f"{self.base_url}/agents"
>
> \# Create SSL context that allows insecure connections for testing
>
> ssl_context = ssl.create_default_context()
>
> ssl_context.check_hostname = False
>
> ssl_context.verify_mode = ssl.CERT_NONE
>
> connector = aiohttp.TCPConnector(ssl=ssl_context)
>
> async with aiohttp.ClientSession(connector=connector) as session:
>
> async with session.get(url, headers=self.headers) as resp:
>
> return {"status": "healthy" if resp.status in (200, 403) else
> "unhealthy", "status_code": resp.status}
>
> except Exception as e:
>
> return {"status": "error", "error": str(e)}

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image32.png)

> **FreshdeskTool 的目的:**

- 本課程提供異步接口，將Freshdesk REST
  API集成到您的代理工作流程中，實現直接從代理作自動創建工單。

> **認證與設置:**

- 它通過基本認證使用你的 Freshdesk API 密鑰進行認證，並向
  /api/v2/tickets 端點構建請求。

- 基礎URL、API密鑰和默認配置（優先級、組ID）均可從環境變量中安全讀取。

> **工單創建邏輯:**

- create_ticket（） 方法構建結構化的 JSON
  負載，包括主題、描述、請求者信息和可選標簽。

- 它向 Freshdesk 發送異步 POST
  請求，創建真實工單並返回歸一化的元數據（ID、狀態、優先級、URL）。

5.  完成後，選擇 **File** **(1)** ，然後點擊 **Save** **(2)** 保存文件。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image10.png)

6.  在資源管理器菜單中選擇 main.py。

7.  用以下片段替換現有代碼。

> import asyncio
>
> import time
>
> import logging
>
> import re
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
> from tools.azure_search_tool import AzureSearchTool
>
> from tools.freshdesk_tool import FreshdeskTool
>
> \# Configure logging
>
> logging.basicConfig(level=logging.INFO, format='%(asctime)s -
> %(levelname)s - %(message)s')
>
> def parse_create_ticket_block(response_text: str) -\> Dict\[str,
> Any\]:
>
> """
>
> Parse CREATE_TICKET block from agent response.
>
> """
>
> if "CREATE_TICKET" not in response_text:
>
> return None
>
> \# Extract the CREATE_TICKET block
>
> lines = response_text.split('\n')
>
> ticket_start = -1
>
> for i, line in enumerate(lines):
>
> if line.strip() == "CREATE_TICKET":
>
> ticket_start = i
>
> break
>
> if ticket_start == -1:
>
> return None
>
> \# Parse ticket details
>
> ticket_data = {
>
> "subject": "",
>
> "body": "",
>
> "tags": \[\],
>
> "email": "system@enterprise.com",
>
> "name": "Enterprise System User"
>
> }
>
> \# Process lines after CREATE_TICKET
>
> for line in lines\[ticket_start + 1:\]:
>
> line = line.strip()
>
> if not line:
>
> continue
>
> if line.startswith("Subject:"):
>
> ticket_data\["subject"\] = line\[8:\].strip()
>
> elif line.startswith("Body:"):
>
> ticket_data\["body"\] = line\[5:\].strip()
>
> elif line.startswith("Tags:"):
>
> tags_str = line\[5:\].strip()
>
> if tags_str:
>
> ticket_data\["tags"\] = \[tag.strip() for tag in tags_str.split(',')\]
>
> elif line.startswith("Email:"):
>
> ticket_data\["email"\] = line\[6:\].strip()
>
> elif line.startswith("Name:"):
>
> ticket_data\["name"\] = line\[5:\].strip()
>
> return ticket_data
>
> async def run_multi_agent_with_user_info(query: str, agents:
> Dict\[str, Any\], user_name: str = None) -\> Dict\[str, Any\]:
>
> """
>
> Enhanced multi-agent system with CREATE_TICKET pattern support and
> user name handling.
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
> \# Step 2: Retrieve relevant context using Azure Search
>
> logging.info("Retrieving context from knowledge base...")
>
> context = await agents\["search_tool"\].search(query, top=3)
>
> \# Step 3: Create enriched prompt with context
>
> enriched_prompt = f"""
>
> Context from Knowledge Base:
>
> {context}
>
> ---
>
> User Question: {query}
>
> Please provide a comprehensive answer based on the context above. If
> no relevant context is found, provide your best guidance based on your
> training.
>
> """
>
> \# Step 4: Get agent response
>
> agent_key = target.lower()
>
> agent_name = f"{target}Agent"
>
> if agent_key in agents:
>
> logging.info(f"Processing with {agent_name}...")
>
> answer = await agents\[agent_key\].run(enriched_prompt)
>
> else:
>
> \# Fallback to HR if routing unclear
>
> logging.warning(f"Unknown target '{target}', falling back to HR")
>
> answer = await agents\["hr"\].run(enriched_prompt)
>
> target = "HR"
>
> agent_name = "HRAgent"
>
> answer_text = str(answer)
>
> \# Step 5: Check for CREATE_TICKET pattern in response
>
> ticket_info = None
>
> ticket_created = False
>
> ticket_data = parse_create_ticket_block(answer_text)
>
> if ticket_data and "freshdesk_tool" in agents:
>
> logging.info("CREATE_TICKET pattern detected - creating Freshdesk
> ticket")
>
> \# Use provided user name if available
>
> if user_name:
>
> ticket_data\["name"\] = user_name
>
> logging.info(f"Using provided user name: {user_name}")
>
> try:
>
> \# Create ticket using parsed data
>
> ticket_result = await agents\["freshdesk_tool"\].create_ticket(
>
> subject=ticket_data\["subject"\] or f"{target} Request:
> {query\[:60\]}...",
>
> description=ticket_data\["body"\] or f"Request: {query}\n\nAgent
> Response:\n{answer_text}",
>
> tags=ticket_data\["tags"\] or \[target.lower(), "agent-system"\],
>
> requester={
>
> "name": ticket_data\["name"\],
>
> "email": ticket_data\["email"\]
>
> }
>
> )
>
> if ticket_result.get("success"):
>
> ticket_info = ticket_result
>
> ticket_created = True
>
> ticket_id = ticket_result.get("ticket", {}).get("id")
>
> ticket_url = ticket_result.get("ticket", {}).get("url")
>
> \# Replace CREATE_TICKET block with success message
>
> if "CREATE_TICKET" in answer_text:
>
> \# Remove the CREATE_TICKET block and replace with success message
>
> lines = answer_text.split('\n')
>
> filtered_lines = \[\]
>
> skip_ticket_block = False
>
> for line in lines:
>
> if line.strip() == "CREATE_TICKET":
>
> skip_ticket_block = True
>
> \# Add success message with user name
>
> success_msg = f"""
>
> 🎫 \*\*Support Ticket Created Successfully\*\*
>
> - Ticket ID: \#{ticket_id}
>
> - Subject: {ticket_data\["subject"\]}
>
> - Requester: {ticket_data\["name"\]}
>
> - Status: Open
>
> - URL: {ticket_url}
>
> Your request has been submitted to our {target} team. You will receive
> updates via email.
>
> """
>
> filtered_lines.append(success_msg)
>
> continue
>
> elif skip_ticket_block and (line.startswith("Subject:") or
> line.startswith("Body:") or
>
> line.startswith("Tags:") or line.startswith("Email:") or
>
> line.startswith("Name:")):
>
> continue
>
> else:
>
> skip_ticket_block = False
>
> filtered_lines.append(line)
>
> answer_text = '\n'.join(filtered_lines)
>
> else:
>
> answer_text += f"\n\n⚠️ \*\*Note\*\*: Could not create support ticket:
> {ticket_result.get('error', 'Unknown error')}"
>
> except Exception as e:
>
> logging.error(f"Failed to create Freshdesk ticket: {e}")
>
> answer_text += f"\n\n⚠️ \*\*Note\*\*: Ticket creation failed:
> {str(e)}"
>
> \# Step 6: Process response
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
> "answer": answer_text,
>
> "context_retrieved": len(context) \> 100, \# Simple check if context
> was found
>
> "ticket_created": ticket_created,
>
> "ticket_info": ticket_info,
>
> "response_time": round(response_time, 2),
>
> "timestamp": time.strftime("%Y-%m-%d %H:%M:%S"),
>
> "success": True,
>
> "user_name": user_name
>
> }
>
> except Exception as e:
>
> logging.error(f"Multi-agent processing error: {e}")
>
> return {
>
> "query": query,
>
> "routed_to": "Error",
>
> "agent_name": "ErrorHandler",
>
> "answer": f"I encountered an error processing your request: {str(e)}.
> Please try again.",
>
> "context_retrieved": False,
>
> "ticket_created": False,
>
> "ticket_info": None,
>
> "response_time": 0,
>
> "timestamp": time.strftime("%Y-%m-%d %H:%M:%S"),
>
> "success": False,
>
> "user_name": user_name
>
> }
>
> async def run_multi_agent(query: str, agents: Dict\[str, Any\]) -\>
> Dict\[str, Any\]:
>
> """
>
> Wrapper for multi-agent system with no user name.
>
> """
>
> return await run_multi_agent_with_user_info(query, agents, None)
>
> def format_response(result: Dict\[str, Any\]) -\> str:
>
> """Format the agent response for display."""
>
> status_icon = "✅" if result\["success"\] else "❌"
>
> context_icon = "📚" if result.get("context_retrieved") else "📭"
>
> ticket_icon = "🎫" if result.get("ticket_created") else ""
>
> formatted = f"""
>
> {status_icon} Agent Response Summary:
>
> ┌─ Routed to: {result\['routed_to'\]} ({result\['agent_name'\]})
>
> ├─ Response time: {result\['response_time'\]}s
>
> ├─ Context retrieved: {context_icon} {'Yes' if
> result.get('context_retrieved') else 'No'}
>
> ├─ Ticket created: {ticket_icon} {'Yes' if
> result.get('ticket_created') else 'No'}
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
> \# Add ticket details if available
>
> if result.get("ticket_info") and
> result\["ticket_info"\].get("success"):
>
> ticket = result\["ticket_info"\]\["ticket"\]
>
> formatted += f"""
>
> 🎫 Ticket Details:
>
> ├─ ID: \#{ticket\['id'\]}
>
> ├─ Status: {ticket\['status'\]}
>
> ├─ Priority: {ticket\['priority'\]}
>
> └─ URL: {ticket\['url'\]}
>
> """
>
> return formatted
>
> async def interactive_ticket_creation(agents: Dict\[str, Any\],
> base_query: str) -\> Dict\[str, Any\]:
>
> """
>
> Simple interactive ticket creation.
>
> """
>
> print("\n🎫 \*\*Manual Ticket Creation\*\*")
>
> print("I'll help you create a support ticket manually.\n")
>
> try:
>
> \# Get basic ticket details
>
> subject = input(f"📝 Ticket Subject: ").strip() or f"Manual Request:
> {base_query\[:60\]}..."
>
> print("\n📄 \*\*Ticket Description\*\* (press Enter twice when
> done):")
>
> description_lines = \[f"Original Request: {base_query}", ""\]
>
> while True:
>
> line = input(" ").strip()
>
> if not line:
>
> break
>
> description_lines.append(line)
>
> description = "\n".join(description_lines)
>
> \# Create the ticket directly
>
> print(f"\n🚀 Creating ticket: '{subject}'...")
>
> ticket_result = await agents\["freshdesk_tool"\].create_ticket(
>
> subject=subject,
>
> description=description,
>
> tags=\["manual", "interactive"\],
>
> requester={
>
> "name": "Enterprise System User",
>
> "email": "system@enterprise.com"
>
> }
>
> )
>
> if ticket_result.get("success"):
>
> ticket_info = ticket_result.get("ticket", {})
>
> print(f"""
>
> ✅ \*\*Ticket Created Successfully!\*\*
>
> 🎫 Ticket Details:
>
> • ID: \#{ticket_info.get('id')}
>
> • Subject: {subject}
>
> • Status: Open
>
> • URL: {ticket_info.get('url')}
>
> 📧 You will receive email updates about your ticket status.
>
> """)
>
> return {
>
> "success": True,
>
> "ticket_created": True,
>
> "ticket_info": ticket_result
>
> }
>
> else:
>
> print(f"❌ \*\*Failed to create ticket\*\*:
> {ticket_result.get('error', 'Unknown error')}")
>
> return {"success": False, "ticket_created": False}
>
> except KeyboardInterrupt:
>
> print("\n🚫 Ticket creation cancelled.")
>
> return {"success": False, "ticket_created": False}
>
> except Exception as e:
>
> print(f"❌ \*\*Error during ticket creation\*\*: {str(e)}")
>
> return {"success": False, "ticket_created": False}
>
> async def run_interactive_mode(agents: Dict\[str, Any\]):
>
> """Interactive mode for real-time queries with enhanced ticket
> creation."""
>
> print("\n🤖 Enterprise Agent System - Interactive Mode")
>
> print("Available agents: HR, Finance, Compliance")
>
> print("Type 'quit' to exit, 'help' for commands, 'ticket' for
> interactive ticket creation\n")
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
> - Ask any question about HR, Finance, or Compliance
>
> - 'ticket' - Interactive ticket creation mode
>
> - 'quit' or 'exit' - Exit the system
>
> - 'help' - Show this help message
>
> 🎯 Example questions:
>
> - "What's the travel reimbursement limit for meals?"
>
> - "I need to create a ticket for sick leave"
>
> - "Can you help me submit a reimbursement request?"
>
> - "How many vacation days do employees get?"
>
> - "Do we need GDPR compliance for EU customers?"
>
> 🎫 Ticket Creation:
>
> - Use 'ticket' command for guided ticket creation
>
> - Or include phrases like "create ticket", "submit request" in your
> question
>
> - For LEAVE and REIMBURSEMENT requests, you'll be prompted for your
> name
>
> """)
>
> continue
>
> elif query.lower() == 'ticket':
>
> if "freshdesk_tool" not in agents:
>
> print("❌ Ticket creation is not available (Freshdesk tool not
> configured)")
>
> continue
>
> base_query = input("📝 Describe what you need help with: ").strip()
>
> if base_query:
>
> await interactive_ticket_creation(agents, base_query)
>
> continue
>
> elif not query:
>
> continue
>
> \# Check if this is a leave or reimbursement request that needs user
> name
>
> query_lower = query.lower()
>
> is_leave_request = any(word in query_lower for word in \["leave",
> "vacation", "sick", "time off", "pto", "holiday"\])
>
> is_reimbursement_request = any(word in query_lower for word in
> \["reimburse", "expense", "travel", "receipt", "reimbursement"\])
>
> wants_ticket = any(keyword in query_lower for keyword in \["create
> ticket", "submit ticket", "file ticket", "raise ticket",
>
> "create request", "submit request", "file request", "need help with",
>
> "open ticket", "new ticket", "support ticket", "help ticket"\])
>
> user_name = None
>
> if (is_leave_request or is_reimbursement_request) and wants_ticket:
>
> print("\n👤 For leave and reimbursement requests, I need to collect
> some information:")
>
> user_name = input("Please enter your name: ").strip()
>
> if not user_name:
>
> print("❌ Name is required for this type of request. Please try
> again.")
>
> continue
>
> print(f"✅ Thank you, {user_name}! Processing your request...")
>
> print("\n🤔 Processing your query...")
>
> result = await run_multi_agent_with_user_info(query, agents,
> user_name)
>
> print(format_response(result))
>
> print() \# Add spacing between queries
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
> print()
>
> async def run_batch_tests(agents: Dict\[str, Any\]):
>
> """Run focused test with ticket creation."""
>
> test_queries = \[
>
> "I need to create a ticket for my travel reimbursement request"
>
> \]
>
> print("🧪 Running focused batch tests with grounded data
> integration...\n")
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
> await asyncio.sleep(1.0) \# Longer delay for tool operations
>
> async def main():
>
> """Main application entry point with enhanced features and tool
> integration."""
>
> print("🚀 Initializing Enterprise Agent System with Tools...")
>
> try:
>
> \# Load environment and build agents
>
> load_env()
>
> logging.info("Building agent network...")
>
> \# Build core agents
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
> \# Initialize and attach tools
>
> logging.info("Initializing tools...")
>
> try:
>
> search_tool = AzureSearchTool()
>
> agents\["search_tool"\] = search_tool
>
> \# Test search tool
>
> health = await search_tool.health_check()
>
> if health\["status"\] == "healthy":
>
> logging.info("✅ Azure Search tool initialized successfully")
>
> else:
>
> logging.warning(f"⚠️ Azure Search tool health check failed: {health}")
>
> except Exception as e:
>
> logging.error(f"Failed to initialize Azure Search tool: {e}")
>
> \# Create mock search tool for testing
>
> class MockSearchTool:
>
> async def search(self, query, top=3):
>
> return f"📭 Mock search results for: {query}\n(Azure Search tool not
> configured)"
>
> agents\["search_tool"\] = MockSearchTool()
>
> \# Initialize Freshdesk tool for ticket creation
>
> try:
>
> freshdesk_tool = FreshdeskTool()
>
> agents\["freshdesk_tool"\] = freshdesk_tool
>
> logging.info("✅ Freshdesk tool initialized successfully")
>
> except Exception as e:
>
> logging.warning(f"⚠️ Freshdesk tool initialization failed: {e}")
>
> \# System will work without Freshdesk, just won't create tickets
>
> logging.info("✅ All agents and tools initialized")
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
> \# Try to run with minimal configuration
>
> logging.info("Attempting to run with minimal configuration...")
>
> try:
>
> minimal_agents = {
>
> "planner": await build_planner_agent(),
>
> "hr": await build_hr_agent(),
>
> "compliance": await build_compliance_agent(),
>
> "finance": await build_finance_agent(),
>
> "search_tool": type('MockSearch', (), {'search': lambda self, q,
> top=3: f"Mock search for: {q}"})()
>
> }
>
> await run_batch_tests(minimal_agents)
>
> except Exception as minimal_error:
>
> print(f"❌ Even minimal configuration failed: {minimal_error}")
>
> if \_\_name\_\_ == "\_\_main\_\_":
>
> asyncio.run(main())

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image33.png)

> **run_multi_agent_with_user_info():**

- 該功能擴展了原有的多智能體邏輯，以處理個性化用戶交互和自動工單創建。

- 它檢測到客服回復中的CREATE_TICKET塊，提取詳細信息（主題、正文、標簽、請求者信息），並自動觸發Freshdesk工單創建。

- 工單創建後，系統會動態用顯示工單ID、主題和URL的確認信息替換該封鎖。

> **interactive_ticket_creation():**

- 引入了引導式手動工單創建流程，用戶可以直接通過提示輸入工單主題和描述。

- 然後它異步調用Freshdesk工具創建工單，並在終端中交互式顯示確認信息。

8.  完成後，選擇 **File** **(1)** ，然後點擊 **Save** **(2)**保存文件。

![](./media/image10.png)

9.  由於主編排器已設置好，你需要給每個代理添加創建工單的指令。

10. 從左側面板選擇**finance_agent.py**，在文件中找到“指令”，該指令會給代理提供指令。

11. 在現有說明下方添加以下說明。

> "If the user asks to create a ticket (phrases like \\create a
> ticket\\, \\submit a reimbursement request\\, \\open a support
> ticket\\), output a structured block starting with:\n"
>
> "CREATE_TICKET\n"
>
> "Subject: \<one-line subject\>\n"
>
> "Body: \<detailed description\>\n"
>
> "Tags: tag1,tag2 (optional)\n"
>
> "Email: user@example.com (optional)\n"
>
> "Name: John Doe (optional)\n"
>
> "Return only the CREATE_TICKET block when requesting a ticket; do not
> call any APIs yourself."

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image34.png)

12. 完成後，請保存文件。

13. 從左側面板選擇**hr_agent.py**，在文件中找到“指令”，該指令為代理提供指令。

14. 在現有說明下方添加以下說明。

> "If the user asks to create a ticket (phrases like \\create a
> ticket\\, \\submit a leave request\\, \\open a support ticket\\),
> output a structured block starting with:\n"
>
> "CREATE_TICKET\n"
>
> "Subject: \<one-line subject\>\n"
>
> "Body: \<detailed description\>\n"
>
> "Tags: tag1,tag2 (optional)\n"
>
> "Email: john.doe@example.com (optional)\n"
>
> "Name: John Doe (optional)\n"
>
> "Return only the CREATE_TICKET block when requesting a ticket; do not
> call any APIs yourself.\n\n"
>
> "Provide specific, actionable guidance with policy references where
> applicable. "
>
> "Be empathetic and professional in your responses."

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image35.png)

15. 完成後，選擇 **File** **(1)** ，然後點擊 **Save** **(2)** 保存文件。

![](./media/image10.png)

16. 選擇......**（1）**頂部菜單中的擴展菜單選項。選擇 **Terminal
    (2)**，然後點擊 **New Terminal (3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

17. 終端打開後，執行以下命令運行代理並查看響應和工單創建。

+++python main.py+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.png)

18. 進入Freshdesk門戶，從左側菜單選擇 **Tickets**，檢查新創建的工單。

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image37.png)

**注意：**如果您無法查看工單，請等待幾秒鐘或刷新頁面以查看。

19. 現在，通過添加 --interactive flag
    再次運行交互模式。這樣你就可以輸入問題，得到回復。當提示提出時，請提供以下提示作為問題。

我需要為我的差旅報銷申請創建工單。我於2025年10月15日至18日前往紐約市參加年度客戶大會。目的是向關鍵利益相關者展示我們的新產品線，並參加戰略規劃會議。我的開銷包括機票（650美元）、酒店住宿（3晚480美元）、餐飲（320美元）、本地交通（85美元）和會議資料（45美元）。總報銷金額為1,580美元。我已經準備好所有收據可以提交。

20. 回到Freshworks查看新創建的工單，查看客服如何根據給出的信息添加細節。

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image38.png)

![A screenshot of a travel reimbursement AI-generated content may be
incorrect.](./media/image39.png)

21. 收到回復後，在下一個提示中添加q以退出或停止代理。

![A black screen with white text AI-generated content may be
incorrect.](./media/image15.png)

22. 您已成功構建了一個多代理系統，能夠從Azure AI
    Search中獲取上下文知識，並無縫集成Freshdesk以實現企業工單管理。

**摘要**

在本實驗室中，你成功測試了完整的多代理系統，代理從Azure AI
Search檢索相關信息，確定處理每個查詢的合適部門，並在必要時創建Freshdesk工單。這展示了代理如何將上下文推理與現實作結合起來，使其在企業工作流程中更加高效和實用。

你已經成功完成了這個實驗。請點擊“Next \>\>”繼續。
