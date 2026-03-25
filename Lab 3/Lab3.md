# ラボ3: 健康保険プラン分析エージェントの作成

**概要**

このラボでは、健康保険プランデータを処理および分析するために特別に設計されたHealth
Insurance Plans Analyser AI Agent
を構築します。このインテリジェントエージェントは、プランの詳細を解釈し、補償オプションを分析し、意思決定を支援する視覚的な表現を生成することで、さまざまな健康保険プランの比較を自動化します。Microsoft
FoundryとAzure
AIサービスを使用することで、エージェントは保険プラン間の違いを明確に示す比較棒グラフを作成します。これにより、ユーザーは選択肢を理解し、最適な健康保険を選択しやすくなります。

**ラボの目的**

このラボでは次のタスクを実行します。

- タスク1: シンプルなAIエージェントを作成する

## タスク1: シンプルなAIエージェントを作成する

このタスクでは、Azure AI
サービスを使用して分析と視覚化を行い、データを処理し、さまざまな健康保険プランを比較する棒グラフを生成するシンプルな
AI エージェントを構築します。

1.  **Lab 2 - Create A Simple AI
    Agent.ipynbファイルを**開きます。この**Lab 2 - Create A Simple AI
    Agent.ipynb**ノートブックでは、データを処理し、さまざまな健康保険プランを比較する棒グラフを生成するシンプルな
    AI エージェントの構築方法を学習します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

2.  右上にある**「Select
    kernel 」**を選択します。リストから**venv（Python
    3.xx）**を選択します。

![A blue and red rectangle with white text AI-generated content may be
incorrect.](./media/image2.png)

3.  以下のセルを実行して、Azure
    AIプロジェクトを操作するために必要なライブラリをインポートし、環境変数を読み込みます。この設定により、Azure
    AIサービスとの安全な認証とやり取りが可能になります。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image3.png)

4.  以下のセルを実行してMicrosoft
    Foundryプロジェクトに接続し、デプロイされた**gpt-4o**モデルにアクセスします。これにより、プロジェクトの接続文字列とAzure資格情報を使用して安全な接続が確立されます。

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image4.png)

5.  このセルを実行すると、Microsoft Foundry
    を使用してデータを処理し、さまざまな健康保険プランを比較する棒グラフを生成する**シンプルな
    AI エージェント**が作成されます。このスクリプトは、AI
    エージェントを初期化し、健康保険プランデータを含むプロンプトを送信して、棒グラフの生成を要求します。エージェントは要求を処理し、グラフを生成して画像ファイルを保存した後、エージェントを削除してクリーンアップを行います。

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image5.png)

6.  最後に出力を観察します。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image6.png)

**まとめ**

このラボでは、Microsoft Foundry と Azure AI サービスを使用して、Health
Insurance Plans Analyser AI
Agentを構築し、健康保険プランの分析と比較を自動化しました。Microsoft
Foundry プロジェクトに接続し、デプロイ済みの GPT-4o
モデルにアクセスし、複雑な健康保険プランデータを処理するインテリジェントエージェントを作成する方法を学習しました。エージェントはプランの詳細を解釈し、補償オプションを分析し、比較棒グラフを自動生成して保険プラン間の違いを視覚化しました。このハンズオンエクスペリエンスでは、AI
エージェントがデータ分析を効率化し、意思決定プロセスをサポートし、ユーザーが健康保険オプションを簡単に理解して比較できるようにする方法を実証しました。

このラボは正常に完了しました。「Next
\>\>」をクリックして次に進んでください。
