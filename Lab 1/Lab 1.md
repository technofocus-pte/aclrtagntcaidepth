# インテリジェントエージェントの構築と拡張

**概要**

このハンズ・オン・ラボでは、Azure AI サービスと Microsoft 365 Copilot
を用いたインテリジェント AI
エージェントの構築方法を紹介します。参加者は、Copilot を HR
ワークフローに活用する方法、Microsoft Foundry
プロジェクトの設定方法、シンプルな AI エージェントの構築方法、RAG
(Retrieval-Augmented Generation)
エージェントの作成方法、オーケストレーション機能を備えたマルチ・エージェント
システムの開発方法を学習します。

**目的**

このラボを終了すると、次のことができるようになります。

- **Copilot Studio を使用して HR アシスタント エージェントを構築する**-
  Microsoft 365 Copilot
  を使用して、従業員の採用、スクリーニング、トレーニング
  マテリアルの開発、フィードバックの収集、パフォーマンス
  レビューを自動化します。

- **AI プロジェクトをセットアップしてチャット完了を実行する**- Microsoft
  Foundry で AI プロジェクトを構成し、Large Language Models (LLM)
  と埋め込みモデルをデプロイし、チャット完了のための VS Code
  接続を確立します。

- **健康保険プラン分析 AI エージェントの構築**- Azure AI
  サービスを使用して、データを処理し、視覚化
  (健康保険プランを比較する棒グラフなど) を生成する AI
  エージェントを作成します。

- **健康計画レポート生成マルチ・エージェント システムの開発**-
  専門エージェント (検索、レポート、検証、オーケストレーター
  エージェント)
  が連携して複雑なタスクを実行する、調整されたマルチ・エージェント
  システムを設計および実装します。

**前提条件**

参加者には以下の条件が必要です:

- **Visual Studio Code (VS Code)** :
  さまざまなプログラミング言語やフレームワークの拡張機能のコーディング、デバッグ、管理に
  VS Code を使用する能力。

- **開発スキル**: Python または JavaScript
  の基本的なプログラミング知識、API、SDK の使用経験、Visual Studio Code
  での作業経験。

- **コマンドライン・ターミナル**: PowerShell
  コマンドの実行と仮想環境の管理に関する知識。

**コンポーネントの説明**

- **Azure AI Search** : 関連ドキュメントのインデックス作成と取得によって
  RAG を可能にするベクターベースの検索サービス。

- **Azure OpenAI サービス**: Azure のエンタープライズ
  インフラストラクチャを通じて GPT-4o
  および埋め込みモデルへのアクセスを提供します。

- **Large Language Models (LLM)** : テキストの理解と生成のための GPT-4o
  などの高度な AI モデル。

- **埋め込みモデル**:
  テキストをセマンティック検索と取得のためのベクトル表現に変換します
  (例: text-embedding-3-large)。

- **Microsoft 365 Copilot** :
  ドキュメント分析とワークフロー自動化のための
  AI-powered生産性向上ツール。

- **Semantic Kernel**: LLM
  をプログラミング言語と統合し、オーケストレーション機能を構築するためのSDK。

# ラボ 1: Copilot Studio で HRアシスタントエージェントを構築する

推定所要時間: 30分

概要

このラボでは、Microsoft 365 Copilot と Copilot Studio
を使用して、組織内の従業員の移行およびオンボーディング
プロセスの効率化と改善に焦点を当てます。適切な候補者の特定、カスタマイズされた移行およびオンボーディング
プランの作成、効果的なコミュニケーションおよびトレーニング
マテリアルの作成、人事ワークフローの自動化、フィードバックの収集、パフォーマンスの監視とレビューのメカニズムの構築方法を学習します。これらの
AI
poweredツールを活用することで、組織がスムーズで効率的な移行プロセスを実現し、社内モビリティを向上させ、従業員が新しい役割にスムーズに適応できるようサポートする方法を実証します。

ラボの目的

このラボでは次のタスクを実行します。

- タスク1: 候補者を迅速にスクリーニングする

- タスク2: トレーニング資料の作成

- タスク3: フィードバックを収集する

- タスク4: パフォーマンスレビュー

アーキテクチャ図

![image](./media/image1.png)

## タスク1: 候補者を迅速にスクリーニングする

このタスクでは、Microsoft 365 Copilot
を使用して履歴書を分析し、関連する経験、技術スキル、学歴などの特定の基準に基づいて候補者をフィルター処理し、Copilot
でさらに検討すべき最有力候補者を強調表示できるようにすることで、データ
アナリスト職に対する多数の応募を迅速に評価します。

1.  Edgeブラウザで新しいタブを追加し、次のリンクを使用してMicrosoft 365
    Copilotアプリを開き、「**Sign in」**をクリックします。 **（２** ）

+++https://m365.cloud.microsoft/+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

2.  **「Sign into Microsoft
    Azure」タブ**に、ログイン画面が表示されます。以下の資格情報を使用してログインしてください。

- Username - +++@lab.CloudPortalCredential(User1).Username+++

- TAP - +++@lab.CloudPortalCredential(User1).TAP+++

3.  **「Welcome to your Microsoft 365 Copilot
    app」**というポップアップが表示されたら、「**Get
    started」**をクリックします。

![A screenshot of a computer application AI-generated content may be
incorrect.](./media/image3.png)

4.  左ペインから**Apps（1）**を選択します。アプリセクションから **OneDrive（2）**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

**注: 「Welcome to Apps」**ポップアップが表示された場合は、
**「X」**をクリックしてポップアップを閉じます。

![A screenshot of a computer application AI-generated content may be
incorrect.](./media/image5.png)

5.  **「My files」**に移動し、 **+ Create or upload
    (1)**ボタンをクリックして、 **Folder upload (2)**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

6.  C:\LabFiles\Day-1\data **(1)**に移動し、CV
    **(2)**フォルダをクリックして、**Upload (3)**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

7.  「Upload 5 files to this
    site?」ポップアップで**「Upload」**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

8.  もう一度、 **+ Create or upload (1)**をクリックし、 **Folder upload
    (2)**を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

9.  C:\LabFiles\Day-1
    **(1)に移動し**、データ**(2)**ファイルをクリックして、**Upload
    3**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.png)

10. 「Upload 19 files to this
    site?」ポップアップで**「Upload」を**選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.png)

11. **M365 Copilot**に戻り、左ペインから**Apps
    (1)**を選択します。次に、Appsセクションから **Copilot
    (2)**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

12. 左ペインから**Copilot**に移動し、
    **Chat（1）**をクリックします。次に、チャットペインの下部にある**+
    (Add)**アイコン**（2）**をクリックし、 **Upload images and files
    (3)**を選択します。

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image12.png)

13. ファイルエクスプローラーのポップアップで、C:\LabFiles\Day-1\data\CV
    **(1)**フォルダに移動し、**first 3 (2)**を選択して、**Open
    (3)**をクリックします。

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image13.png)

14. **Copilot chat**で**3 つのファイル**が正常にアップロードされたら、
    **Enter**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

15. アクティブなCopilot chatで、メッセージボックスの下にある**+ (Add)
    (1)**アイコンをクリックし、**Upload images and files
    (2)**を選択します。　

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.png)

16. ファイルエクスプローラーのポップアップで、C:\LabFiles\Day-1\Data\CV
    (1)フォルダに移動し、**last 2 (2) **を選択して** Open
    (3).**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image16.png)

17. **Copilot chat**で、 **2 files
    (1)**が正常にアップロードされたら**、Enter (2)**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.png)

18. チャットボックスに次のプロンプト**（1）を入力し、送信（2）ボタン**を押します。

> Microsoft 365 Copilot, please help me filter and shortlist resumes of
> Data Analyst candidates based on required qualifications such as
> experience in SQL, Python, and data visualization tools.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.png)

19. 以下のプロンプトに従って**送信**ボタンを押します

> Create a summary report of top Data Analyst candidates, including
> their skills, work experience, and educational background.
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

**結果**: HR
チームは最も適格な候補者を効率的に特定し、時間を節約して、集中的な採用活動を確実に行うことができます。

## タスク 2: トレーニング マテリアルを開発する

このタスクでは、Microsoft Copilot
を使用して、役割固有のガイド、会社のポリシー、使用するツールとテクノロジの概要などのカスタマイズされたオンボーディング
コンテンツを作成し、新入社員向けの包括的なトレーニング
マテリアルを準備します。これにより、トレーニング
マテリアルが徹底的かつ適切に構造化され、従業員の役割に合わせて調整されていることを確認します。

1.  チャットボックスに次のプロンプト**（1）**を入力し**、送信（2）**ボタンを押します。

> Generate a comprehensive onboarding training plan for the new Data
> Analyst, including topics like company policies, data tools training,
> and team introductions.
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image20.png) ![A screenshot of a web page
> AI-generated content may be incorrect.](./media/image21.png)

2.  **以下のプロンプト（1）**に従って**送信（2）**ボタンを押します。

> Create an interactive training presentation covering data analysis
> best practices and key performance metrics and generate a downloadable
> PPT.
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image22.png)

**注**：このプロンプトを実行すると、PowerPointプレゼンテーションがダウンロードされ、編集またはデザインできます。ファイルがダウンロードされていない場合は、スクリーンショットに示されているように、プレゼンテーションのタイトルのハイパーリンクを探してみてください。

**注**：このプロンプトを実行した後、ダウンロードするPowerPointプレゼンテーションのオプションが表示されません。上記のプロンプトを再度実行してください。

結果:
新入社員は整理されたトレーニング資料を受け取るため、すぐに業務に慣れて効果的に職務を遂行できるようになります。

## タスク3: フィードバックを収集する

このタスクでは、Microsoft Copilot を使用してフィードバック
アンケートを生成および配布し、回答を収集および分析して、採用およびオンボーディング
プロセスの長所と改善が必要な領域に関する分析情報を取得することで、新入社員と面接担当者からのフィードバックを収集します。

1.  チャット ボックスで次のプロンプトを入力し、
    **\[送信\]**ボタンをクリックします。

> Create a feedback form for interviewers to evaluate Data Analyst
> candidates based on technical skills, problem-solving abilities, and
> cultural fit Generate a downloadable Word or PDF version of this
> feedback form.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

2.  以下のプロンプトに従って、 **「送信」**ボタンを押します。

> Send out a survey to new hires to gather feedback on their onboarding
> experience and identify areas for improvement Generate a downloadable
> Word or PDF version of the survey.
>
> ![A screenshot of a survey AI-generated content may be
> incorrect.](./media/image23.png)
>
> 結果:
> 人事部門は貴重なフィードバックを得て、採用およびオンボーディングの慣行を改善し、将来の採用者により良い体験を提供できるようになります。

## タスク4: パフォーマンスレビュー

このタスクでは、Microsoft Copilot を使用してパフォーマンス レビュー
テンプレートを作成し、レビュー
ミーティングをスケジュールし、成果を追跡し、同僚からのフィードバックを収集し、構造化されたパフォーマンス
レポートをコンパイルすることで、定期的にパフォーマンス
レビューを実施し、新入社員の進捗状況と成長を評価します。

1.  チャット ボックスで次のプロンプトを入力し、
    **\[送信\]**ボタンをクリックします。

> Set up a performance review schedule for the new Data Analyst, with
> quarterly reviews and goal-setting sessions and Generate a calender
> CSV file.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

2.  以下のプロンプトに従って、 **「送信」**ボタンを押します。

> Generate a template for performance review reports, including sections
> for achievements, areas of improvement, and future goals Generate a
> performance review template.
>
> ![A screenshot of a report AI-generated content may be
> incorrect.](./media/image25.png)
>
> 結果:
> 新入社員は建設的なフィードバックとサポートを受け、専門的な成長を促進し、会社内での長期的な成功に貢献します。
>
> **まとめ**
>
> このラボでは、Microsoft 365 Copilot を使用して HR
> アシスタントエージェントを構築し、従業員の採用とオンボーディングのプロセスを効率化しました。履歴書を分析し、SQL、Python、データ視覚化などの技術スキルに基づいてフィルタリングすることで、データアナリストの候補者を迅速に選別する方法を学習しました。さらに、新入社員向けの包括的なオンボーディング
> トレーニング
> プランとインタラクティブなプレゼンテーションを作成しました。面接官向けのフィードバック
> フォームと新入社員向けのアンケートを作成し、採用プロセスを評価・改善しました。さらに、実績と目標を追跡するための構造化テンプレートを使用して、四半期ごとの業績評価スケジュールを設定しました。AI-poweredツールを活用することで、組織が
> HR
> ワークフローを自動化し、効率を高め、新入社員のスムーズな移行プロセスを実現する方法を示しました。
>
> このラボは正常に完了しました。「Next
> \>\>」をクリックして次に進んでください。
