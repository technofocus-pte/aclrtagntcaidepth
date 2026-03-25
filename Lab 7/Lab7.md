# ラボ 7: MCP を使用した AI-Poweredチケット管理システムの構築

**推定所要時間**：60分

**概要**

このラボでは、Model Context Protocol (MCP)
を使用してエンタープライズエージェントシステムを実際の組織データや外部サービスに接続することで拡張します。Azure
AI Search
を統合し、インデックス化されたナレッジベースからコンテキストに基づいた応答を提供するとともに、Freshdesk
API
を接続してエージェントが人事や財務のチケット作成などの実際のアクションを実行できるようにします。

このラボを完了すると、エージェントは静的な会話モデルから、エンタープライズ
システムと安全に対話できるインテリジェントでデータ対応のアクション駆動型アシスタントへと進化します。

ラボの目的

このラボでは次のタスクを実行します。

- タスク 1: Azure Search MCP ツールを構築する

- タスク 2:
  エージェントにツールをアタッチし、プロンプトを充実させ、テストを実行する

- チケット管理用のFreshworksの設定

- タスク 4: エージェントを外部 API に接続する (Freshdesk MCP 統合)

## タスク 1: Azure Search MCP ツールを構築する

このタスクでは、AI Search 資格情報を使用して環境変数を更新し、Azure AI
Search をクエリして上位 N 個のドキュメント スニペットを返す非同期ツール
クラスを作成します。エージェントはこれをコンテキストとして使用します。

MCPは、AIエージェントが構造化された入出力契約を通じて外部の知識やツールに安全にアクセスできるようにする標準規格です。エージェントはMCPによって、事実情報を取得し、APIを呼び出し、制御された監査可能な方法でアクションを実行できるようになります。これにより、基盤が確立され、拡張可能なエンタープライズAIシステムのバックボーンが形成されます。

1.  Azure ポータルで**agenticai**に移動し、リソース
    リストから**ai-knowledge-** Search serviceを選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

2.  左側のメニューの「Settings」から**Keys
    (1) **を選択し、図のようにコピー オプションを使用して**、Query Key
    (2)**をコピーします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

3.  コピーしたら、メモ帳などに安全に貼り付け、左のメニューにあるSearch
    Managementから**Indexes (1) **を選択し、**Index Name
    (2)**をコピーします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

4.  以前にマルチ・エージェント
    システムを作成したことがあるため、接続用の AI
    検索キーを追加する必要があるため、Visual Studio Code ペインで .env
    ファイルを選択します。

5.  .env ファイルで、AI Foundry キーの下に以下の部分を追加します。 **\[
    Query_Key \]**と**\[ Index_Name
    \]**の値は、先ほどコピーした値に置き換えてください。

> AZURE_SEARCH_ENDPOINT=https://ai-knowledge--@lab.LabInstance.Id.search.windows.net/
>
> AZURE_SEARCH_API_KEY=\[Query_Key\]
>
> AZURE_SEARCH_INDEX=\[Index_Name\]

6.  完了したら、ファイルを保存してください。上部のメニューから**file
    (1) **オプションをクリックし**、save
    (2)** を選択してファイルを保存します。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image4.png)

7.  **Create
    Folder **オプションをクリックし、プロンプトが表示されたらフォルダ名に「tools」と入力してください。ルートに新しいフォルダを作成してください。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

8.  作成後、フォルダー構造は次のようになります。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

9.  次に、先ほど作成した**tools
    (1)**フォルダを選択し、図のように**Create file
    (2)**をクリックします。これにより、ツールフォルダ内にファイルが作成されます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

10. ファイル名を azure_search_tool.py として指定します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

11. 作成したら、次のコード
    スニペットを追加して、エージェントの検索ツールを構成します。

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
>
> asyncio.run (メイン())

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image9.png)

> **AzureSearchToolの目的:**

- このツールは、エージェントが Azure Cognitive Search
  インデックスをクエリし、ユーザー クエリに関連する事実のコンテキスト
  スニペットを取得するための非同期 MCP
  互換インターフェイスを提供します。

> **環境ベースの構成:**

- このツールは、Azure Search の資格情報 (エンドポイント、API
  キー、インデックス名)
  を環境変数から直接読み取り、安全で柔軟な構成を保証します。

> **コア検索機能（検索方法）：**

- search() メソッドは、Azure Search REST API に非同期 POST
  要求を送信し、最も一致するドキュメントを取得し、そのコンテンツ
  フィールドを 1 つのコンテキスト文字列に連結します。

> **テストおよび診断モード:**

- ファイルには、環境変数を読み込み、ライブヘルスチェックを実行し、エージェント統合前の迅速なスタンドアロン検証用のサンプルクエリを実行する組み込みの
  main() ルーチンが含まれています。

12. 完了したら、**File（1）**を選択します。 **Save (2) **をクリックして、ファイルを保存します。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image10.png)

13. **...（1）**オプションを選択してメニューを拡張します。 **Terminal
    (2) **を選択し、 **New Terminal (3)**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

14. 検索ツールの動作をテストするには、以下のコマンドを実行します。

+++python .\tools\azure_search_tool.py+++

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image12.png)

15. Azure AI Search
    データをエージェントに接続し、インデックス付けされたナレッジ
    ベースから関連するコンテキストを取得できるようにする MCP
    ツールが正常に作成されました。

## タスク 2: エージェントにツールを添付し、プロンプトを充実させ、テストを実行する

AzureSearchToolを HR/Finance/Compliance
エージェントにアタッチし、エージェントを呼び出す前にコンテキストを取得するようにオーケストレーションを更新し、バッチ
テストと対話型テストを実行します。

1.  ツールを作成したので、応答を返す前に検索ツールをトリガーするようにエージェントのオーケストレーションを変更する必要があります。

2.  Visual Studio Code エクスプローラーで、
    **main.py**ファイルを開き、既存の内容を以下のコードに置き換えます。

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

> **更新されたメインスクリプトの目的:**

- このバージョンでは、以前のオーケストレーターが拡張され、MCP を介して
  Azure AI Search が統合され、一般的な LLM 推論ではなくエンタープライズ
  データを使用して各エージェントの応答がコンテキストに基づいて行われるようになります。

> **検索ツールの統合（新機能）:**

- AzureSearchToolインスタンスは初期化され、 agents\[" search_tool "\]
  としてエージェントディクショナリにアタッチされます。

- システムが実行される前に、Azure AI Search
  の接続性とインデックスの準備状況を確認するための正常性チェックが実行されます。

> **コンテキスト強化ロジック (拡張されたrun_multi_agent ):**

- すべてのクエリに対して、システムは Azure Search から関連するテキスト
  スニペットを取得するようになりました (context = await agents\["
  search_tool "\].search(query))。

- 返されたコンテキストは、専門のエージェントに送信される前にプロンプトに直接埋め込まれます。

- LLM
  は、事実に基づいた応答を確実にするために、主にこの文脈データに基づいて回答するように明示的に指示されています。

3.  完了したら、**File (1) **を選択します。 **Save** **(2)** をクリックして、ファイルを保存します。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image10.png)

4.  **...（1）**オプションを選択してメニューを拡張します。 **Terminal
    (2)を選択し、 New Terminal (3)**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

5.  ターミナルが開いたら、次のコマンドを実行してエージェントを実行し、コード
    ファイルで提供されるテスト プロンプトの応答を確認します。

+++python main.py+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image14.png)

**取得されたコンテキストパラメータ**を確認し、エージェントがグラウンディングされたデータからコンテキストを取得する方法を確認します。

6.  --interactive
    フラグを追加して、エージェントを対話モードで再度実行します。これにより、質問を入力して応答を取得できるようになります。以下のプロンプトを質問として入力してください。

+++python main.py –interactive+++

+++Is employee data protected under GDPR?+++

7.  応答を取得したら、次のプロンプトで q
    を追加してエージェントを終了するか、エージェントを停止します。

![A black screen with white text AI-generated content may be
incorrect.](./media/image15.png)

## チケット管理用のFreshworksの設定

Freshworksをセットアップして構成し、マルチ・エージェント
システムのチケット管理とエンタープライズ統合を有効にします。

**Freshworksは**、カスタマーサポート業務の改善とユーザー満足度の向上を目的として設計されたクラウドベースのカスタマーサービスおよびエンゲージメントプラットフォームです。チケット管理、ライブチャット、ヘルプセンター構築、カスタマーセルフサービスのためのツールスイートを提供しています。Freshworksはオムニチャネルコミュニケーションをサポートし、メール、チャット、電話、ソーシャルメディアを通じた顧客とのインタラクションを一元管理できるインターフェースを提供します。自動化機能により、ワークフローの効率化、チケットの割り当て、パフォーマンス追跡のための分析機能を提供します。それでは、
Freshworksアカウントの設定を行いましょう。

1.  URL をコピーし、VM 内のブラウザの新しいタブに貼り付けて、
    **Freshworks**ポータルを開きます。

    - URL:

+++https://www.freshworks.com/freshdesk/lp/home/?tactic_id=3387224&utm_source=google-adwords&utm_medium=FD-Search-Brand-India&utm_campaign=FD-Search-Brand-India&utm_term=freshdesk&device=c&matchtype=e&network=g&gclid=EAIaIQobChMIuOK90qvLjQMV_dQWBR3JAi9VEAAYASAAEgK87_D_BwE&audience=kwd-30002131023&ad_id=282519464145&gad_source=1&gad_campaignid=671502402+++

2.  ポータルで**「無料トライアルを開始」を選択して**、無料トライアルを開始します。

![](./media/image16.png)

3.  次のペインで、以下の詳細を入力し、「**無料で試す」(6)をクリックします**。

    - **First name:** +++LODS+++

    - **Last name:** +++User1+++

    - **Work
      email:** **+++@lab.CloudPortalCredential(User1).Username+++**

    - **Company name:** Zava

    - **Organization size:** Select **1-10**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image17.png)

4.  次のペインで、以下の詳細を入力し、 **Next (4)**をクリックします。

    - **What industry are you from ?:** リストから**Software and
      internet (1)**を選択してください

    - **How many employees are there in your
      company?:** **1～10人（2）**を選択します。

    - **I'm trying customer service software for the first time
      (3)**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.png)

5.  完了したら、以下の URL をコピーし、VM
    内のブラウザの新しいタブに貼り付けて**Outlook** を開きます。

    - URL:
      +++https://go.microsoft.com/fwlink/p/?LinkID=2125442&clcid=0x409&culture=en-us&country=us+++

6.  アカウントの選択ペインで、このラボに割り当てられているアカウントを選択します。

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

7.  Freshworksの確認メールを開いて、 **Activate
    Account**をクリックします。

> ![A screenshot of a computer screen AI-generated content may be
> incorrect.](./media/image20.png)
>
> **注：**
> Freshworksからのアクティベーションメールが見つからない場合は、メールの配信に遅延が発生している可能性があるため、数分お待ちください。しばらく経ってもメールが届かない場合は、新しいプライベートウィンドウまたはシークレットウィンドウで無料トライアルのアクティベーション手順を再度実行することを検討してください。また、メールがスパムフォルダまたは迷惑メールフォルダに振り分けられている可能性がありますので、ご確認ください。

8.  次のパネルで、 **Enter password (1) とConfirm password
    (2)**に同じパスワードを入力します。 **Activate your account
    (3)**をクリックします。

> ![A screenshot of a login screen AI-generated content may be
> incorrect.](./media/image21.png)

9.  ポータルに入ったら、右上隅にある**Profile
    (1) **アイコンをクリックし、**Profile settings (2)**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

10. プロフィールページで、 **「View API Key** **」**をクリックしてAPI
    キーを取得します。

![A screenshot of a web page AI-generated content may be
incorrect.](./media/image23.png)

**注意:**このオプションが見つからない場合は、 **CTRL +
-**を使用して画面サイズを最小化してください。

11. 次のペインで、 **CAPTCHA** を入力します。

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image24.png)

12. API キーをメモ帳にコピーしてください。後で使用します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

13. ブラウザタブから、表示されている**Account
    URL**をコピーし、その値をメモ帳にコピーしてください。この情報は後ほど使用します。

![](./media/image26.png)

14. **Tickets **アイコンをクリックすると、いくつかのデフォルトのチケットが表示されます。

![A screenshot of a social media post AI-generated content may be
incorrect.](./media/image27.png)

15. 完了したら、Visual Studio Code ペインに移動し、.env
    ファイルを開きます。

16. .env
    ファイルに次のコンテンツを追加し、先ほどコピーしたキーとドメイン URL
    を追加します。

> \# Freshdesk Configuration
>
> FRESHDESK_DOMAIN=\[Domain_URL\]
>
> FRESHDESK_API_KEY=\[API_Key\]

![A black and white text with red lines AI-generated content may be
incorrect.](./media/image28.png)

![A computer screen shot of a program AI-generated content may be
incorrect.](./media/image29.png)

17. 完了したら、**File (1) **を選択します。 **Save (2) **をクリックして、ファイルを保存します。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image10.png)

## タスク 4: エージェントを外部 API に接続する (Freshdesk MCP 統合)

このタスクでは、REST
APIを介してエージェントを外部のFreshdeskインスタンスに接続する追加のMCPツールを作成します。このツールにより、エージェント（特に財務および人事部門）は、ユーザーからの払い戻し、出張承認、ポリシーの明確化などのアクションのリクエストに応じて、実際のチケットを作成できるようになります。

1.  環境変数が設定されたら、Freshdesk
    との統合を処理するツールを作成します。

2.  エクスプローラーメニューから**tools (1)** フォルダを選択し、**Create
    File (2)**オプションをクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.png)

3.  ファイル名を freshdesk_tool.py として指定します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

4.  次に、ファイルを選択し、次のコード
    スニペットを追加してツールを構成します。

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

> **FreshdeskToolの目的:**

- このクラスは、Freshdesk REST API をエージェント
  ワークフローに統合するための非同期インターフェイスを提供し、エージェントのアクションから直接チケットを自動作成できるようにします。

> **認証とセットアップ:**

- Freshdesk API キーを使用して Basic Auth 経由で認証し、/ api
  /v2/tickets エンドポイントへのリクエストを構築します。

- ベース URL、API キー、およびデフォルト構成 (優先度、グループ ID)
  は、環境変数から安全に読み取られます。

> **チケット作成ロジック:**

- create_ticket ()
  メソッドは、件名、説明、要求者情報、オプションのタグを含む構造化された
  JSON ペイロードを構築します。

- Freshdesk に非同期 POST
  リクエストを送信し、実際のチケットを作成して、正規化されたメタデータ
  (ID、ステータス、優先度、URL) を返します。

5.  完了したら、**File (1) **を選択します。 **Save (2) **をクリックして、ファイルを保存します。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image10.png)

6.  エクスプローラー メニューから main.py を選択します。

7.  既存のコードを次のスニペットに置き換えます。

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

> **run_multi_agent_with_user_info ():**

- この機能は、元のマルチ・エージェント
  ロジックを拡張して、パーソナライズされたユーザー
  インタラクションと自動チケット作成を処理します。

- エージェントの応答で CREATE_TICKET ブロックを検出し、詳細
  (件名、本文、タグ、要求者情報) を抽出し、Freshdesk
  チケットの作成を自動的にトリガーします。

- チケットが作成されると、ブロックはチケット ID、件名、URL
  を示す確認メッセージに動的に置き換えられます。

> **interactive_ticket_creation():**

- ユーザーがプロンプトを通じてチケットの件名と説明を直接入力できる、ガイド付きの手動チケット作成フローを導入します。

- 次に、Freshdesk
  ツールを非同期的に呼び出してチケットを作成し、確認の詳細をターミナルに対話的に表示します。

8.  完了したら、**ファイルを選択します。** **（1）保存**をクリック **（２）**ファイルを保存します。

![](./media/image10.png)

9.  メインオーケストレーターが設定されているので、チケット作成のために各エージェントに指示を追加する必要があります。

10. **finance_agent.py**を選択し、ファイル内でエージェントへの指示を与える
    Instructions を見つけます。

11. 既存の手順の下に次の手順を追加します。

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

12. 完了したら、ファイルを保存してください。

13. **hr_agent.py**を選択し、ファイル内でエージェントへの指示を与える
    Instructions を見つけます。

14. 既存の手順の下に次の手順を追加します。

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
>
> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image35.png)

15. 完了したら、**File (1) **を選択します。 **Save** **(2)** をクリックファイルを保存します。

![](./media/image10.png)

16. **...（1）**オプションを選択してメニューを拡張します。 **Terminal
    (2)**を選択し、 **New Terminal (3)**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

17. ターミナルが開いたら、次のコマンドを実行してエージェントを実行し、応答とチケットの作成を確認します。

+++python main.py+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.png)

18. Freshdesk
    ポータルに移動し、左側のメニューから**\[Tickets \]**を選択して、作成された新しいチケットを確認します。

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image37.png)

**注:チケットが表示**されない場合は、数秒待つか、ページを更新して表示してください。

19. --interactive
    フラグを追加して、エージェントを対話モードで再度実行します。これにより、質問を入力して応答を取得できるようになります。以下のプロンプトを質問として入力してください。

I need to create a ticket for my travel reimbursement request. I
traveled to New York City from October 15-18, 2025, for the annual
client conference. The purpose was to present our new product line to
key stakeholders and attend strategic planning sessions. My expenses
include flights ($650), hotel accommodation ($480 for 3 nights), meals
($320), local transportation ($85), and conference materials ($45). The
total reimbursement amount is $1,580. I have all receipts ready for
submission.

20. Freshworksに戻り、作成された新しいチケットを確認し、エージェントが指定された詳細を使用して詳細をどのように追加したかを確認します。

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image38.png)

![A screenshot of a travel reimbursement AI-generated content may be
incorrect.](./media/image39.png)

21. 応答を取得したら、次のプロンプトで q
    を追加してエージェントを終了するか、エージェントを停止します。

![A black screen with white text AI-generated content may be
incorrect.](./media/image15.png)

22. Azure AI Search からコンテキスト ナレッジを取得し、エンタープライズ
    チケット管理のために Freshdesk
    とシームレスに統合するマルチ・エージェント システムを構築しました。

**まとめ**

このラボでは、エージェントがAzure AI
Searchから関連情報を取得し、各クエリを処理する適切な部門を特定し、必要に応じてFreshdeskチケットを作成する、完全なマルチ・エージェントシステムのテストに成功しました。これにより、エージェントがコンテキスト推論と実際のアクションを組み合わせることで、エンタープライズワークフローにおいてより効率的かつ実用的になる様子が実証されました。

このラボは正常に完了しました。「Next
\>\>」をクリックして次に進んでください。
