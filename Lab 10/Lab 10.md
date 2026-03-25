# 大規模な AI エージェントの管理、セキュリティ保護、監視

**概要**

このハンズオン ラボでは、Azure AI Agent Service SDK と Microsoft Foundry
を使用して、大規模な AI
エージェントの管理、セキュリティ保護、監視に焦点を当てます。参加者は、OpenTelemetry
統合と Azure Application Insights を通じて AI
エージェントを監視および管理する分野である AgentOps
から始めて、エンタープライズ AI
の展開に不可欠な実稼働レベルのプラクティスを深く掘り下げます。このワークショップでは、Microsoft
の 6 つの基本原則 (公平性、信頼性、プライバシー、説明責任など)
を実装し、ヘイトスピーチ、暴力、機密情報などの有害な出力を検出してブロックする構成可能なコンテンツ
セーフティ フィルターを通じて、
の重要性を強調します。さらに、参加者は、専門的な AI
エージェントが疑わしいアクティビティを分析し、重要な意思決定のために高リスクのケースを人間のアナリストにインテリジェントにルーティングする不正検出システムを例に、洗練されたhuman-in-the-loop
(HITL)
ワークフローを構築します。ラボ全体を通して、取得、検証、オーケストレーションの各タスクを連携させるマルチエージェントシステムを操作し、エンドツーエンドのトレース、カスタムメトリクスの可視化、パフォーマンス監視ダッシュボード、リアル・タイムワークフロー管理の実践的な経験を積んでいきます。このワークショップの終了までに、参加者はエンタープライズ環境でAIエージェントを展開、監視、管理するための必須スキルを習得し、組織のポリシーと規制要件へのコンプライアンスを維持しながら、大規模かつ安全、倫理的、かつ効率的に運用できるようになります。

**目的**

このラボを終了すると、次のことができるようになります。

- **可観測性と監視を有効にする**: Azure Application Insights
  と統合された OpenTelemetry を使用して AI
  エージェントのエンド・ツー・エンドのトレースとテレメトリを実装し、エージェントの動作、パフォーマンス
  メトリック、実行トレースをキャプチャします。

- **エージェント メトリックの視覚化**: Application Insights でカスタム
  ダッシュボードとワークブックを作成し、エージェントのパフォーマンス、応答時間、トークンの使用状況、ルーティングの精度、システムの健全性をリアル・タイムで監視します。

- **Responsible AI プラクティスを実装する**: Microsoft Foundry
  でコンテンツ セーフティ フィルターを構成して、有害な出力
  (ヘイトスピーチ、暴力、センシティブなコンテンツ)
  を検出してブロックし、倫理的でコンプライアンスに準拠した AI
  の動作を確保します。

- **Human-in-the-Loopワークフローの構築**:
  AIエージェントがアラートを分析し、リスクの高いケースを人間のアナリストにルーティングしてレビューと意思決定を行う不正検出システムを設計および展開します。

- **マルチ・エージェントシステムの監視**：エージェント間の通信を追跡し、複数の専門エージェントにまたがる分散ワークフローをトレースし、複雑なエージェントオーケストレーションのボトルネックや障害を特定します。

コンポーネントの説明

- **Microsoft Foundry** : エンタープライズ AI
  アプリケーション向けの集中ガバナンス、監視、コンプライアンス機能を備えた
  AI
  モデルの開発、展開、管理を行うクラウドベースのプラットフォームです。

- **Azure AI Hub** : 共有リソースとガバナンス ポリシーを使用してチームが
  AI
  アプリケーションを構築、管理、展開するための、一元化された安全な共同作業環境を提供する最上位レベルの
  Azure リソース。

- **Azure AI Search** :
  関連ドキュメントのインデックス作成と取得によって、Retrieval-Augmented
  Generation (RAG) を可能にするベクター
  ベースの検索サービスで、根拠のある情報に基づいて AI
  生成の応答を改善します。

- **Azure AI サービス**:
  視覚、言語、音声、意思決定機能用の、構築済みでカスタマイズ可能な API
  とモデルを提供するクラウドベースの AI サービスのコレクション。

- **OpenTelemetry** : エージェント実行トレース、パフォーマンス
  メトリック、およびエラー追跡をキャプチャするために Microsoft Agent
  Frameworkにネイティブに統合された分散トレース、メトリック、およびログ記録のオープン
  スタンダード。

- **Content Safetyフィルター**: Microsoft Foundry
  に組み込まれたフィルタリング
  システムで、ヘイトスピーチ、暴力、性的コンテンツ、機密情報 (PII)
  などのカテゴリにわたって有害な出力を自動的に検出し、ブロックします。

- **LLM と埋め込み**: Large Language
  Modelsは、自然言語の理解と生成を提供し、埋め込みは AI
  アプリケーションでのテキストの類似性、検索、知識取得に使用されるベクトル表現です。

# ラボ 10: 前提条件 - ナレッジインデックスとチケットシステムの設定

**推定所要時間**：30分

**概要**

この前提条件ラボでは、企業の知識を取得し、サポートチケットを自動作成できるAI駆動型ワークフローに必要な基礎コンポーネントを設定します。検索可能なナレッジベースを準備し、AIエージェントがMCP（Model
Context
Protocol）ツールを使用してその知識を照会できるようにし、下流のアクションのためのチケットシステムを統合することに重点を置きます。

これらのタスクを完了すると、エージェントが次のことを実行できるコアインフラストラクチャが確立されます。

- インデックスデータから関連情報を取得する

- 会話やワークフロー中にその情報を状況に応じて使用する

- 外部サービスでチケットを作成して問題をエスカレーションする

この設定により、後続のラボがスムーズに実行され、実際の企業シナリオが反映されます。

ラボの目的

このラボでは次のタスクを実行します。

- タスク1: ナレッジインデックスを準備する

- タスク2: チケット管理用のFreshworksの設定

## タスク 1: Azure リソースを作成する

このタスクでは、このラボを実行するために必要なすべての Azure
リソースを作成します。

### タスク 1.1: ストレージ アカウントを作成する

1.  以下の資格情報を使用して、+++https://portal.azure.com+++ の Azure
    ポータルにログインし、Storage accountsを選択します。

- Username - +++@lab.CloudPortalCredential(User1).Username+++

- TAP - <+++@lab.CloudPortalCredential(User1).TAP>+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image1.png)

2.  **Create**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

3.  以下の詳細を入力し、
    **Review+create**を選択します。次の画面でCreateを選択します。

- Storage account name - +++aistorage@lab.LabInstance.Id+++

- Preferred storage type – **Azure Blob Storage または Azure Data Lake
  Storage Gen2**を選択します。

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image3.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image4.png)

4.  リソースが作成されたら、 **Go to resource**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

5.  **Upload**を選択し、 **Create
    new**を選択して新しいコンテナを作成します。「+++ **datasets**
    +++」という名前を付け、 **「OK」**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

![A screenshot of a login box AI-generated content may be
incorrect.](./media/image7.png)

6.  **Browse for files**を選択し、 **C:\Labfiles\Day 2**からポリシー
    ファイルを選択して、**Upload**をクリックします。

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
    ページから、 **Foundry**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.png)

2.  **Foundry**を選択し、 **Create**を選択して、Foundry
    リソースを作成します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

3.  以下の詳細を入力し、**Review + create**を選択します。

- Name – <+++agentic-@lab.LabInstance.Id>+++

- Default project name – <+++agentic-ai-project-@lab.LabInstance.Id>+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.png)

4.  **Create**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

5.  リソースが作成されたことを確認します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

6.  [**agentic-ai-project-@lab.LabInstance.Id**](mailto:agentic-ai-project-@lab.LabInstance.Id)を開きます。
    **Go to Foundry portal**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.png)

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image16.png)

7.  Microsoft Foundry の左側のペインで、Models +
    endpointsを選択します。+ **Deploy model** -\> **Deploy base
    model**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.png)

8.  +++gpt-4o-mini+++
    を検索して選択し、Confirmをクリックしてモデルをデプロイします。

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image18.png)

9.  デプロイ ウィンドウで**Deploy**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.png)

10. 同様に、 +++text-embedding-ada-002+++ を検索してデプロイします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image21.png)

このタスクでは、Foundry
リソースを正常に作成し、そこにチャットと埋め込みモデルをデプロイしました。

### タスク 1.3: Application insightsを作成する

このタスクでは、監視に必要な Application Insights リソースを作成します。

1.  Azure ポータルのホーム
    ページで**Subscriptions**を選択し、割り当てられたサブスクリプションを選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

2.  左側のペインから**Resource providers**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

3.  +++Operational+++を検索し、**Microsoft.OperationalInsights**の横にある
    3 つのドットを選択して、 **Register** をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

4.  Microsoft Foundry の左側のペインから、**Monitoring**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

5.  **Create New**
    -\>を選択し、名前を<+++agent-insights-@lab.LabInstance.Id> +++
    として入力し、 **Create**を選択します。

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
Generation（RAG）を実現します。インデックスは検索可能なカタログのように機能し、コンテンツをチャンクに分割し、メタデータを追加することで、エージェントが会話中に適切な情報を取得できるようにします。

このタスクでは、Azure AI Search
を使用してアップロードされたドキュメントにインデックスを付け、検索可能なナレッジ
ベースを作成します。

1.  Azure ポータルのホーム ページで、 **Foundry**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.png)

2.  左側のペインから**AI Search**を選択し、 **\[+ Create\]**
    を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

3.  以下の詳細を入力し、 **Review + create**を選択します。

- Service name - +++ai-knowledge-@lab.LabInstance.Id+++

- Region - East US2

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image32.png)

4.  **Create**を選択します。リソースが作成されたら、Go to
    resourceを選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image34.png)

5.  **Import data (new)**を選択します。

![A screenshot of a search engine AI-generated content may be
incorrect.](./media/image35.png)

6.  **Choose data source**で**Azure Blob Storage** を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.png)

7.  次のペインでは、取得ベースのエージェントを構築するため、
    **RAG**オプションを選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image37.png)

> それぞれのオプションの意味は次のとおりです。

1.  **キーワード検索：**正確なキーワードに基づく従来の検索エクスペリエンスに使用されます。テキストをインデックス化することで、ユーザーはAIによる推論を必要とせず、キーワードマッチングによって情報を検索できます。

2.  **RAG (Retrieval-Augmented Generation):**ドキュメント検索と AI
    生成を組み合わせます。テキスト (および単純な OCR 画像)
    を取り込むことで、AI
    エージェントが根拠のあるコンテキスト認識型の回答を提供できるようになります。

3.  **マルチモーダルRAG：**
    RAGを拡張し、図、表、ワークフロー、グラフなどの複雑な視覚コンテンツを処理できるようにします。AIがテキストと視覚要素の両方を解釈し、より豊かで洞察に基づいた応答を提供できるようになります。

&nbsp;

8.  **Storage account**で<aistorage@lab.LabInstance.Id>を選択し、
    **\[Blob Container\] の下で**と**datasetsを選択して、Next**
    を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

9.  以下の詳細を選択し、 **Next**を選択します。

- Kind – Azure AI Foundry (Preview)

- Azure AI Foundry/Hub Project –
  <agentic-ai-project-@lab.LabInstance.Id>

- Model deployment – text-embedding-002-ada

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.png)

10. 次の画面で**Next**を選択し、 **Review and
    create**画面が表示されるまで続けます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image40.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.png)

11. **Review and create**画面で**Create**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.png)

12. 作成成功ダイアログでC**lose**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.png)

データセットをAzure AI
Searchに取り込み、検索可能なインデックスを作成しました。次のタスクでは、AIエージェントを作成し、このインデックスをナレッジソースとして接続します。

# タスク2: チケット管理用のFreshworksの設定

このタスクでは、Freshworks
をセットアップして構成し、マルチ・エージェント
システムのチケット管理とエンタープライズ統合を有効にします。

**Freshworksは**、カスタマーサポート業務の改善とユーザー満足度の向上を目的として設計されたクラウドベースのカスタマーサービスおよびエンゲージメントプラットフォームです。チケット管理、ライブチャット、ヘルプセンター構築、カスタマーセルフサービスのためのツールスイートを提供しています。Freshworksはオムニチャネルコミュニケーションをサポートし、メール、チャット、電話、ソーシャルメディアを通じた顧客とのインタラクションを一元管理できるインターフェースを提供します。自動化機能により、ワークフローの効率化、チケットの割り当て、パフォーマンス追跡のための分析機能を提供します。それでは、Freshworksアカウントの設定を行いましょう。

1.  URL をコピーし、VM 内のブラウザの新しいタブに貼り付けて、
    **Freshworks**ポータルを開きます。

    - URL:

> +++https://www.freshworks.com/freshdesk/lp/home/?tactic_id=3387224&utm_source=google-adwords&utm_medium=FD-Search-Brand-India&utm_campaign=FD-Search-Brand-India&utm_term=freshdesk&device=c&matchtype=e&network=g&gclid=EAIaIQobChMIuOK90qvLjQMV_dQWBR3JAi9VEAAYASAAEgK87_D_BwE&audience=kwd-30002131023&ad_id=282519464145&gad_source=1&gad_campaignid=671502402+++

2.  ポータルで**Start free
    trial** を選択して、無料トライアルを開始します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.png)

3.  次のペインで、以下の詳細を入力して、**Try it free
    (6)**をクリックします。

    - **First name:** LODS

    - **Last name:** User1

    &nbsp;

    - **Work
      email:** **+++@lab.CloudPortalCredential(User1).Username+++**

    &nbsp;

    - **Company name:** Zava

    - **Organization size:** Select **1-10**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image45.png)

4.  次のペインで、以下の詳細を入力し、 **Next（4）**をクリックします。

    - **What industry are you from ?:** リストから**Software and
      internet (1)**を選択してください

    - **How many employees are there in your company?:** **1-10
      (2)**を選択します

    - **I'm trying customer service software for the first time
      (3)**を選択します

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image46.png)

5.  完了したら、指定された URL をコピーし、VM
    内のブラウザの新しいタブに貼り付けて**Outlook** を開きます。

    - URL:

> +++https://go.microsoft.com/fwlink/p/?LinkID=2125442&clcid=0x409&culture=en-us&country=us+++

6.  アカウントの選択ペインで、このラボに割り当てられているアカウントを選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.png)

7.  Freshworks の確認メールを開いて、 **Activate
    Account**をクリックします。

> ![A screenshot of a computer screen AI-generated content may be
> incorrect.](./media/image48.png)

**注：**
Freshworksからのアクティベーションメールが見つからない場合は、メールの配信に遅延が発生している可能性があるため、数分お待ちください。しばらく経ってもメールが届かない場合は、新しいプライベートウィンドウまたはシークレットウィンドウで無料トライアルのアクティベーション手順を再度実行することを検討してください。また、メールがスパムフォルダまたは迷惑メールフォルダに振り分けられている可能性がありますので、ご確認ください。

8.  次のパネルで、 **Enter password（1）とConfirm
    password（2）**に同じパスワードを入力します。 **Activate your
    account（3）**をクリックします。

> ![A screenshot of a login screen AI-generated content may be
> incorrect.](./media/image49.png)

9.  ポータルに入ったら、右上隅にある**Profile（1）**アイコンをクリックし、**Profile
    settings（2）**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.png)

10. プロフィールページで、 **View API Key** をクリックしてAPI
    キーを取得します。

![A screenshot of a web page AI-generated content may be
incorrect.](./media/image51.png)

**注意:**このオプションが見つからない場合は、 **CTRL +
-**を使用して画面サイズを最小化してください。

11. 次のペインで、 **CAPTCHA** を入力します。

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image52.png)

12. API キーをメモ帳にコピーしてください。後で使用します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image53.png)

13. ブラウザタブから、表示されている**Account
    URL** をコピーし、その値をメモ帳にコピーしてください。この情報は後ほど使用します。

![](./media/image54.png)

**まとめ**

この前提条件ラボを完了することで、エンド・ツー・エンドのエージェントワークフローに不可欠な基盤を構築できました。検索可能なナレッジインデックスを準備し、エージェントが**Azure
AI
Search**上に構築されたMCPツールを通じてそのデータを照会できるようにし、チケット管理を自動化するために**Freshworks**を統合しました。

この基盤により、エージェントは正確なコンテキストを取得し、情報に基づいた意思決定を行い、問題を効率的にエスカレートできるようになり、今後のラボでより高度なエージェント主導のシナリオに対応できる環境が整います。

このラボは正常に完了しました。Next\>\>をクリックして次に進んでください。
