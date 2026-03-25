# ラボ 9: Azure AI フレームワークを使用した単一エージェントおよびマルチ・エージェント ワークフローの実装

**推定所要時間**: 45分

**概要**

Contoso Ltd. の AI エンジニアとして、Azure AI Framework
を用いたインテリジェントエージェントワークフローの開発を担当しています。このラボでは、MCP
を使用して外部ツールと統合するシングルエージェントシステムを構築し、複数の専門エージェントが連携したり、ユーザーの意図に基づいてタスクを動的にハンドオフしたりするマルチ・エージェント
ワークフローを設計します。

ラボの目的

このラボでは次のタスクを実行します。

- タスク 1: Azure OpenAI Chat エージェントの構築とテスト

- タスク 2: ツール統合による単一エージェントワークフローの作成

- タスク3: マルチ・エージェントワークフロー設計

  - タスク3.1: マルチ・エージェント ワークフローのオーケストレーション

  - タスク3.2: ハンドオフパターンのマルチ・エージェントシステム

## タスク0: ラボ環境のセットアップ

1.  C:\Labfiles\Day 2 から、**OpenAIWorkshop-Framework**
    ファイルを抽出します。

2.  LabVM デスクトップから **Visual Studio Code** をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

3.  **File (1)** を選択し、**Open
    Folder** **(2)** をクリックして**OpenAIWorkshop-Framework**フォルダーを開きます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

4.  C:\Labfiles\Day 2\\**OpenAIWorkshop-Framework**
    パスに移動し、**OpenAIWorkshop-Framework**
    を選択してから、フォルダーを選択します。

5.  「**Yes, I trust the authors**.」を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

6.  **省略記号(...)
    (1)**をクリックし、次に**Terminal** **(2)** をクリックして、**Terminal** **(3)**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

7.  以下のコマンドを入力して**applications** ディレクトリに移動し、**requirements.txt**
    ファイルから必要な依存関係をすべてインストールします。

> cd agentic_ai/applications
>
> pip install -r requirements.txt

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image5.png)

8.  コマンドが完了するまでに 5 ～ 10 分かかる場合があります。

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image6.png)

## タスク 1: Azure OpenAI チャットエージェントの構築とテスト

このタスクでは、Visual Studio Code でシンプルな Azure OpenAI
チャットエージェントを構築し、テストします。環境変数を設定し、エージェントをデプロイしたモデルに接続し、さまざまなプロンプトに基づいて動的な応答が生成される様子を確認します。

1.  **Visual Studio Code** に戻ります。

2.  pip install -r requirements.txt
    コマンドが正常に完了していることを確認してください。まだ実行中の場合は、完了するまでお待ちください。

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image6.png)

3.  **Explorer**から、**agentic_ai (1)** **\> applications (2)**
    を展開します。.env.sample **(3)**
    を右クリックし、**Rename** **(4)**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

4.  ファイルの名前を .env に変更し、クリックしてファイルを開きます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

5.  AZURE_OPENAI_API_KEY (1) と AZURE_OPENAI_ENDPOINT (2)
    の値を実際の値に置き換えます。これらの値はMicrosoft
    Foundryの**Overview**ページから取得してください。

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image9.png)

6.  AZURE_OPENAI_CHAT_DEPLOYMENTを**gpt-4o-mini (3)**として追加します。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image10.png)

7.  **File (1)** を選択し、**Save(2)**を選択します。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

8.  **application** **(1)** フォルダを右クリックし、**New
    file** **(2)** を選択して、単純なエージェントを構成するための新しいファイルを作成します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.png)

9.  エージェントファイルの名前を +++simple_agent_test.py+++ にします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

10. 次のコードをコピーしてファイルに貼り付けます。

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

11. **File (1)** を選択し、**Save(2)**を選択します。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

12. simple_agent_test.py (1) を右クリックし、**Open in Integrated
    Terminal** **(2)**を選択します。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image15.png)

13. 以下のコマンドを実行してエージェントを実行し、出力を観察してエージェントがどのように動作したかを理解します。

+++python simple_agent_test.py+++

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image16.png)

14. エージェントの応答を確認するために、命令を変更してみましょう。「Tell
    me a joke about the
    Earth **(1)** 」（行番号31）という命令を入力し、ファイルを**保存します**。次に、以下のコマンド**（2）**を実行し、エージェントの応答（**3）**を確認します。

+++python simple_agent_test.py+++

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image17.png)

15. これは、提供された指示に応じてエージェントの応答がどのように変化するかを示しており、さまざまなプロンプトに適応する能力を強調しています。

## タスク 2: ツール統合による単一エージェント ワークフローの作成

このタスクでは、MCP（Model Context
Protocol）を使用して外部ツールと統合する単一エージェントワークフローを構築およびテストします。環境変数を設定し、MCPサーバー、バックエンド、フロントエンドをローカルで実行し、エージェントがMCPツールを活用してユーザークエリを処理し、インテリジェントでコンテキストに応じた応答を返す様子を観察します。

1.  Visual Studio Code で、**agents** **(1)
    \>** **agent_framework** **(2)
    \>** **single_agent** **(3)** を展開し、MCPStreamableHTTPTool
    ツールが統合されたシングルエージェントワークフロー (4)
    を表示します。　

    - MCPStreamableHTTPTool を使用すると、エージェントは MCP
      サーバー経由で外部の HTTP
      ベースのサービスを呼び出し、会話にツール出力を含めることができます。

    - ChatAgentに渡され、指示とユーザーのプロンプトに基づいて自動的に使用されます。

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image18.png)

2.  コードを確認して、それがどのように統合されているかを理解してください。

    - \_maybe_create_tools メソッド内:

> ![A screen shot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

- これにより、MCP サーバーに接続されたストリーミング可能な HTTP
  ツールが作成されます。

- これにより、エージェントはワークフローの一部として外部サービス (MCP
  経由) への HTTP 呼び出しを実行できるようになります。

&nbsp;

- ツールは初期化中に ChatAgent に渡されます。

> ![A screen shot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

- エージェントは、ユーザー
  プロンプトによってツール呼び出しがトリガーされるたびにこのツールを使用できます。

- WebSocket によるストリーミング サポート:
  ストリーミングされた会話中にツール/関数が呼び出されると、\_chat_async_streaming
  を介してツール名とターンがブロードキャストされます。

3.  .env ファイル **(1)** に移動し、次のEnvironment変数を .env
    ファイルに追加して、実行する**Single agentワークフロー**を指定します
    (2)。

+++AGENT_MODULE=agents.agent_framework.single_agent+++

- DISABLE_AUTH=true **(3)**
  環境変数を追加します。これは、アプリケーション内で認証を無効にするために使用されます。これにより、ローカルでの開発とテストが容易になります。

> +++DISABLE_AUTH=true+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

4.  **File (1)** を選択し、**Save(2)**を選択します。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

5.  ここで、**MCP サーバー、バックエンド、React
    フロントエンド**を起動して、完全なエージェント環境をローカルで実行し、UI
    がエージェントやツールと対話できるようにします。

6.  Visual Studio Code ウィンドウで、**省略記号 (...) (1)**
    をクリックし、次に**Terminal (2)**をクリックして、**New Terminal
    (3)**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

7.  前のステップが完了するまで待ってから、次のステップに進みます。

8.  MCPサーバーを起動する（ターミナル1）: (mcp
    ディレクトリはプロジェクトのルートレベルにあります)

    - 以下のコマンドを実行して**MCPサーバー**を起動します。MCPサーバーは、エージェントがツールとして呼び出すことができるAPIを公開します。（サーバーは、http://localhost:8000で実行されます。）

> cd mcp
>
> uv run python mcp_service.py
>
> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image21.png)
>
> 注意: エラーが発生した場合は、以下のコマンドを実行してください。

+++pip install uv+++

+++uv run python mcp_service.py+++

9.  コマンドを実行し、新しいターミナルを開きます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

10. **バックエンドを起動する（ターミナル2）**:

    - 以下のコマンドを実行して、エージェント
      ワークフロー、セッション管理、および API
      エンドポイントをホストするバックエンド サーバーを起動します。

> cd agentic_ai/applications
>
> uv run python backend.py

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image22.png)

- ローカルで実行:[http://localhost:7000](http://localhost:7000/)。

- これはフロントエンドが通信するコアアプリケーションロジックです。**接続がオープンになっていること**を確認してください。

> ![A screen shot of a computer AI-generated content may be
> incorrect.](./media/image23.png)

11. コマンドを実行し、新しいターミナルを開きます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

12. **Reactフロントエンドを起動する（ターミナル3）**:

    - 以下のコマンドを入力して、 react-frontend
      ディレクトリに移動します。

> +++cd agentic_ai/applications/react-frontend+++

- 以下のコマンドを入力して、エージェントUI用の**Reactフロントエンド**を起動します。エージェントと対話し、その応答をリアルタイムで確認するためのユーザーインターフェースを提供します。

> +++npm start+++

- コンパイルには時間がかかる場合があります。警告は無視して完了するまでお待ちください。**Webpackのコンパイルが完了すると**、エージェントアプリケーションは次の場所でローカルに実行されます。[http://localhost:3000](http://localhost:3000/)。

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image24.png)

13. 3 つのターミナルがすべて実行されると、エージェント
    アプリケーションがブラウザーで起動し、エージェントと対話してその機能をテストできるようになります。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

**注記**:
3つのターミナルすべてが起動していることを確認してください。いずれかが停止している場合は、該当するコマンドを再実行してください。3つすべてが起動していない場合は、接続エラーが発生する可能性があります。

14. 以下のプロンプトをチャット**（1）**に送信し、応答**（2）**を表示します。

+++Customer 251, what's my billing summary?+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

**注記**:
3つのターミナルすべてが起動していることを確認してください。いずれかが停止している場合は、該当するコマンドを再実行してください。3つすべてが起動していない場合は、接続エラーが発生する可能性があります。

15. 出力を表示します。プロンプトを解釈し、おそらく MCP
    ツールを呼び出して出力を生成したのは ChatAgent (self.\_agent)
    でした。

    - エージェントはあなたのリクエストを**Customer
      251**.の請求に関する問い合わせであると解釈しました。

    - **MCP ツール**を使用して構造化された請求データを取得しました。

    - エージェントは意図したとおりに動作しており、ツールの出力と AI
      推論を動的に統合して、ユーザー固有の質問に答えます。

16. テストが完了したら、VS
    Codeに戻り、実行中のターミナルセッションをすべて終了します。これにより、以降のマルチ・エージェント
    ワークフローが干渉なく実行されるようになります。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

## タスク3: マルチ・エージェント ワークフロー設計

このタスクでは、様々な連携パターンを示す高度なマルチ・エージェント
ワークフローを設計・実装します。まず、中央管理ツールを介して複数の専門エージェントをオーケストレーションし、複雑なクエリを協調的に処理します。次に、ユーザーの意図に基づいてドメイン固有のエージェント間で制御が動的に移行するハンドオフベースのシステムを検討します。

### タスク3.1: マルチ・エージェント ワークフローのオーケストレーション

このタスクでは、中央オーケストレーターが複数の専門エージェントを調整して、複雑なユーザークエリを共同で処理し、正確なツールベースの応答を生成するマルチ・エージェント
ワークフローをオーケストレーションします。

1.  **agent (1) \> agent_framework (2) \> multi_agent (3) \>
    magentic_group (4)**に移動してコードを表示します **(5)。**

    - このコードは、中央オーケストレーターの指示の下で複数の専門エージェントが連携するシステムを定義するため、マルチ・エージェント
      オーケストレーション フレームワークを表します。

> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image28.png)

- \_create_participants は、複数の専門エージェント
  (CRM/請求、製品/プロモーション、セキュリティ/認証) を初期化します。

- 各エージェント:

  - 特定のドメインとツール セットがあります。

  - ユーザーと直接通信するのではなく、オーケストレーターとのみ通信します。

  - 事実に基づいた、ツールに裏付けられた応答を提供します。

- このマルチ・エージェント
  ワークフローで使用されるエージェントは次のとおりです。

  - **CRMおよび請求エージェント**–
    事実に基づいたツールベースのデータを使用して、顧客アカウント、サブスクリプション、課金、請求書、支払い、および関連するクエリを処理します。

  - **製品およびプロモーションエージェント**–
    構造化されたソースを使用して、製品の在庫状況、プロモーション、割引、資格、条件を提供します。

  - **セキュリティおよび認証エージェント**–
    ログとツールを使用して、セキュリティ
    インシデント、認証の問題、アカウント
    ロックアウト、リスク軽減ガイダンスを管理します。

2.  .env ファイル **(1)** に移動し、単一エージェント変数 **(2)**
    をコメント アウトし、以下のコマンドを入力して **Orchestrating
    Multi-Agent** 変数 **(3)** を追加します。

+++AGENT_MODULE=agents.agent_framework.multi_agent.magentic_group+++

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image29.png)

3.  **File (1)** を選択し、**Save(2)**を選択します。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

4.  次の手順に従って 3 つのコア
    コンポーネントを起動し、完全なエージェント
    アプリケーションを起動します。

5.  Visual Studio Code ウィンドウで、**省略記号 (...) (1)**
    をクリックし、次に**Terminal (2)**をクリックして、**New Terminal
    (3)**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

6.  **MCPサーバーを起動する（ターミナル1）**: (mcp
    ディレクトリはプロジェクトのルートレベルにあります)

    - 以下のコマンドを実行してMCPサーバーを起動します。MCPサーバーは、エージェントがツールとして呼び出すことができるAPIを公開します。（サーバーはhttp://localhost:8000で実行されます。）

> cd mcp
>
> uv run python mcp_service.py

7.  コマンドを実行し、新しいターミナルを開きます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

8.  **バックエンドを起動する（ターミナル2）**:

    - 以下のコマンドを実行して、エージェント
      ワークフロー、セッション管理、および API
      エンドポイントをホストするバックエンド サーバーを起動します。

> cd agentic_ai/applications
>
> uv run python backend.py

- これはフロントエンドが通信するコアアプリケーションロジックです。**接続が開いていること**を確認してください。

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image23.png)

9.  コマンドを実行し、新しいターミナルを開きます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

10. **Reactフロントエンドを起動する（ターミナル3）**:

    - 以下のコマンドを入力して、 react-frontend
      ディレクトリに移動します。

> +++cd agentic_ai/applications/react-frontend+++

- 以下のコマンドを入力して、エージェントUI用の**Reactフロントエンド**を起動します。エージェントと対話し、その応答をリアルタイムで確認するためのユーザーインターフェースを提供します。

> +++npm start +++

- Webpack が正常にコンパイルされると、エージェント
  アプリケーションは次の場所でローカルに実行されます。[http://localhost:3000](http://localhost:3000/)。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

11. 以下のプロンプトをチャットで送信し、左側のペインで応答を確認します。

+++Customer 251, what's my billing summary?+++

12. オーケストレーターはマネージャーやルーターのようなものです。ユーザークエリを読み取り、どの専門エージェントが処理すべきかを決定します。この決定には、コンテキストとキーワード（「billing」「promotion」「login」など）が使用されます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.png)

13. オーケストレーターはタスクをドメインエージェントに割り当てます。オーケストレーターはクエリを以下の内部エージェントのいずれかに送信します。

    - crm_billing – 請求、請求書、支払い

    - product_promotions – 商品、割引、オファー

    - security_authentication –
      セキュリティ、ログイン、アカウントのロックアウト

14. クエリ (「billing summary」) の場合、オーケストレーターはそれを
    crm_billing にルーティングします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

- ドメインエージェントは接続されたツールを使用します。各エージェントはMCPサーバーを介して特定のツール（API）にアクセスできます。

- 例: crm_billing can call get_customer_detail, get_billing_summary,
  get_invoice_payment etc.

- エージェントは適切なツールを呼び出し、構造化されたデータを取得し、事実に基づいた応答を形成します。

15. テストが完了したら、VS
    Codeに戻り、実行中のターミナルセッションをすべて終了します。これにより、以降のマルチ・エージェント
    ワークフローが干渉なく実行されるようになります。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

### タスク3.2: ハンドオフパターンのマルチ・エージェント システム

このタスクでは、ハンドオフベースのマルチ・エージェント
システムについて説明します。このシステムでは、ユーザーの意図に基づいて専門エージェント
(請求、プロモーション、セキュリティなど)
間で会話がシームレスに移行し、ドメイン間でスムーズでコンテキストに応じたやり取りが保証されます。

- **仕組み**

  - ユーザーはドメイン エージェント (CRM および請求エージェントなど)
    と直接対話します。

  - インテント分類器は、ユーザーの新しいメッセージが別のドメイン
    (プロモーションやセキュリティなど)
    に属しているかどうかを確認します。

  - そうであれば、システムは会話を適切な専門エージェントに自動的に転送（「ハンドオフ」）します。

  - 各エージェントには、そのドメイン（請求、プロモーション、セキュリティ）に関連するフィルターされたツールがあります。

  - ハンドオフはスムーズに行われ、コンテキスト転送により新しいエージェントが会話履歴を理解できるようになります。

1.  **agents (1) \> agent_framework (2) \> multi_agent (3) \>
    handoff_multi_domain_agent
    (4)** を展開し、Code **(5)**.を表示します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image32.png)

2.  .env ファイル **(1)** に移動し、Orchestrating Multi-Agent
    変数**(2)** をコメント アウトし、以下のコマンドを入力して Handoff
    Pattern Multi-Agent System 変数 **(3)** を追加します。

+++AGENT_MODULE=agents.agent_framework.multi_agent.handoff_multi_domain_agent+++

- ハンドオフ中に過去の会話コンテキストをどれだけ渡すかを制御するには、以下のコマンドを入力します。
  -1 は、以前の会話ターンをすべて転送することを意味します **(4)**。

> +++HANDOFF_CONTEXT_TRANSFER_TURNS=-1+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

3.  **File (1)** を選択し、**Save(2)**を選択します。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

4.  次の手順に従って 3 つのコア
    コンポーネントを起動し、完全なエージェント
    アプリケーションを起動します。

5.  Visual Studio Code ウィンドウで、**省略記号 (...) (1)**
    をクリックし、次に**Terminal (2)**をクリックして、**New Terminal
    (3)**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

6.  **MCPサーバーを起動する（ターミナル1）**: (mcp
    ディレクトリはプロジェクトのルートレベルにあります)

    - 以下のコマンドを実行してMCPサーバーを起動します。MCPサーバーは、エージェントがツールとして呼び出すことができるAPIを公開します。（サーバーはhttp://localhost:8000で実行されます）

> cd mcp
>
> uv run python mcp_service.py

7.  コマンドを実行し、新しいターミナルを開きます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

8.  **バックエンドを起動する（ターミナル2）**:

    - 以下のコマンドを実行して、エージェント
      ワークフロー、セッション管理、および API
      エンドポイントをホストするバックエンド サーバーを起動します。

> cd agentic_ai/applications
>
> uv run python backend.py

- これはフロントエンドが通信するコアアプリケーションロジックです。**接続が開いていること**を確認してください。

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image23.png)

9.  コマンドを実行し、新しいターミナルを開きます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

10. **Reactフロントエンドを起動する（ターミナル3）**:

    - 以下のコマンドを入力して、 react-frontend
      ディレクトリに移動します。

> +++cd agentic_ai/applications/react-frontend+++

- 以下のコマンドを入力して、エージェントUI用の**Reactフロントエンド**を起動します。エージェントと対話し、その応答をリアルタイムで確認するためのユーザーインターフェースを提供します。

> +++npm start +++

- **Webpack が正常にコンパイルされると**、エージェント
  アプリケーションは次の場所でローカルに実行されます。[http://localhost:3000](http://localhost:3000/)。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

11. 以下のプロンプトをチャットで送信し、左側のペインで応答を確認します。

+++Customer 251, what's my billing summary?+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image34.png)

- ここで、インテント分類器はcrm_billingドメインにルーティングされます

- get_billing_summaryツールが顧客251に対して呼び出されます

12. 請求に関する継続については、次のクエリを提供できます。

+++Yes, I would like to view the invoice details+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image35.png)

**注記**: もし、「I was unable to retrieve the invoice details because
the referenced numbers are invoice IDs, not subscription IDs. Please
provide the subscription ID, or let me know if you need details for a
specific invoice so I can assist you
correctly.」とのレスポンスが返されたら、以下のプロンプトを入力してください。

+++Yes, I would like to view the invoice details for customer 251+++

13. 次に、ハンドオフがどのように機能するかをテストするために、別のドメインに関連するクエリを試してみましょう。

14. 製品とプロモーションに関連する次のクエリを入力して、応答を表示します。

+++Are there any promotions available for my subscription plan+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image36.png)

- 前回の会話はCRM＆請求スペシャリストが担当していたため、システムはドメインの変更を検出し、会話を製品＆プロモーションスペシャリストに引き継ぐことにしました。

- システムは、HANDOFF_CONTEXT_TRANSFER_TURNS
  設定に応じて、以前の会話コンテキスト
  (どの顧客について話し合っているかなど)
  を新しいエージェントにオプションで転送します。

- 製品およびプロモーション
  スペシャリストは、プロモーション、プラン、製品情報に関連するツール
  (get_promotions、get_eligible_promotions など)
  にのみアクセスできます。

15. テストが完了したら、VS
    Codeに戻り、実行中のターミナルセッションをすべて終了します。これにより、以降のマルチ・エージェント
    ワークフローが干渉なく実行されるようになります。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

**まとめ**

このラボでは、MCPを使用して外部ツールと統合するシングルエージェントワークフローを作成し、複数の専門エージェントが連携したり、ユーザーの意図に基づいて会話を引き継いだりするマルチ・エージェント
設計を検討しました。環境変数を設定し、完全なエージェント環境を起動し、エージェントがドメイン固有のクエリにインテリジェントに応答する様子をテストしました。
