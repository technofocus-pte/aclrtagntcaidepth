# ラボ 2: AI プロジェクトをセットアップし、VS Code からチャット補完を実行する

**概要**

このラボでは、Microsoft Foundry で AI プロジェクトを作成・構成し、Large
Language Models (LLM) と埋め込みモデルをデプロイし、プロジェクトを
Visual Studio Code に接続することで、AI
エージェントの構築に必要な開発環境を準備します。その後、コードから簡単なチャット補完を実行してセットアップを検証し、環境が正しく構成され、AI-poweredアプリケーションの開発準備が整っていることを確認します。

ラボの目的

このラボでは次のタスクを実行します。

- タスク 1: Microsoft Foundry での AI プロジェクトのセットアップ

- タスク2: LLMの導入とモデルの埋め込み

- タスク3:
  依存関係をインストールし、仮想環境を作成し、環境変数ファイルを作成する

## タスク 1: Microsoft FoundryでのAIプロジェクトのセットアップ

このタスクでは、Microsoft Foundry 内で AI
プロジェクトを作成および構成します。必要なリソースの設定、プロジェクトパラメータの定義、AI
モデルのデプロイ環境の準備などを行います。このタスクを完了すると、完全に初期化された
AI
プロジェクトが完成し、今後の開発と実験の基盤として活用できるようになります。

1.  Azure Portal ページで、ポータルの上部にある \[リソースの検索\]
    ボックスに**「Microsoft Foundry (1) 」と入力し**、 \[サービス\]
    の**\[Microsoft Foundry (2)\]を選択します**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

2.  **Use with Foundry**の左側のナビゲーションペインで、 **AI Hubs
    (1)**を選択します。AIハブページで、**Create（2）**をクリックし、ドロップダウンから**Hub（3）**を選択します。

![](./media/image2.png)

3.  **Create an Azure AI
    hub **ペインで、**Basics（１）**の下に次の詳細を入力します。 :

    - Subscription : **Leave default subscription** **(2)**

    - Resource Group : **AgenticAI** **(3)**

    - Region : **East US2** (4)

    - Name : ** <+++ai-foundry-hub@lab.LabInstance.Id>+++ (5)**

    - Connect AI Services incl. OpenAI : Click on **Create
      New** **(6)**.

    - Connect AI Services incl. OpenAI : Provide a
      name **<+++my-ai-service@lab.LabInstance.Id>+++ (7)**.

    - **「Save 」(8)**をクリックし 、**Next:Storage** **(9)**をクリックします。

> ![](./media/image3.png)

4.  **Review + Create **タブをクリックし、 **Create**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

![](./media/image5.png)

5.  デプロイメントが完了するまで待ってから、 **Go to
    resource**をクリックします。

![](./media/image6.png)

6.  「概要」ペインで、 **「Launch Azure AI
    Foundry」**をクリックします。これにより、Microsoft
    Foundryポータルに移動します。

![](./media/image7.png)

7.  Hub Overviewで**+ New project**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

8.  プロジェクト名を[**+++**
    **ai-foundry-project@lab.LabInstance.Id**](mailto:+++ai-foundry-project@lab.LabInstance.Id)
    **+++** として入力し、 **Create（2）**をクリックします。

![](./media/image9.png)

9.  プロジェクトが作成されたら、下にスクロールして**Project connection
    string**をコピーし、メモ帳または安全な場所に貼り付けます。今後のタスクで必要になります。

![A screenshot of a project AI-generated content may be
incorrect.](./media/image10.png)

## タスク2: LLMの導入とモデルの埋め込み

このタスクでは、Microsoft Foundry プロジェクト内にLarge Language Models
（LLM）と埋め込みモデルをデプロイします。これらのモデルは、今後のラボで
AI 駆動型アプリケーションやベクターベースの検索機能に使用されます。

1.  **Microsoft Foundry project**で、**My
    assets (1) **セクションに移動し、**Models +
    endpoints (2)**を選択します。**Deploy
    model (3)**をクリックし、**Deploy base
    model (4) **を選択して続行します。　

![](./media/image11.png)

2.  **Select a model ウィンドウ**で、
    **gpt-4o（1）**を検索します。 **gpt-4o（2）**を選択して **Confirm（3） **を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.png)

3.  **Deploy model gpt-4o **ウィンドウで、 **Customize**を選択します。

![](./media/image13.png)

- Deployment Name: gpt-4o (1)

- Deployment type: Global Standard (2)

- **Model versionを2024-08-06 (Default) (3)に**変更します。 

- Tokens per Minute Rate Limitを**200K (4)に変更します。** **（4）**

- **Connect and Deploy (5)**をクリックします。

![](./media/image14.png)

4.  **Model +
    Endpoints (1)**をクリックし、展開された**gpt-4o (2) **を見ることができます。

>  ![](./media/image15.png)

5.  **Azure Portal **に戻り、 **Open AI (1) **を検索し、**Azure Open
    AI（２）**資源を選択 します。

![](./media/image16.png)

6.  **Microsoft Foundry | Azure OpenAI**ページで、 **+
    Create (1) **を選択し、**Azure OpenAI(2) **を選択して、Azure
    OpenAIリソースを作成します。

![](./media/image17.png)

7.  **Create Azure
    OpenAI ページ**で、次の設定を指定して**Next (6)**をクリックします:

[TABLE]

> ![](./media/image18.png)

8.  **「Next 」**をクリックします。

9.  **「Review + submit 」ページ**で、 **「Create」**をクリックします。

![](./media/image19.png)

10. デプロイメントが成功するまで待ってから、 **Go to
    resource**を選択します。

![](./media/image20.png)

11. **my- openai -service**リソース ページで、 **Go to Foundry
    portal**を選択します。

![](./media/image21.png)

12. AI Foundryプロジェクトで、**Shared
    resources **セクションに移動し、**Deployments (1)**を選択します。**Deploy
    model (2)**をクリックし、**Deploy base
    model (3) **を選択して続行します。

![](./media/image22.png)

**注**：後続のラボで使用するAzure AI
Searchのインポートおよびベクター化ウィザードは、AI
Foundryプロジェクト内のテキスト埋め込みモデルをまだサポートしていません。そのため、Azure
OpenAIサービスを作成し、そこにテキスト埋め込みモデルをデプロイする必要があります。このテキスト埋め込みモデルは、後でベクターインデックスを作成する際に使用します。

13. **Select a model **ウィンドウで、
    **text-embedding-3-large (1)**を検索します。**text-embedding-3-large (2)**を選択して、**Confirm **を選択します。 **（3）**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

14. **Deploy model text-embedding-3-large **ウィンドウで、

    - Deployment type: Select Standard (1)

    - Tokens per Minutes Rate Limit: 120K (2)

    - モデルをデプロイするには、 **Deploy (3) **を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

15. **Deployment
    (1)**をクリックすると、デプロイされた**text-embedding-3-large
    (2) **モデルが表示されます。

![](./media/image25.png)

## タスク3: 依存関係をインストールし、仮想環境を作成し、環境変数ファイルを作成する

このタスクでは、必要な依存関係をインストールし、仮想環境をセットアップし、環境変数ファイルを作成します。これにより、制御された開発環境が確保され、AIプロジェクトの構成設定が安全に管理されます。

1.  **Lab VM**で**Visual Studio Code**を起動します。

2.  **File (1)**をクリックして、 **Open Folder**をクリックします。　

![](./media/image26.png)

3.  C:\LabFiles\Day-1\\ **(1)**に移動し、
    **azure-ai-agents-labs（２）**を選択します。それから、**Select
    folder (3)**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

4.  **「Yes, I Trust the authors」**をクリックします。

![](./media/image28.png)

5.  **省略記号(...)**をクリックし、**Terminal** **(2)**、それから**New
    Terminal** **(3)**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.png)

6.  **azure-ai-agents-labsプロジェクトディレクトリ**にいることを確認してください。以下のPowerShellコマンドを実行して、仮想環境を作成し、アクティブ化してください。

7.  python -m venv venv

+++venv/Scripts/activate+++

![](./media/image30.png)

8.  以下のPowerShellコマンドを実行してください。これにより、必要なパッケージがすべてインストールされます。

9.  pip install -r requirements.txt

+++pip install azure-ai-ml azure-identity+++

![A screen shot of a computer code AI-generated content may be
incorrect.](./media/image31.png)

10. 以下のPowerShellコマンドを実行して、pip
    をインストールまたは最新バージョンにアップグレードします。

+++python.exe -m pip install --upgrade pip+++

![](./media/image32.png)

11. 以下のコマンドを実行して、Azure アカウントにログインします。

+++az login+++

12. 承認するユーザー アカウントを選択します。

13. 承認が完了したら、Visual Studio codeに戻ります。

![](./media/image33.png)

14. **Sample.env**ファイルを開き、必要な環境変数を指定します。

![](./media/image34.png)

- Microsoft Foundry ポータルに移動します。My assetsの**Models +
  endpoints(1)** セクションから
  **gpt-4o(2)** をクリックし、右側のペインから**Endpoint**の下の**Target
  URI(1)**と**Key(2)**をコピーしてメモ帳に貼り付けます。

![](./media/image35.png)

![](./media/image36.png)

15. **Sample.envファイル**では、

    - AIPROJECT_CONNECTION_STRING:タスク 1 の手順 9
      でコピーした**Project connection string** **の値**を指定します。

    - CHAT_MODEL_ENDPOINT:前の手順でコピーした**gpt-4oモデル**の**Target
      URI** を指定します。

    - CHAT_MODEL_API_KEY:前の手順でコピーした**gpt-4oモデル**の**Key** の**値**を入力します。

    - チャットモデル: **gpt-4o**

![](./media/image37.png)

16. **Sample.envファイル**への変更を保存します。

17. 以下のPowerShellコマンドを実行してください。これにより、
    **.env**ファイルが作成されます。

+++cpサンプル.env .env+++

![](./media/image38.png)

18. 後で**Lab 1 - Project Setup.ipynb**ファイルを開いてください。Lab
    **1 - Project Setup.ipynb**ノートブックは、Microsoft
    FoundryでのAIプロジェクトのセットアップ、LLMのデプロイとモデルの埋め込み、VS
    Code接続の設定をガイドします。また、セットアップを確認するための簡単なChat
    Completion
    API呼び出しも含まれています。このノートブックを実行することで、AI-poweredアプリケーションの開発環境が正しく構成されていることを確認できます。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image39.png)

19. 右上隅にある**Select kernel
    (1)** を選択し、**選択した拡張機能のインストール/有効化（
    python+jupyter ）（2）**を選択します。

![](./media/image40.png)

20. **Python 環境**を選択して、必要な依存関係がインストールされた正しい
    Python インタープリターでJupyter Notebook が実行されるようにします。

![](./media/image41.png)

21. Microsoft Foundry SDK
    およびその他の依存関係との互換性を保つためにこのバージョンが必要になる可能性が高いため、リストから**venv
    (Python 3.xx)**を選択します。

![](./media/image42.png)

**注: venv (Python 3.xx)**がリストに表示されない場合は、Visual Studio
Code を閉じて開きます。

22. 最初のセルを実行して、Azure AI サービスを使用するために必要な Python
    ライブラリをインポートします。

![](./media/image43.png)

23. 以下のセルを実行すると、環境変数からプロジェクトの接続文字列とモデル名を取得できます。これらの値は、機密情報をハードコーディングすることなく、Large
    Language Models （LLM）と安全にやり取りするために必要です。

![](./media/image44.png)

24. 以下のセルを実行して、接続文字列を使用してMicrosoft
    Foundryプロジェクトに接続します。これにより、AIProjectClientとの安全な接続が確立され、プロジェクトのリソースとのやり取りが可能になります。

![](./media/image45.png)

25. 以下のセルを実行すると、Microsoft
    Foundryプロジェクトを使用してGPT-4oモデルを操作できます。このコードはチャットクライアントを初期化し、テディベアに関するジョークのリクエストを送信し、レスポンスを出力します。最後に、チャットモデルからの出力を確認してください。

![](./media/image46.png)

> **注:**このステップの出力は AI
> モデルによって動的に生成されるため、毎回異なる場合があります。

**まとめ**

このラボでは、Microsoft
FoundryでAIプロジェクトを作成および構成し、GPT-4o Large Language
Modelとtext-embedding-3-large埋め込みモデルをデプロイし、Visual Studio
Codeから安全な接続を確立することで、完全なAI開発環境を正常にセットアップしました。必要な依存関係をインストールし、仮想環境を作成し、機密情報を安全に管理するために環境変数を構成しました。最後に、簡単なChat
Completion
API呼び出しを実行してセットアップを検証し、環境が正しく構成され、AI-poweredアプリケーションの開発準備が整っていることを確認しました。

このラボは正常に完了しました。次のステップに進むには、「Next
\>\>」をクリックしてください。
