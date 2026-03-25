# ラボ4: 健康計画レポート生成のためのマルチ・エージェントシステムの開発

**概要**

このラボでは、包括的な健康保険プランレポートの自動生成に特化したインテリジェントなマルチ・エージェントシステムを開発します。このシステムは、4つの専門AIエージェントが連携して動作し、詳細な健康保険関連書類の取得、分析、生成、検証を行います。このマルチ・エージェントアーキテクチャは、自律型エージェントが連携して、単一のエージェントでは効率的に処理することが困難な複雑なタスクを遂行する方法を示します。

以下の 4 つの AI エージェントを構築します。

- **検索エージェント**- このエージェントは、Azure AI Search
  インデックスで特定の健康保険プランに関する情報を検索します。

- **レポートエージェント**-
  このエージェントは、検索エージェントから返された情報に基づいて、健康保険プランのポリシーに関する詳細なレポートを生成します。

- **検証エージェント**-
  このエージェントは、生成されたレポートが指定された要件を満たしているかどうかを検証します。このケースでは、レポートに補償対象外に関する情報が含まれていることを確認します。

- **オーケストレーターエージェント**-
  このエージェントは、検索エージェント、レポート
  エージェント、検証エージェント間の通信を管理するオーケストレーターとして機能します。

![A diagram of a company AI-generated content may be
incorrect.](./media/image1.png)

オーケストレーションは、マルチ・エージェント
システムの重要な部分です。これは、作成するエージェントが目的を達成するために相互に通信できる必要があるためです。

検索エージェント、レポートエージェント、検証エージェントの作成には、Azure
AI Agent Serviceを使用します。一方、Orchestrator Agentの作成にはSemantic
Kernelを使用します。Semantic
Kernelライブラリは、マルチ・エージェントシステムのオーケストレーションに必要な機能をすぐに提供します。

**ラボの目的**

このラボでは次のタスクを実行します。

- タスク 1: Azure AI 検索インデックスを作成する

- タスク 2:
  検索エージェント、レポートエージェント、および検証エージェントを作成する

## タスク 1: Azure AI 検索インデックスを作成する

このタスクでは、**Azure AI 検索インデックス**を作成し、健康保険プラン
ドキュメントのベクトル化された表現を保存して、AI
駆動型検索と分析のための効率的な取得を可能にします。　

1.  **Azureポータル**に移動し、**AI Search (1)**
    を検索して、サービスから **AI Search (2)** リソースを選択します。

![](./media/image2.png)

2.  そうすると、 AI Foundry に移動し、**AI Search (1)** 内で **Create
    (2)** をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

3.  「**Create a Search
    service** 」パネルで次の詳細を入力し、「**Review +
    Create** (4)」をクリックします。

    - Subscription: デフォルトのサブスクリプションのままにします。

    - Resource Group: **AgenticAI (1)** を選択します。

    - Service Name：**my-search-service- (2)**

    - Location: (3)

![](./media/image4.png)

4.  「**Review + Create**」で「**Create**」をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

5.  デプロイメントが完了するまで待ってから、**Go to
    resource**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

6.  左側のメニューの**Settings** から**Keys**（1）に移動します**API
    Access control**で**Both**（2）を選択します。

![](./media/image7.png)

7.  **Are you sure want to update the API Access Control for this serach
    service**? で \[**Yes** \] を選択します。

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image8.png)

8.  「**Settings**」の**Identity** (1)
    に移動します。System-assignedでStatusを「**On**」(2)
    に設定し、**Save** (3) をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.png)

9.  **Enable System assigned managed identity**には \[**Yes** \]
    を選択します。

![A close-up of a computer error AI-generated content may be
incorrect.](./media/image10.png)

10. Azure ポータルで、**Storage accounts** (1)
    を検索し、サービスから**Storage accounts** (2) を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

11. **aifoundry** で始まるストレージ アカウントを選択します。

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image12.png)

12. **Access
    control（IAM）（1）**を選択し、**Add（2）**をクリックして、**Add
    role assignment**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

13. **Job function roles**の下で、**Storage Blob Data Reader (1)** (1)
    を検索し、**Storage Blob Data Reader** **(2)** を選択して、**Next
    (3)** を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

14. **Add role assignment** ページで、

    - Membersの下で、**Managed identity(1)**を選択します。

    - **Members (2)**を選択します。

    - Managed identity: **search service(1)** **(3)**

    - 次に**my-search-service-**(4)検索サービスを選択します。

    - **Select (5)**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.png)

15. 「**Review + assign** 」を 2 回クリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image16.png)

16. **Azure OpenAI**, **my-openai-service**にアクセスします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.png)

17. **Access control (IAM)
    (1)**を選択し、**Add**（2）をクリックして、**Add role
    assignment**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.png)

18. \[**Job function roles**\] で **Cognitive Services OpenAI User
    (1)**を検索し、**Cognitive Services OpenAI User (2)**を選択してから
    **Next (3)**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.png)

19. **Add role assignment** ページで、　

    - Membersの下で、**Managed identity(1)**を選択します。

    - **Members (2)**を選択します。

    - Managed identity: **search service(1)** **(3)**

    - 次に**my-search-service-**(4)検索サービスを選択します。

    - **Select (5)**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

20. **Review + assign** を 2 回選択します。

![](./media/image21.png)

21. Azure ポータルに移動し、**Storage account
    (1)** を検索して、**Storage account (2)**を選択します。　

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

22. **aifoundryhub** で始まるストレージ アカウントを選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

23. データストレージの下の**Containers
    (1)** をクリックし、**+Container(2)**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

24. New
    Containerページで、名前として**healthplan(1)** を入力し、**Create
    (2)**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

25. **healthplan** コンテナーをクリックして開きます。

![](./media/image26.png)

26. **Upload
    (1)** をクリックして、ファイルをアップロードし、次に**browse for
    files (2)**をクリックします。

> ![](./media/image27.png)

27. C:\LabFiles\Day-1\azure-ai-agents-labs\data (1)
    に移動し、アップロードする PDF を両方選択し (2)、**Open
    (3)**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.png)

28. **Upload**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.png)

**注記：**既存のコンテナを選択するように求められた場合は、ドロップダウンから
healthplan を選択します。

29. Azure AI 検索サービスに移動し、**my-search-service-** を選択します。

![](./media/image30.png)

30. **import data (new)**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

31. **Azure blob storage**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image32.png)

32. **RAG** モデルを選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

33. Azure Blob Storage の構成で、次の詳細を入力し、**Next (5)**
    をクリックします。

[TABLE]

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image34.png)

34. 「テキストをベクター化する」で、次の詳細を入力し、「次へ」(7)
    をクリックします。

[TABLE]

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image35.png)

35. 「**Next** 」を2回クリックします。

36. **Objects name prefix** に **health-plan (1)** と入力し、**Create
    (2)**をクリックします。

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image36.png)

**注記**: 検索サービスのインデックスへのデータのアップロードには 5 ～ 10
分かかる場合があります。

37. ポップアップで「**Start searching** 」をクリックします。

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image37.png)

38. **ai-foundry-project-**の**Overview** (1)ページに移動し、**Open In
    management center**(2)をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

39. **Connected resources** (1)を選択し、**New
    connection** (2)をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.png)

40. 検索バーに**Azure AI Search**(1)と入力し、**Azure AI
    Search**(2)を選択します。

![](./media/image40.png)

41. **Add connection** をクリックして続行します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.png)

## タスク 2: 検索エージェント、レポートエージェント、検証エージェントを作成する

このタスクでは、健康プランレポートの取得、生成、検証を行う検索エージェント、レポートエージェント、検証エージェントを作成します。これらのエージェントは連携して、レポートの正確性と要件への準拠を確保します。各エージェントは、レポートの取得、コンパイル、そして正確性の確保において、それぞれ異なる役割を果たします。

1.  **Lab 4 - Develop A Mult-Agent System.ipynb**
    ファイルを開きます。この **Lab 4 - Develop A Mult-Agent
    System.ipynb**
    ノートブックでは、検索エージェント、レポートエージェント、検証エージェント、オーケストレーターエージェントを備えたマルチ・エージェントシステムを開発し、医療保険プランレポートの生成と検証を行う手順を解説します。各エージェントは、レポートの取得、コンパイル、そして正確性の確保という異なる役割を果たします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.png)

2.  右上隅にある**Select kernel (1)** 設定を選択し、リストから**venv
    (Python 3.x.x) (2)** を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.png)

3.  このセルを実行すると、Azure AI Search、GPT-4o、Semantic
    Kernelを統合し、インテリジェントなタスク実行を実現するマルチ・エージェントシステムを開発できます。このセットアップにより、複数のAIエージェントが連携して、情報の取得、レスポンスの生成、複雑なクエリの処理を行うことができます。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image44.png)

4.  このセルを実行すると、GPT-4o を使用して Azure AI Search
    から健康保険プランの詳細を取得する**検索エージェント**が作成されます。このエージェントにより、健康保険プランのドキュメントから構造化された情報を効率的に取得できます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.png)

5.  このセルを実行すると、GPT-4oを使用して健康保険プランに関する詳細なレポートを生成する**レポートエージェント**が作成されます。このエージェントは、様々なプランの構造化されたインサイト、補償内容の詳細、除外事項を提供することで、ドキュメント作成を強化します。

![](./media/image46.png)

6.  このセルを実行して**検証エージェント**を作成します。これにより、レポート
    エージェントによって生成されたレポートが品質基準を満たしていることが確認され、特に対象範囲の除外がチェックされます。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.png)

7.  **マルチ・エージェントシステムを作成する**:
    以下のセルを実行すると、VS Code の上部にチャット
    ボックスがポップアップ表示され、健康保険プランの名前を入力するように求められます。

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image48.png)

8.  ご存知のとおり、2つの健康保険プランを検索インデックスにアップロードしました。プロンプトが表示されたら、上部に表示されるボックスに以下の健康保険プランのいずれかを入力し、Enterキーを押して、マルチ・エージェントシステムの実行を開始してください。

    - **Northwind Health Standard**

    - **Northwind Health Plus**1

![](./media/image49.png)

9.  上部にボックスが表示されたら、ボックスに exit と入力して Enter
    キーを押して、実行中のコード ブロックを停止します。

**注記**: セルの実行が正常に完了すると、次の結果が表示されます。

> Orchestrator Agent is starting...
>
> Calling SearchAgent...
>
> SearchAgent completed successfully.
>
> Calling ReportAgent...
>
> ReportAgent completed successfully.
>
> Calling ValidationAgent...
>
> ValidationAgent completed successfully.
>
> The report for Northwind Plus has been generated. Please check the
> Northwind Plus Report.md file for the report.
>
> Orchestrator Agent is starting...

**まとめ**

このラボでは、4 つの専用 AI
エージェントを連携させることで、包括的な健康保険プランレポートの生成を自動化するインテリジェントなマルチ・エージェント
システムを開発しました。ベクトル化された健康保険ドキュメントを保存するための
Azure AI Search
インデックスを作成し、ポリシー情報を取得する検索エージェント、詳細なドキュメントを生成するレポートエージェント、要件への準拠を確認する検証エージェント、そしてSemantic
Kernelを使用してすべてのエージェント間の通信を管理するオーケストレーター
エージェントを構築しました。実際の健康保険プランデータを使用してマルチ・エージェント
システムを実行することで、自律型エージェントが効果的に連携し、単一のエージェントでは困難な複雑なタスクを達成する様子を実証し、実用的なビジネス
アプリケーション向けのエンタープライズ グレードのエージェント
オーケストレーション パターンを示しました。

おめでとうございます！ラボは完了しました。
