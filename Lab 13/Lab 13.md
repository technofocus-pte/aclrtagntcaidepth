# ラボ 13: Human-in-the-Loop AI による企業内不正検出の実装

**推定所要時間**： 60分

**概要**

Contoso
Ltd.のAIエンジニアで、human-in-the-loop（HITL）AIワークフローの実装を担当しています。このラボでは、Contoso
Fraud Detection &
Responseワークフローについて学習します。このワークフローでは、AIエージェントが疑わしいアクティビティを分析し、高リスクのアクションを人間のアナリストにルーティングしてレビューを行います。リアルタイムのReact +
FastAPIダッシュボードを使用して、監視とインタラクションを行います。

ラボの目的

このラボでは次のタスクを実行します。

- タスク 1: Azure エージェント フレームワークを使用したHuman-in-the-Loop
  AI ワークフローの実装

## タスク0: コードを設定する

1.  C:\Labfiles\Day 3 から、OpenAIWorkshop-Framework
    ファイルを抽出します。

2.  LabVM デスクトップから Visual Studio Code をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

3.  File (1)を選択し、**Open
    Folder** **(2)** をクリックして**OpenAIWorkshop-Framework**フォルダーを開きます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

4.  C:\Labfiles\Day 3\\**OpenAIWorkshop-Framework**
    パスに移動し、**OpenAIWorkshop-Framework**
    を選択してから、**フォルダーを選択します**。

5.  「**Yes, I trust the authors**」を選択します。

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image3.png)

6.  **省略記号(...)
    (1)**をクリックし、次に**Terminal** **(2)** をクリックして、**New
    Terminal** **(3)**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

7.  以下のコマンドを入力して**applications** ディレクトリに移動し、**pyproject.toml
    / uv.lock** ファイルから必要な依存関係をすべてインストールします。

> cd agentic_ai/applications
>
> uv sync

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

**注記：**エラーが発生した場合は、以下のコマンドを実行してください。

> +++pip install uv+++

+++uv sync+++

8.  このコマンドは完了までに5～10分かかる場合があります。**その間、タスク1を続行してください。**

## タスク 1: Agent Frameworkを使用したHuman-in-the-Loop AI ワークフローの実装

このラボでは、Contoso 社のFraud Detectionシステムに Human-in-the-Loop
(HITL)
ワークフローを実装します。マルチエージェントによる不正検出の実行、高リスクアラートの確認、人間による意思決定、そして
React + FastAPI
ダッシュボードを用いたワークフローのリアルタイム可視化を行います。

1.  Visual Studio Codeから、**agentic_ai (1) \> workflow (2)\>
    fraud_detection (3)**を展開し、**fraud_detection_workflow.py
    (4)**を選択します。Code **(5)**を表示します 。

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image6.png)

2.  **fraud_detection（1）**の下で、**.env.sample（2）**を右クリックし、**Rename** **(3)**を選択します。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image7.png)

3.  名前を .env に変更し、クリックしてファイルを開きます。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image8.png)

4.  AZURE_OPENAI_API_KEY (1) と AZURE_OPENAI_ENDPOINT (2)
    の値を、前のラボでコピーした実際の値に置き換えます。

5.  AZURE_OPENAI_CHAT_DEPLOYMENTを**gpt-4o-mini (3)**として追加します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.png)

- **Microsoft
  Foundry**ポータルに移動し、**Overview** **(1)**,を選択し、「**Azure
  OpenAI** **(2)**を選択します。**Azure OpenAI key** **(3)** と**Azure
  OpenAI endpoint** **(4)**をコピーします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.png)

6.  File (1)を選択し、**Save** **(2)**を選択します。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

7.  Visual Studio Code ウィンドウで、**省略記号 (...) (1)**
    をクリックし、次に**Terminal** **(2)** をクリックして、**New
    Terminal** **(3)**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

8.  以下のコマンドを実行します。

> cd mcp
>
> uv run python mcp_service.py

9.  コマンドを実行し、新しいターミナルを開きます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

10. コマンドラインでワークフローを実行するには、以下のコマンドを入力します。

> cd agentic_ai/workflow/fraud_detection
>
> uv run python fraud_detection_workflow.py
>
> ![A black screen with white text AI-generated content may be
> incorrect.](./media/image12.png)

**注記**:
コマンドの完了には5～10分かかる場合があります。完了するまでお待ちください。

11. この例には 3 つのサンプル アラートが含まれています。

    - **警告1: Multi-Country Login** （重大度が高い）

    - アラートID: "ALERT-001"

    - 顧客ID: 1

    - アラートタイプ: 「multi_country_login」

    - 説明: 「2 Login attempts from USA and Russia within 2 hours.」

重大度：「high」

- **アラート2: Data Spike** （中程度の重大度）

- アラートID: "ALERT-002"

- 顧客ID: 2

- アラートタイプ: "data_spike"

- 説明: 「Data usage increased by 500% in the last 24 hours.」

重大度:「medium」

- **警告3: Unusual Charges**（重大度が高い）

- アラートID: "ALERT-003"

- 顧客ID: 3

- アラートタイプ: 「unusual_charges」

- 説明: 「Three large purchases totaling $5,000 in 10 minutes」

重大度：「high」

12. 実行が成功すると、以下のターミナルが表示されます。リスクの重大度に応じてアクションを選択してください。リスクの重大度が0.6以上の場合は、人間によるレビューが必要です。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

13. リスクの重大度が高いため、2を入力して顧客アカウントをロックすることができます（1）

    - アナリストのコメント：3つの分析すべてで高リスクが確認されました。即時対応：不正アクセス防止のためアカウントをロックします。**(2)**

    - アナリストIDを入力してください（デフォルト：analyst_cli）：**Enter**キーを押します**（3）**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

14. ワークフローが完了すると、次のような出力が表示されます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.png)

15. コマンドが成功したら、**実行中の既存のターミナル
    セッションをすべて削除します。**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image16.png)

## Contoso 不正検出および対応ワークフロー向けReal-Time Workflow Visualizer UI

Real-Time Workflow Visualizer UIを使用して、Contoso Fraud Detection &
Responseワークフローを監視および操作します。すべてのサービス（MCPサーバー、バックエンド、フロントエンド）を起動し、サンプルアラートを選択し、ライブワークフローの実行を監視し、高リスクの不正行為アラートを確認し、アナリストの決定を送信し、イベントストリームをリアルタイムで監視します。

1.  新しいターミナルを開きます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

2.  すべてのサービスを開始（3つのターミナル）:

    - ターミナル1 -​​ MCPサーバー:

> cd mcp
>
> uv run mcp_service.py

- ターミナル2 - FastAPIバックエンド:

> cd agentic_ai/workflow/fraud_detection
>
> uv run --prerelease allow backend.py
>
> ![A screen shot of a computer program AI-generated content may be
> incorrect.](./media/image17.png)

- ターミナル3 - Reactフロントエンド:

> cd agentic_ai/workflow/fraud_detection/ui
>
> npm run dev
>
> **注記**: エラーが発生した場合は、+++npm install+++
> コマンドを実行してから、+++npm run dev+++
> コマンドを再実行してください。
>
> ![A computer screen with white text AI-generated content may be
> incorrect.](./media/image18.png)

- http://localhost:3000 を**Ctrl +
  クリックして、**ブラウザでアプリケーションを開きます。

> ![A screen shot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

3.  Real-Time Workflow Visualizer UIを表示します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

4.  **Select Alerts** ドロップダウンからサンプルアラートを表示できます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image21.png)

**注記**:
2つ目のターミナル（backend.py）で接続が確立された後、ドロップダウンからアラートが表示されるようになります。接続が確立されていることを確認してください。

5.  **アラートの選択**:
    3つのサンプルアラート（ALERT-001、ALERT-002、ALERT-003）から選択してください（1）

    - **Start Workflow (2)** をクリックして処理を開始します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

6.  **ライブアップデートを見る**:
    実行プログラムが実行されるたびにノードの色が変わります

    - 🔵 青 = ランニング

    - 🟢 緑 = 完了

    - ⚪ 灰色 = アイドル状態

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

7.  **アナリストレビュー**: 高リスクの不正行為が検出されると、レビュー
    パネルが表示されます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

8.  **決定を送信**: アクションを選択してメモを追加する

    - Your Decision: 重大度が高い場合は、**Lock Account
      (1)**を選択します。

    - Analyst notes：High risk confirmed from all three analyses.
      Immediate action: locking account to prevent unauthorized
      access. **(2)**を入力します

    - **SUBMIT WORKFLOW (3)**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

9.  **イベントの監視**: 右側のパネルには、完全なイベント
    ストリームが表示されます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

**まとめ**

このラボでは、Azure Agent Framework
を用いて不正行為検出のためのHuman-in-the-Loop (HITL)
ワークフローを実装しました。AI
エージェントが疑わしいアクティビティを分析し、高リスクのケースを人間のアナリストにルーティングし、リアルタイムの
React + FastAPI
ダッシュボードと連携してワークフローの実行状況を監視し、意思決定を送信する仕組みを学習しました。
