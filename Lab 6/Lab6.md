# ラボ6: Agent-to-Agent (A2A)通信によるマルチエージェントシステムの開発

**推定所要時間**：30分

**概要**

このラボでは、Microsoft Agent Framework
を使用してマルチエージェントシステムを構築します。個別のエージェントロール（Planner、人事、コンプライアンス）を定義し、展開し、A2A（エージェント間）通信を構成して、あるエージェントが他のエージェントに電話をかけられるようにします。ユーザークエリがエージェントネットワークを介して委任されるシナリオをテストし、トレースとログを検査してルーティングが正しいことを確認します。

Microsoft Agent Framework
SDKは、推論、アクションの実行、他のエージェントとの連携が可能なインテリジェントなモジュール型エージェントを構築するための、新しい公式開発キットです。以下の機能を提供します。

- 統合エージェントアーキテクチャ - AutoGen 、Semantic
  Kernel、断片化されたオーケストレーターを置き換えます

- Microsoft Foundry の組み込みサポート – エージェントを Foundry’s Agent
  Serviceに直接導入

- MCP（Model Context Protocol）によるツール –
  データ、API、システムとの標準化された統合

- ネイティブA2A通信 -
  エージェントは他のエージェントを自律的な協力者として呼び出すことができます

この SDK
は、信頼性、可観測性、ガバナンスが最初から組み込まれた、エンタープライズ
グレードの本番環境対応エージェント
システムをサポートするように設計されています。

ラボの目的

このラボでは次のタスクを実行します。

- タスク 1: 事前構成された VS Code プロジェクトを開く

- タスク2: Planner Agentを作成する

- タスク3: HR & COMPLIANCE WORKER AGENTSを作成する

- タスク4:
  A2Aルーティングロジック（エージェントグラフ/ワークフロー）を定義する

- タスク5: マルチエージェント会話のテストとログの検査

## タスク 1: 事前構成された VS Code プロジェクトを開く

このタスクでは、事前構成されたフォルダ構造を確認し、エージェント定義、ワークフロー、およびツールがどこに配置されているかを理解します。これにより、Microsoft
Agent Framework SDK を使用してシステムを拡張する準備が整います。

1.  LabVMデスクトップから**Visual Studio Code**を開きます。

2.  Visual Studio Codeが開いたら、 **File (1) **をクリックし、** Open
    Folder (2)**オプション** **を選択 してコードファイルフォルダを開きます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

3.  開いたフォルダー ペインで、C:\Labfiles\Day
    2\Enterprise-Agent-Code-Files に移動し、**select
    folder**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

4.  開くとポップアップ ウィンドウが開きます。 **\[Yes, I trust
    authors\]**オプションをクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

5.  エンタープライズ エージェントのフォルダー構造を確認してください。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

6.  **.env.example (1)**ファイルを右クリックし**、 Rename
    (2) **を選択してファイルの名前を変更します。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image5.png)

7.  完了したら、ファイルの名前を **.env.example --\> .env**
    に変更して**、**この環境ファイルをこのエージェントに対してアクティブにします。

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image6.png)

8.  .env ファイルの内容を以下の内容に置き換えます。

> AZURE_OPENAI_ENDPOINT=https://agentic-
> @lab.LabInstance.Id.cognitiveservices.azure.com/
>
> AZURE_OPENAI_API_KEY=**\<Replace with Azure OpenAI key\>**
>
> AZURE_OPENAI_RESPONSES_DEPLOYMENT_NAME=gpt-4o-mini
>
> AZURE_OPENAI_API_VERSION=2025-03-01-preview

Microsoft Foundry OverviewページからAPIキーをコピーし、

env ファイル内のプレース ホルダー**\<Replace with Azure OpenAI
key\>**を置き換えてください**。**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

9.  完了したら、**File (1) **を選択して、**Save (2) **をクリックします。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image9.png)

## タスク2: Planner Agentを作成する

このタスクでは、ユーザーのクエリを解釈し、タスクを委任する専門エージェントを決定するPlanner
Agentを定義します。Agent Framework
SDKを使用して、ロール固有の指示に従ってエージェントを設定します。

1.  リストから、エージェント
    フォルダーの下の**planner_agent.py**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.png)

2.  Planner エージェントを構成するには、次の Python コードを追加します。

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

> **Planner Agentの目的:**

- このエージェントは、ユーザーのクエリを分析し、どの専門エージェント
  (HR、財務、コンプライアンス)
  が応答を処理すべきかを決定するように設計されています。

> **AzureOpenAIResponsesClientを使用したエージェントの作成:**

- build_planner_agent () 関数は、環境変数から読み込まれた API
  ベースの資格情報を使用して、Agent Framework SDK を使用して Planner
  を初期化します。

> **LLM ガイドによる ルーティング (プライマリ ロジック):**

- Planner
  エージェントは、クエリ内のキーワードとコンテキストに基づいて、HR、FINANCE、または
  COMPLIANCE のいずれか 1 つの単語を返すように指示されます。

> **意思決定のためのclassify_target ()：**

- この関数はまず await agent.run ()
  呼び出しを使用して、Plannerにどのスペシャリストを選択するかを問い合わせます。応答が不明瞭な場合は、フォールバックキーワードベースの分析を適用します。

> **ハイブリッドAI + ヒューリスティック戦略:**

- この設計により、モデルの推論と手動のキーワードスコアリングが組み合わされ、信頼性の高いルーティングが保証され、AI
  出力が曖昧な場合でも Planner が堅牢になります。

3.  完了したら、**File (1)**を選択し、**Save** **(2)** をクリックして、ファイルを保存します。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

## タスク3: ワーカーエージェントを作成する

このタスクでは、人事、財務、コンプライアンスに関する知識を担当するドメイン固有のエージェントを開発します。各エージェントはエージェントレジストリに登録され、A2A通信による検出と委任が可能になります。

1.  リストから、エージェントフォルダ内の**hr_agent.pyを選択し、**以下のPythonコードを追加してhrエージェントを設定します。以下のPythonコードを追加してhrエージェントを設定します**。**

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

> **HRエージェントの目的:**

- このエージェントは、従業員の福利厚生、休暇制度、福利厚生、職場の手続きに関する質問に答えるよう訓練された専任の
  HR ポリシー専門家として機能します。

> **Azure Responses クライアントによるエージェントの初期化:**

- build_hr_agent () 関数は、環境変数に保存されている API
  キーとエンドポイント値を通じて認証されたAzureOpenAIResponsesClientを使用してエージェントを初期化します。

> **ドメイン固有の専門化:**

- 指示セクションでは、休暇の種類、福利厚生、オンボーディング、従業員関係、パフォーマンス管理など、HR
  エージェントの業務範囲が明確に定義されており、HR
  関連の問い合わせにのみ応答することが保証されます。

> **プロフェッショナルで共感的な口調:**

- このエージェントは実際の HR
  コミュニケーション標準を模倣するように設計されており、正確でプロフェッショナル、そして共感的なガイダンスを提供し、社内組織アシスタントに最適です。

> **マルチエージェントコラボレーションの基盤:**

- 構築されると、この HR エージェントは Planner
  Agentによって呼び出され、HR
  関連のクエリが検出されたときにマルチエージェント
  ワークフローで自動委任が可能になります。

2.  完了したら、**File (1) **を選択し、**Save (2) **をクリックして、ファイルを保存します。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

3.  リストから、エージェントフォルダの**finance_agent.py**を選択し、コンプライアンスエージェントを設定するための以下のPythonコードを追加します。財務エージェントを設定するための以下のPythonコードを追加します**。**

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

> 専門財務役割:

- このエージェントは、払い戻しポリシー、出張予算、手当、購入承認など、財務関連のあらゆるトピックを処理するように設計されています。

> **Agent Framework SDK による初期化:**

- build_finance_agent () 関数は、安全な環境変数からの API
  キー認証を活用して、
  AzureOpenAIResponsesClientを使用してエージェントを作成します。

> **ポリシーに焦点を当てた指示:**

- エージェントの指示により、エージェントの責任は財務手続きに明確に限定され、コスト、支払い、予算、および企業の経費規則に関する正確な回答が保証されます。

> **精度と実用的な出力:**

- 汎用エージェントとは異なり、この財務アシスタントは、制限、資格、承認フローなどの特定のポリシー値を提供するように指示されているため、従業員にとって実用的です。

> **Planner委任 (A2A) をサポートします:**

- このエージェントは、**Planner**が財務関連のキーワードまたはクエリを検出すると自動的に呼び出され、システム内でシームレスなマルチエージェントコラボレーションを可能にします。　

4.  完了したら、**File (1) **を選択し、**Save (2) **をクリックして、ファイルを保存します。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

5.  リストから、エージェントフォルダの**compliance_agent.py**を選択し、コンプライアンスエージェントを設定するための以下のPythonコードを追加します。コンプライアンスエージェントを設定するための以下のPythonコードを追加します。

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

> **エージェントの目的:**

- このエージェントは、GDPR、規制フレームワーク、契約法、リスク評価、セキュリティ標準に関連するクエリの処理を担当する専任の法務およびコンプライアンス機関として機能します。

> **エージェントの初期化:**

- build_compliance_agent () 関数は、API キー認証を備えた
  AzureOpenAIResponsesClient を使用して、 Microsoft Agent Framework SDK
  を通じてComplianceエージェントを登録します。

> **指示書で定義されている規制の専門知識:**

- この手順では、世界的なプライバシー規制
  (GDPR、HIPAA、SOX)、監査の準備、法的契約、違反プロトコルなどの明確なコンプライアンス範囲が提供され、信頼性の高い対応が保証されます。

> **トーンと出力の期待:**

- このエージェントは、該当する場合は法的な引用や実装の推奨事項など、正式で権威のある口調で回答を提供するように構成されています。

> **マルチ・エージェントシステムにおける役割:**

- A2A 委任中、Planner
  エージェントは法的またはコンプライアンス関連のクエリをこのスペシャリストにルーティングし、企業の意思決定ワークフローの正確性とガバナンスを維持します。

6.  完了したら、**File (1) **を選択し、**Save (2) **をクリックして、ファイルを保存します。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

## タスク4: A2Aルーティングロジック（エージェントグラフ/ワークフロー）を定義する

Agent-to-Agent (A2A) は、Microsoft Agent Frameworkのコア機能であり、1
つのエージェントが別のエージェントにタスクを自律的に委任できるようにします。

このタスクでは、エージェントワークフローを使用してルーティングロジックを実装し、Plannerがクエリの意図に基づいて人事エージェントまたはコンプライアンスエージェントに自律的に電話をかけられるようにします。これにより、真のマルチエージェントコラボレーションが確立されます。

1.  リストから、エージェントフォルダの**main.py**を選択し、以下の Python
    コードを追加して **A2A**
    通信フローエージェントを設定します。また、以下の Python
    コードを追加してエージェントルーティングロジックを設定します。

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

> **中央実行エンジン:**

- このスクリプトはコア
  オーケストレーターとして機能し、すべてのエージェント (Planner, HR,
  Finance, Compliance) を調整し、Microsoft Agent
  Frameworkを使用してマルチエージェント ルーティングを管理します。　

> **エージェントネットワークの初期化:**

- 環境設定を読み込み、await build\_\*\_agent()
  で各エージェントを構築し、簡単に委任できるように共有辞書に登録します。

> **高度なA2A ルーティング:**

- run_multi_agent ()
  関数は、Plannerを介してユーザークエリを適切なスペシャリストにルーティングし、スペシャリストエージェントからの応答を待ちます。この関数は、ルーティング、タイミング、成功ステータス、最終回答を取得します。

> **複数の実行モード:**

- バッチ モード: 事前定義されたテスト クエリを実行します。

- インタラクティブ モード (--interactive): ライブ
  テストと探索のためのリアルタイム チャットを有効にします。

> **本番対応の回復力:**

- レスポンスのフォーマット設定、タイムスタンプ、エラーフォールバックメカニズム、ロギングなどが含まれており、後の演習における可観測性、テレメトリ、エージェント運用管理のための強固な基盤となります。

2.  完了したら、**File (1) **を選択し、**Save (2) **をクリックして、ファイルを保存します。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

## タスク5: マルチ・エージェント会話のテストとログの検査

このタスクでは、マルチエージェント
システムを通じてエンド・ツー・エンドのテスト クエリを実行し、Microsoft
Foundry
のログとテレメトリを使用してエージェントのコラボレーションを観察します。

1.  Planner
    エージェントとWorkerエージェントを含むマルチ・エージェントシステムの構成が完了しました。次に、このマルチ・エージェントシステムの動作をテストします。

> **注：**マルチエージェントシステムは現在LLM機能で構成されていますが、MCPとの統合や、データセットやAzure
> AI
> Searchインデックスなどの外部知識ソースへのアクセスはまだありません。現段階では、エージェントは一般的なモデルのインテリジェンスのみを利用して質問に答えます。

2.  トップメニューから**...（1）**オプションを選択してメニューを拡張します。
    **Terminal（2）**を選択し、 **New Terminal (3)**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image16.png)

3.  ターミナルが開いたら、次のコマンドを実行します。

+++pip install -r requirements.txt+++ to install all the required
packages.

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image17.png)

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image18.png)

4.  インストールが正常に完了したら、次のコマンドを実行してエージェントを実行し、コード
    ファイルで提供されるテスト プロンプトの応答を確認します。

+++python main.py+++

> ![A computer screen shot of a program AI-generated content may be
> incorrect.](./media/image19.png)

**「ルーティング先」パラメータを**チェックして、エージェントがリクエストを決定し、それぞれのワーカーエージェントにルーティングする方法を確認します。

5.  --interactive
    フラグを追加して、エージェントを対話モードで再度実行します。これにより、質問を入力して応答を取得できるようになります。以下のプロンプトを質問として入力してください。

    - Command:

> +++python main.py –interactive+++

- Prompt:

> +++How much reimbursement is allowed for international flights?+++

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image20.png)

6.  応答を取得したら、次のプロンプトで「q」
    を追加してエージェントを終了するか、エージェントを停止します。

![A black screen with white text AI-generated content may be
incorrect.](./media/image21.png)

**まとめ**

このラボでは、Microsoft Agent Framework
SDKを使用して3つのエージェント（Planner、HR、Compliance）を定義し、登録しました。Agent-to-Agent通話を通じてユーザーからの問い合わせを委任するルーティングワークフローを構築しました。マルチエージェントシナリオをテストし、ログを検査してメッセージルーティングと実行フローが正しいことを確認しました。
