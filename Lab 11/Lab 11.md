# ラボ 11: AgentOps – 可観測性と管理

**推定所要時間**：60分

**概要**

AgentOpsに焦点を当てます。Microsoft Agent
Frameworkに組み込まれた**OpenTelemetry**との統合機能を利用して、Application
Insightsでオブザーバビリティとテレメトリを実現する方法を学びます。

Microsoft Agent Framework のOpenTelemetryについて

Microsoft Agent Framework
は、分散トレース、メトリクス、ログ記録のオープンスタンダードであるOpenTelemetryとネイティブに統合されています。スパントレース、ツール呼び出し、モデル応答、ワークフローパフォーマンスなどのテレメトリデータを自動的にキャプチャすることで、エージェントの動作をエンド・ツー・エンドで可視化します。この統合により、開発者は
Azure Monitor、Application
Insights、またはその他のOpenTelemetry互換バックエンドに観測データを直接エクスポートできます。この標準化されたアプローチにより、複雑なマルチエージェントシステム全体のあらゆるエージェントアクションを追跡し、最小限の構成でパフォーマンスチューニング、トラブルシューティング、コンプライアンス監査が可能になります。

ラボの目的

このラボでは次のタスクを実行します。

- OpenTelemetryを使用してエージェントの可観測性を有効にする

- タスク2: エージェントメトリクスの視覚化

- タスク3: Foundryポータルでエージェント固有のメトリクスを監視する

## OpenTelemetryを使用してエージェントの可観測性を有効にする

OpenTelemetryとAgent
Frameworkの可観測性をプロジェクトに統合します。テレメトリエクスポーターを構成し、
setup_observability
()を使用してトレースを初期化し、エージェントルーティング、Azure AI
Searchの取得、チケット作成など、ワークフローの各ステージの詳細なスパンをキャプチャします。これにより、Application
InsightsのトレースIDを使用して、エージェントの動作とシステム間の相関関係を統合的に可視化できます。

1.  以前のコードを再度変更するのではなく、更新された監視対応ファイルが既に含まれた新しいフォルダーで作業します。Microsoft
    Agent Framework Observability と Application Insights
    を使用して、テレメトリ、トレース、監視がどのように統合されるかを理解します。

2.  Visual Studio Code で、新しいフォルダーを開く前に、.env
    ファイルを選択し、その内容をコピーしてメモ帳に安全に保管します。

3.  上部のメニューから**ファイルオプション**をクリックし、 **Open
    Folder**を選択します。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image1.png)

4.  開いているフォルダー ペインで、C:\telemetry-codefiles
    に移動し、フォルダーの選択をクリックします。

5.  開くと、エクスプローラー メニュー内のファイルは次のようになります。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

6.  コード
    ファイルを確認して、すべてのエージェントでopentelemetryがどのように実装されているか、およびトレースがどのように行われているかを確認してください。

> **統合の概要**
>
> agent_framework.observabilityパッケージを使用して、エージェント
> ワークフロー全体にOpenTelemetryトレースを統合しました。

- get_tracer ()をインポートし、
  OpenTelemetryスパンを使用して、重要な操作ごとに構造化されたテレメトリをキャプチャしました。

- コンテキスト属性を持つスパンに主要な機能
  (分類、ルーティング、RAG、チケット作成など) をラップします。

- エクスポーターとメトリック パイプラインを構成するために、
  setup_observability
  ()を使用して統合されたスタートアップ監視設定を追加しました。

- より詳細な可視性を実現するために、クエリ
  テキスト、ルーティングの決定、フォールバック
  メソッドなどのカスタム属性を記録します。

- 例外トレースを記録し、各ワークフロー実行をトレース ID
  にリンクしてシステム間の相関関係を確立するためのエラー処理が強化されました。

> **ファイルの強化**
>
> main.py – エンド・ツー・エンドのトレースとメトリクス

- OpenTelemetryトレース
  パイプラインとエクスポーターのセットアップを構成しました。

- 完全なワークフローの可視性を実現するために、Span
  内にマルチエージェント オーケストレーションをラップします。

- ルーティング、データ取得
  (RAG)、エージェント応答、チケット作成というサブステップの範囲を追加しました。

> planner_agent.py – ルーティングの観測性強化

- トレーサー インスタンス ( get_tracer ()) を追加しました。

- 生の LLM 応答、信頼スコア、およびフォールバック キーワード
  メトリックをスパン属性としてキャプチャしました。

- ラベル付きスパン ( SpanKind.INTERNAL )を使用した AI
  ベースの分類とヒューリスティック分類を区別します。

> azure_search_tool.py – RAG オブザーバビリティ

- 待機時間と成功率を測定するために、Azure Search API
  呼び出しの範囲を追加しました。

- 取得したドキュメント数とペイロード サイズをカスタム
  メトリックとして記録します。

- OpenTelemetryトレース内でキャプチャされた検索エラーとパフォーマンス
  データ。

> freshdesk_tool.py – チケット作成の可観測性

- チケット作成期間と応答ステータスを追跡するための API
  呼び出し範囲を追加しました。

- 追跡可能な監査ログに記録されたチケット
  ID、タグ、およびリクエスタの詳細。

- インシデント追跡を改善するために、外部 API
  のレイテンシとエラー応答を監視しました。

7.  **.env.example (1)**ファイルを右クリックし**、 Rename
    (2)**を選択してファイルの名前を変更します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

8.  完了したら、ファイルの名前を **.env.example --\>** **.env**
    に変更して**、**この**環境ファイル**をこのエージェントに対してアクティブにします。

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image4.png)

9.  次に、.env ファイルを選択し、先ほどコピーした内容を貼り付けます。

10. Azure ポータルで、 **agenticai**リソース グループに移動し、リソース
    リストから**ai-knowledge-** Search serviceを選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

11. 左側のメニューのSettingsから**Keys (1)** を選択し、図のようにコピー
    オプションを使用して**Query key（2）**をコピーします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

12. コピーしたら、メモ帳などに安全に貼り付け、左のメニューから「検索管理」の**Indexes** **を選択し**、**Index
    Name（2）**をコピーします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

13. Visual Studio Code ペインで、接続用の AI
    検索キーを追加する必要があるため、 **.envファイルを選択します。**

> \# Azure AI Search (MCP)
>
> AZURE_SEARCH_ENDPOINT=https://ai-knowledge--@lab.LabInstance.Id.search.windows.net/
>
> AZURE_SEARCH_API_KEY=\[Query_Key\]
>
> AZURE_SEARCH_INDEX=\[Index_Name\]

**注:** Query_KeyとIndex_Name
の値を、先ほどコピーしたものに置き換えてください。

14. 以下の内容で .env ファイルの内容を追加します。

> AZURE_OPENAI_ENDPOINT=https://agentic-
> @lab.LabInstance.Id.cognitiveservices.azure.com/
>
> AZURE_OPENAI_API_KEY=\<Replace with Azure OpenAI key\>
>
> AZURE_OPENAI_RESPONSES_DEPLOYMENT_NAME=gpt-4o-mini
>
> AZURE_OPENAI_API_VERSION=2025-03-01-preview

15. 次の Foundry プロジェクト キー変数を .env ファイルに追加します。

> \# Azure AI Project Configuration
>
> AZURE_AI_PROJECT_ENDPOINT=**\<Microsoft Foundry endpoint\>**
>
> AZURE_AI_MODEL_DEPLOYMENT_NAME=gpt-4o-mini
>
> Overviewページから Microsoft Foundry
> プロジェクトのエンドポイントを見つけて、 **\<Microsoft Foundry
> endpoint\>を**その値で置き換えます。
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image8.png)

![](./media/image9.png)

16. 完了したら、次の App Insights 変数を同じファイルに追加します。

> \# Observability and Monitoring Configuration
>
> APPLICATIONINSIGHTS_CONNECTION_STRING=**\<Connection string\>**
>
> ENABLE_OTEL=true
>
> ENABLE_SENSITIVE_DATA=true
>
> Azure ポータルから Application Insights
> リソースを開き、接続文字列をコピーして、 **\<Connection
> string\>を**コピーした値に置き換えます。
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image10.png)

17. .env ファイルに次の内容を追加し、先ほどコピーした Freshdesk の API
    キーとアカウント URL を追加します。

> \# Freshdesk Configuration
>
> FRESHDESK_DOMAIN=\[Domain_URL\]
>
> FRESHDESK_API_KEY=\[API_Key\]

18. 最終的な .env ファイルは、指定された画像のようになるはずです。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

19. 完了したら、**File** **(1)** **を選択します。** **Save** **(2)** をクリックして、ファイルを保存します。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image12.png)

20. **...（1）**オプションを選択してメニューを拡張します。
    **Terminal（2）**を選択し、 **New Terminal（3）**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

21. **VS Codeターミナル**で、Azure CLI サインイン コマンドを実行します。

+++ az login +++

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image14.png)

22. **Sign in** ウィンドウで**Work or school account** を選択し、
    **Continue**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.png)

23. **\[Sign into
    Microsoft** **\]**タブで、以下の資格情報を使用してログインします。

- Username - <+++@lab.CloudPortalCredential(User1).Username>+++

- TAP - +++@lab.CloudPortalCredential(User1).TAP+++

24. サインイン オプションのプロンプトが表示されたら、 **\[No, this app
    only** **\]**を選択して、他のデスクトップ
    アプリをリンクせずに続行します。

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image16.png)

25. **Select a subscription and tenant**で「 **1」**と入力して Enter
    キーを押します。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image17.png)

26. ターミナルが開いたら、次のコマンドを実行します。

> +++pip install -r requirements.txt+++
> を実行して、必要なパッケージをすべてインストールします。

27. 検索ツールの動作をテストするには、以下のコマンドを実行します。

+++python main.py+++

> ![A screenshot of a computer screen AI-generated content may be
> incorrect.](./media/image18.png)

## タスク2: エージェントメトリクスの視覚化

このタスクでは、Azure Application Insights
を使用してエージェントのテレメトリデータを視覚化します。応答時間、ルーティング精度、チケット作成成功率などのカスタムメトリックを探索します。さらに、主要業績評価指標（KPI）と傾向を表示するインタラクティブな
Azure Monitor
ダッシュボードを構築します。これにより、ボトルネックの特定、効率の測定、そしてデプロイされたエージェントの正常な動作をリアルタイムで確認できるようになります。

1.  Azure Portal に移動し、リソース グループを開いて、リソース
    リストから**agent-insights-** app insight resource を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.png)

2.  概要ページに移動すると、いくつかのデフォルトのメトリックが表示されます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

3.  左側のメニューから**Search（1）**を選択し、 **See all data in last
    24 hours (2)**をクリックします。

![A screenshot of a search engine AI-generated content may be
incorrect.](./media/image21.png)

4.  開いたら、下から**Traces（1）**を確認し、**View as individual items
    （2）**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

5.  完了すると、エージェントとのコミュニケーションの詳細と、指定した期間内に行われたすべてのトランザクションを確認できます。期間を調整して、さらに詳しく調べることもできます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

6.  トランザクションを詳しく確認するには、クリックするだけで詳細ビューを開くことができます。エージェント、メッセージ、取得の詳細など、すべての詳細を確認する方法をご確認ください。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

7.  次に、**Failures (1)** 、**failed requests**
    **(2)**を選択して、失敗したすべての実行を一元的に表示し、詳細なトレース分析を通じて根本的な原因を特定します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

8.  次に、 **Performance （1）**を選択し、**operations and response
    times（2）**を確認します。これに基づいて、エージェントのパフォーマンスSLAを判断できます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

9.  次に、左側のメニューのMonitoringから**Metrics**を選択します。SPAN
    を通じて公開されているカスタムメトリックを確認できます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.png)

10. 選択したら、**Metric
    Namespace（1）で**azure.applicationinsights**（２**
    ）を選択します。 

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.png)

11. 次に、メトリクスで**gen_ai.client.operation.duration**を選択し**、aggregationをavg
    （1）**に設定します。**ライングラフ（2）**で、エージェントがユーザーに返信するのにかかった**Response
    Time**メトリックを確認します。

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image30.png)

12. 同様に、
    **gen_ai.client.token.usage**を選択し、**aggregation**を**avg
    （1）に設定します**。**ライングラフ（2）**で、エージェントからのトークン使用量を確認します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

13. 次に、左側のメニューから**Logs (1)**を選択し、**Queries hub
    (2)**ペインをキャンセルします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image32.png)

14. 閉じたら、**Tables**オプションをクリックし、
    **customMetrics**パラメータにマウスを移動すると**Run** オプションが表示されるので、それをクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

![A close-up of a message AI-generated content may be
incorrect.](./media/image34.png)

15. クエリが正常に実行されると、以下にリストされているすべてのカスタム
    メトリックがクエリ結果として表示されます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image35.png)

16. 次に、左側のメニューから**Workbooks(1)**を選択し、**Quick start
    の下のEmpy(2)**ワークブックをクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.png)

17. 開いたら、 **+ Add（1）**をクリックし、 **Add
    metric（2）**を選択します。

![A screenshot of a phone AI-generated content may be
incorrect.](./media/image37.png)

18. メトリック ペインが開いたら、 **Add
    metric** オプションをクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

19. ここで、**Metric** としてgen_ai.client.token.usage**(1)**を選択します。 Display
    nameにトークン使用法**(2)**を入力し、 **Save (3)**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.png)

20. もう一度、 **Add metric **オプションをクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

21. ここで、**Metric**としてgen_ai.client.operation.duration。 **(1)**
    を選択します。**Display nameを**Response Time  **(2)**として入力し、
    **Save (3)**をクリックします。

![A screenshot of a screenshot of a metric settings AI-generated content
may be incorrect.](./media/image40.png)

22. 両方のメトリックを選択したら、 **Run Metrics**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.png)

23. **Visualization** **をArea
    Chart** に変更すると、同様のグラフが表示されます。他にも様々な視覚化オプションや時間範囲のオプションを試すことができます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.png)

24. 編集が完了したら、 Done
    editingをクリックします。これでこのカードがワークブックに保存されます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.png)

25. 次に、もう一度**+ Add (1)**をクリックし、 **Add
    query（2）**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.png)

26. クエリペインで次の**query（1）**を追加し、 **Run
    Query（2）**をクリックします。

+++customMetrics+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.png)

27. クエリが正常に実行されたら、結果を確認してください。確認後、 **Done
    Editing**をクリックしてください。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image46.png)

28. **Done editing（1）**をクリックし、
    **Save（2）**アイコンをクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image48.png)

29. Save As paneペインで、Titleにagent-workbook **(1)**と入力し、 **Save
    As (2)**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image49.png)

30. これはラボ環境であるため、包括的な監視を行うには利用可能なデータが限られている可能性があります。ただし、エージェントからのカスタムメトリックを追加し、次のような特定の目的に焦点を当てた専用の監視ダッシュボードを作成することで、可視性を向上させることができます。

- **エージェントパフォーマンスダッシュボード**

> **表示されるメトリック:**

- エージェントの応答時間（平均、P95）

- エージェントタイプ別の成功率

- リクエスト量の傾向

- エラー率アラート

> **ビジネスに関する質問への回答:**

- どのエージェントが最もパフォーマンスが優れているでしょうか。

- SLA 目標を達成していますか。

- システムの速度低下の原因は何ですか。

&nbsp;

- **ユーザーエクスペリエンスダッシュボード**

> **表示されるメトリック:**

- エンド・ツーエ・ンドのリクエストレイテンシ

- チケット作成率

- 知識検索の成功

- ユーザー満足度代理指標

> **ビジネスに関する質問への回答:**

- ユーザーは素早い応答を得ていますか。

- リクエストがサポート チケットになる頻度はどのくらいですか。

- ナレッジベースはユーザーに役立っていますか。

## タスク3: Foundryポータルでエージェント固有のメトリクスを監視する

このタスクでは、Azure Application Insights
を使用してエージェントのテレメトリデータを視覚化します。Microsoft
Foundry ポータルからエージェント固有のカスタムメトリックを調べます。

1.  Application Insights は既に Microsoft Foundry
    ポータルに接続しているので、Foundry
    ポータルに戻ってエージェントの動作を視覚化できます。

2.  リソース グループに戻り、リソース リストから**agent** -foundry
    リソースを選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.png)

3.  次のペインで、 **Go to Foundry portal**をクリックします。Microsoft
    Foundryポータルに移動し、最初のエージェントを作成します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image51.png)

4.  エージェントをテストする前に、Application Insights
    を接続して、詳細なログとトレースの可視性を有効にします。

5.  Microsoft Foundryポータルで、左側のメニューから**Monitoring
    (1)**を選択し、**agent-insights-  (2)**を選択して**Connect
    (3)**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image52.png)

6.  次に、以前にアプリケーション
    インサイトを接続した**Monitoring**ペインに移動し、 **Resource
    usage** タブを選択して、すべてのメトリックと値を確認します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image53.png)

7.  左側のメニューから**Tracing （1）**を選択し、
    **Trace（2）**のいずれかをクリックして、エージェントのインタラクションの詳細なトレースを確認します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image54.png)

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image55.png)

**まとめ**

このラボでは、エンタープライズエージェントの可観測性と監視を構成しました。OpenTelemetry
トレースを使用して、ワークフローの各ステップの詳細な実行データをキャプチャし、Azure
Application Insights
と統合することで、パフォーマンスメトリックとエージェントの正常性を視覚化するダッシュボードを作成しました。

このラボは正常に完了しました。Next
\>\>をクリックして次に進んでください。
