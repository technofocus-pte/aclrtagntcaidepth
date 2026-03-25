# Microsoft FoundryとAgent Frameworkを使用してスケーラブルなAIエージェントを設計する

**概要**

このハンズ・オン・ラボは 3 日間にわたり、Microsoft FoundryとMicrosoft
Agent
Frameworkを使用してスケーラブルなAIエージェントを設計および構築します。参加者はまず、Microsoft
Foundry ポータルで最初の AI エージェントを作成し、エンタープライズ
ポリシー ドキュメントをアップロードして Azure AI Search
に取り込んで検索可能なナレッジ
ベースを準備する方法を学習します。次に、ワークショップは Microsoft Agent
Framework SDK を使用してマルチ・エージェント
システムを構築します。このシステムでは、複数の特殊エージェントがAgent-to-Agent
(A2A) 通信パターンを介して連携します。学習者は、Model Context Protocol
(MCP) を使用して外部ツールとデータ ソースを統合し、ナレッジ検索用の
Azure AI Search とチケット管理用の Freshdesk などの外部 API
の両方に接続することで、エージェントの機能を拡張します。トレーニングは、状態管理とエンタープライズ
グレードの信頼性を備えた永続的なクラウド ホスト
ソリューションとしてエージェントを Microsoft Foundry Agent Service
に展開するところまで進みます。最後に、参加者は、集中的な調整機能を備えたオーケストレーションされたマルチ・エージェント
システムや、ユーザーの意図とドメインの専門知識に基づいて専門エージェント間で会話がシームレスに移行するハンドオフベースのシステムなど、高度なワークフロー
パターンを実装します。

**目的**

このラボを終了すると、次のことができるようになります。

- **AI プロジェクトを設定し、VS Code
  からチャット完了を実行する：**Microsoft Foundry
  プロジェクトを作成し、GPT-4 をデプロイしてモデルを埋め込み、Visual
  Studio Code から安全な接続を確立することで、本番環境に対応した AI
  開発環境を構築します。チャット完了呼び出しを実行してセットアップを検証し、適切な認証とプロジェクト構成によって、ローカル開発環境と
  Azure AI サービス間のシームレスな統合を実現します。

- **健康保険プラン分析 AI
  エージェントを構築する:**健康保険データの分析と可視化に特化したインテリジェントAIエージェントを開発します。複雑な健康保険プラン情報を処理し、比較棒グラフを自動生成するエージェントを作成し、データ解釈、natural
  language
  understanding、コード実行、意思決定支援のための自動可視化生成といったAIエージェントの中核機能を実証します。

- **マルチ・エージェント協調システムの開発:**専門のAIエージェントが連携して医療保険プランのドキュメントを分析し、包括的なレポートを生成する、高度なマルチ・エージェントアーキテクチャを設計・実装します。Azure
  AI
  Searchを用いたインテリジェントなドキュメント検索のための検索エージェント、詳細な分析レポートを生成するレポートエージェント、コンプライアンスと正確性を確保するための検証エージェント、エージェント間の通信とワークフロー調整を管理するオーケストレーターエージェントを構築し、エンタープライズグレードのエージェント連携パターンを紹介します。

**前提条件**

参加者には以下の条件が必要です:

- **Azureとクラウドの経験**- Azure Portal、リソースグループ、Azure AI
  サービスに関する知識

- **プログラミングスキル**- 基本的な Python の知識
  (async/await、環境変数、API 呼び出し)

- **AIコンセプト**- LLM、埋め込み、RAG（Retrieval-Augmented
  Generation）、プロンプトエンジニアリングの理解

- **開発ツール**- Visual Studio Code、ターミナルの使用、Git の熟練度

- **エージェントフレームワークの認識**-
  エージェントのアーキテクチャ、ツール、オーケストレーション
  パターンに関する基礎知識

コンポーネントの説明

- **マイクロソフトファウンドリー**Microsoft
  Foundryは、エンタープライズAIエージェントの開発、展開、管理のためのクラウドプラットフォームです。マネージドエージェントサービスランタイム、一元化されたプロジェクト管理、アプリケーションインサイトの監視機能を提供し、エージェントのライフサイクル全体を通じてエンタープライズグレードの信頼性、セキュリティ、そして可観測性を確保します。

- **Microsoft エージェントフレームワーク SDK**: AutoGenとSemantic
  Kernelに代わる、インテリジェントなモジュール型エージェントを構築するための公式Python
  SDKです。ネイティブなAgent-to-Agent通信、Model Context
  Protocol統合、Microsoft
  Foundryサポートを備えており、標準化されたツールを使用することで、本番環境対応のエンタープライズエージェントシステムを構築できます。

- **Azure AI Search**: Retrieval-Augmented
  Generationを可能にするベクターベースの検索エンジン。ベクター類似度とキーワード検索を組み合わせたハイブリッド検索、関連性を向上させるセマンティックランキング、ドキュメントインデックス機能を提供し、エージェントが企業の知識ソースから根拠のある、事実に基づいた正確な回答を提供できるようにします。

- **Model Context Protocol（MCP）**:
  エージェントが外部の知識やツールに安全にアクセスできるようにする標準化されたインターフェース。MCPは、エンタープライズデータソース、Freshdeskなどの外部API、構造化スキーマを備えたカスタムツールに接続し、信頼性が高く監査可能なインタラクションを確保し、拡張可能なエンタープライズAIシステムの基盤を形成します。

- **チャットレスポンスエージェント**:
  ローカル開発とテストのための、シングルターンのステートレスエージェントモデルです。コンテキストを保持することなくリクエストを独立して処理し、ローカル環境内で実行され、即座に応答します。永続エージェントを導入して本番環境に移行する前に、コアロジックのプロトタイプ作成や動作検証を行うのに最適です。

- **永続的なエージェント**: Microsoft Foundry
  のクラウドホスト型長期サービスで、会話全体にわたって状態を維持します。MCP
  を介した外部ツールとの統合、Agent-to-Agentの連携、組み込み監視機能によるエンタープライズ規模の信頼性をサポートし、ステートフルでマルチターンな会話エクスペリエンスを必要とする本番環境アプリケーションの基盤を提供します。

- **プランナーエージェント**:
  ユーザーからの問い合わせを分析し、適切な専門エージェントにルーティングするインテリジェントなオーケストレーターです。AI推論とキーワードヒューリスティックを用いて、人事、財務、コンプライアンスなどの領域をまたいで問い合わせを分類し、最適なタスク配分を実現し、中央コーディネーションポイントとして機能します。

- **ワーカーエージェント**:
  人事、財務、コンプライアンスといった特定分野に精通したドメインスペシャリスト。各エージェントは、ドメイン固有の指示、専用ツール、関連知識ソースを備えています。プランナーエージェントとA2Aコミュニケーションを通じて連携し、複雑なドメイン固有の問い合わせに対して、信頼性が高く正確な回答を提供します。

- **Azure オープンAI**:
  セキュアなAPIエンドポイントを通じて高度なLLMへのアクセスを提供するエンタープライズグレードのサービスです。チャット補完、埋め込みモデル、コンテンツフィルタリング、コンプライアンス機能を提供します。Microsoft
  Foundryとシームレスに統合されているため、エージェントはデータプライバシーとガバナンス管理を維持しながらGPT-4を活用できます。

# ラボ 5: Microsoft Foundry を使用したRetrieval-Augmented AI エージェントの構築

**概要**

このラボでは、Microsoft Foundry ポータルを使用して最初の AI
エージェントを作成します。まず、エンタープライズ ポリシー
ドキュメントをアップロードし、Azure AI Search
に取り込んでナレッジベースを準備します。次に、Microsoft Agent Framework
を使用してエージェントを構成し、retrieval-augmented generation (RAG)
を有効にします。最後に、エージェントの応答をテストし、実行ログを分析して、エージェントがどのように情報を取得および処理するかを確認します。

**ラボの目的**

このラボでは次のタスクを実行します。

- タスク 1: Azure リソースを作成する

- タスク2: Microsoft FoundryでAIエージェントを作成する

- タスク 3: RAG 用の Azure AI Search を接続する

- タスク4: エージェント実行ログのテストと観察

## タスク 1: Azure リソースを作成する

このタスクでは、このラボを実行するために必要なすべての Azure
リソースを作成します。

### タスク 1.1: ストレージ アカウントを作成する

1.  以下の資格情報を使用して、+++https://portal.azure.com+++ の Azure
    ポータルにログインし、ストレージ アカウントを選択します。

- Username - +++@lab.CloudPortalCredential(User1).Username+++

- TAP - <+++@lab.CloudPortalCredential(User1).TAP>+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image1.png)

2.  **Create**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

3.  以下の詳細を入力し、**Review +
    create**を選択します。次の画面でCreateを選択します。

- Storage account name - +++aistorage@lab.LabInstance.Id+++

- Preferred storage type – **Azure Blob Storage or Azure Data Lake
  Storage Gen2**を選択します。

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image3.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image4.png)

4.  リソースが作成されたら、**Go to resource**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

5.  「**Upload**」を選択し、**Create
    new**を選択して新しいコンテナを作成します。名前を+++**datasets**+++にして、「OK」を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

![A screenshot of a login box AI-generated content may be
incorrect.](./media/image7.png)

6.  **Browse for files**を選択し、C:\Labfiles\Day 2 からポリシー
    ファイルを選択して、\[**Upload**\] をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

![A screenshot of a upload box AI-generated content may be
incorrect.](./media/image9.png)

これで、ストレージ アカウントが正常に作成され、ポリシー
ドキュメントが読み込まれました。

### タスク1.2: Foundryリソースを作成する

このタスクでは、Microsoft Foundry にアクセスするために必要な Foundry
リソースを作成します。

1.  Azure ポータル (+++https://portal.azure.com+++) のホーム
    ページから、**Foundry** を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.png)

2.  左側のペインから **Foundry**を選択し、**Create**を選択して Foundry
    リソースを作成します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

3.  以下の詳細を入力し、Review + createを選択します。

- Name – <+++agentic-@lab.LabInstance.Id>+++

- Default project name – <+++agentic-ai-project-@lab.LabInstance.Id>+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.png)

4.  検証したら、**Create**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

5.  リソースが作成されたことを確認します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

6.  [**agentic-ai-project-@lab.LabInstance.Id**](mailto:agentic-ai-project-@lab.LabInstance.Id)を開き、
    **Go to Foundry portal**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.png)

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image16.png)

7.  Microsoft Foundry の左側のペインで、Models + endpointsを選択します+
    **Deploy model** -\> **Deploy base model**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.png)

8.  +++gpt-4o-mini+++
    を検索して選択し、Confirmをクリックしてモデルをデプロイします。

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image18.png)

9.  **Deploy**ウィンドウで \[デプロイ\] を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.png)

10. 同様に、 +++text-embedding-ada-002+++ を検索してデプロイします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image21.png)

このタスクでは、Foundry
リソースを正常に作成し、そこにチャットと埋め込みモデルをデプロイしました。

### タスク 1.3: アプリケーションインサイトを作成する

このタスクでは、監視に必要な Application Insights リソースを作成します。

1.  Azure ポータルのHomeページで
    **Subscriptions**を選択し、割り当てられたサブスクリプションを選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

2.  左側のペインから**Resource providers**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

3.  +++Operational+++を検索し、Microsoft.OperationalInsights の横にある
    3 つのドットを選択して、\[**Register**\] をクリックします。　

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

4.  Microsoft Foundry の左側のペインから、\[**Monitoring**\]
    を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

5.  **Create New**
    -\>を選択して、名前として<+++agent-insights-@lab.LabInstance.Id>+++を入力します。それから、**Create**を選択します。

![A screenshot of a application AI-generated content may be
incorrect.](./media/image27.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.png)

このタスクでは、Application Insight リソースを作成しました。

### タスク 1.4: 検索リソースを作成する

AIエージェントが企業の質問に正確に回答するには、信頼できるデータソースにアクセスする必要があります。Azure
AI
Searchは、ポリシー、契約書、マニュアルなどのドキュメントにインデックスを付けることで、Retrieval-Augmented
Generation
（RAG）を実現します。インデックスは検索可能なカタログのように機能し、コンテンツをチャンクに分割し、メタデータを追加することで、エージェントが会話中に適切な情報を取得できるようにします。

このタスクでは、Azure AI Search
を使用してアップロードされたドキュメントにインデックスを付け、検索可能なナレッジ
ベースを作成します。

1.  Azure ポータルのホーム ページで、**Foundry** を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.png)

2.  左側のペインから **AI Search**を選択し、**+ Create**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

3.  以下の詳細を入力し、**Review + create**を選択します。

- Service name - +++ai-knowledge-@lab.LabInstance.Id+++

- Region - East US2

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image32.png)

4.  検証に合格したら、**Create**を選択します。リソースが作成されたらGo
    to resourceを選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image34.png)

5.  **Import data (new)**を選択します。

![A screenshot of a search engine AI-generated content may be
incorrect.](./media/image35.png)

6.  **Choose data source**で **Azure Blob Storage** を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.png)

7.  次のペインでは、取得ベースのエージェントを構築するため、RAG
    オプションを選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image37.png)

> それぞれのオプションの意味は次のとおりです。

1.  **キーワード検索:**正確なキーワードに基づく従来の検索エクスペリエンスに使用されます。テキストをインデックス化することで、ユーザーはAIによる推論を必要とせず、キーワードマッチングによって情報を検索できます。

2.  **RAG (Retrieval-Augmented Generation ):**ドキュメント検索と AI
    生成を組み合わせます。テキスト (および単純な OCR 画像)
    を取り込むことで、AI
    エージェントが根拠のあるコンテキスト認識型の回答を提供できるようになります。

3.  **マルチモーダルRAG:**RAGを拡張し、図、表、ワークフロー、グラフなどの複雑なビジュアルコンテンツを処理できるようにします。AIがテキストとビジュアル要素の両方を解釈し、より豊かで洞察に基づいた応答を提供できるようになります。

&nbsp;

8.  **Storage
    account**の下にある「aistorage@lab.LabInstance.Id」を選択し、**Blob
    containe**rの下にある「**datasets**」を選択して、**Next**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

9.  以下の詳細を選択し、**Next**を選択します。

- Kind – Azure AI Foundry (Preview)

- Azure AI Foundry/Hub project –
  <agentic-ai-project-@lab.LabInstance.Id>

- Model deployment – text-embedding-002-ada

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.png)

10. 次の画面で「**Next**」を選択し、**Review and
    create**画面が表示されるまで続けます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image40.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.png)

11. **Review and create**画面で **Create**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.png)

12. Create succeededダイアログで**Close**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.png)

データセットをAzure AI
Searchに取り込み、検索可能なインデックスを作成しました。次のタスクでは、AIエージェントを作成し、このインデックスをナレッジソースとして接続します。

## タスク2: Microsoft FoundryでAIエージェントを作成する

このタスクでは、Microsoft Foundry で新しい AI
エージェントを作成し、Microsoft Agent Framework
インターフェイスを使用してその主な目的、指示、およびモデルを構成します。

1.  リソース グループに戻り、リソース リストから **agentic**-foundry
    リソースを選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.png)

2.  次のペインで、**Go to Foundry portal**をクリックします。Microsoft
    Foundryポータルに移動し、最初のエージェントを作成します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.png)

3.  Foundry Portal にアクセスしたら、左側のメニューから**Agents
    (1)** を選択すると、**既に作成された**エージェントが表示されます。まだ作成されていない場合は、**+
    New agent (2)** オプションをクリックして作成してください。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image46.png)

4.  新しく作成した**エージェント**を選択すると、右側に設定ペインが開きます。以下の情報を入力してください。

[TABLE]

> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image47.png)

5.  Microsoft Foundry
    でエージェントの作成に成功しました。次は、次のタスクでインデックス付けしたデータを接続して、エージェントに知識を付加します。

## タスク 3: RAG 用の Azure AI Search を接続する

このタスクでは、ナレッジ統合パネルを使用して Azure AI Search
をエージェントと統合し、MCP (Model Context
Protocol)を通じて検索拡張応答を有効にします。

1.  同じエージェント構成ペインで下にスクロールし、**Knowledge** パラメータの
    **+ Add** をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image48.png)

2.  AI 検索リソースにインデックスが用意されているので、**Add
    knowledge** ウィンドウで **\[Azure AI Search\]** を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image49.png)

3.  次のペインの **Azure AI Search resource
    connection** オプションで、**ドロップダウン矢印 (1)**
    をクリックし、**Connect other Azure AI Search resource
    (2)**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.png)

4.  次のペインで、正しい AI
    検索リソースが選択されていることを確認し**Add
    connection**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image51.png)

5.  **Adding Azure AI Search** の手順で、次の詳細を構成し、完了したら
    **Connect (5)** をクリックします。

[TABLE]

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image52.png)

6.  エージェントは、会話中に正確な情報を取得するための検索可能なナレッジ
    ベースとして機能する Azure AI Search
    インデックスを使用して、知識を正常に強化できるようになりました。

## タスク4: エージェント実行ログのテストと観察

このタスクでは、ポリシー関連の質問をして構造化ログを確認し、ツールの使用状況、検索呼び出し、根拠のある応答を検証することで、エージェントをテストします。

1.  エージェントをテストする前に、Application Insights
    を接続して、詳細なログとトレースの可視性を有効にします。

2.  Microsoft Foundryポータルで、左側のメニューから**Monitoring
    (1)** を選択し、**agent-insights- (2)**を選択して**Connect
    (3)**をクリックします。

![](./media/image53.png)

3.  完了したら、左側のメニューから **Agents
    (1)** を選択し、EnterpriseAssistant (2) エージェントを選択して
    、**Try in playground (3)**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image54.png)

4.  チャットパネルが開き、プロンプトを入力できます。エージェントは、接続したドキュメントとデータセットを使用して応答します。

サンプルプロンプト -

- +++What is the employee travel reimbursement policy?+++

- +++Summarize the contract approval rules and cite the document.+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image55.png)

5.  エージェントが質問に回答したら、上部のメニューから**Thread
    logs** をクリックして、現在のスレッドのログとトレースをチェックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image56.png)

6.  エージェント
    ログの詳細な概要を示すこれらのメトリック、トレース、および評価を調べて確認します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image57.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image58.png)

7.  次に、以前にアプリケーション
    インサイトを接続した**monitoring** ペインに移動し、**Resource
    usage** タブを選択して、すべてのメトリックと値を確認します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image59.png)

8.  キュレーションされたエンタープライズデータセットを活用したRAGベースのエージェントを構築できました。次は、マルチ・エージェントコラボレーションを実現し、エージェントが委任、推論、そしてインテリジェントな連携を行えるようにします。

まとめ

このラボでは、Microsoft Foundry で最初の AI
エージェントを作成し、インデックス化されたナレッジベースに接続しました。ドキュメントをアップロードして
Azure AI Search に取り込み、Microsoft Agent Framework との統合を通じて
RAG
を有効化しました。エージェントをテストし、実行ログを確認することで、エージェントが根拠となる情報を取得し、エンタープライズ対応の応答を生成する仕組みを実際に体験しました。
