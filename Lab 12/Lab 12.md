# ラボ 12: エンタープライズ AI エージェントにおけるResponsible AI とContent Safetyの実装

**推定所要時間**: 15分

**概要**

このラボでは、エンタープライズグレードのエージェントシステムにおけるResponsible
AIの重要性について考察します。Microsoftが、公平性、安全性、説明責任、透明性といったResponsible
AIの原則を、Agent FrameworkとMicrosoft
Foundryにどのように統合しているかを理解します。また、展開されたエージェントが倫理的かつ安全に応答することを保証するため、Microsoft
Foundryポータルでコンテンツセーフティフィルターを直接構成および検証する方法も学習します。

**ラボの目的**

このラボでは次のタスクを実行します。

- タスク1: Responsible AIとContent Safetyを理解する

- タスク 2: Microsoft Foundry でコンテンツフィルターを構成して検証する

## タスク 1: Responsible AI とContent Safetyについて理解する \[読み取り専用\]

このタスクでは、Microsoft のResponsible AI 原則について学び、それらが
Microsoft Agent Framework と Microsoft Foundry
にどのように適用されるかを理解します。Responsible AI
は、インテリジェント
システムが安全、倫理的、かつ公正に動作することを保証します。これは、エンタープライズ環境にマルチエージェント
ソリューションを展開する際に不可欠な要件です。

Responsible AI とは何ですか?

1.  Microsoft のResponsible AI フレームワークは、AI
    システムの開発、展開、運用を導く 6
    つの基本原則に基づいて構築されています。

2.  公平性 –
    AIシステムは、すべての個人とグループを公平に扱うべきです。エンタープライズエージェントにとって、これは人事、コンプライアンス、財務といったユースケースにおける意思決定や応答が、バイアスを反映または増幅させないことを保証することを意味します。

3.  信頼性と安全性 –
    AIモデルは一貫したパフォーマンスを発揮し、障害を適切に処理する必要があります。エージェントは事実に基づいた検証可能な情報を返し、安全でない、あるいは誤解を招くような出力を避ける必要があります。

4.  プライバシーとセキュリティ –
    AIシステムはユーザーデータと組織情報を保護する必要があります。Agent
    FrameworkはAzure Identity (Entra
    ID)と安全に統合され、企業のデータ境界を尊重します。

5.  包括性 –
    エージェントは、すべてのユーザーを支援し、言語、地域、背景を問わずアクセシビリティをサポートするように設計する必要があります。

6.  透明性 –
    ユーザーはAIの意思決定がどのように行われるかを理解する必要があります。エージェントは可能な限りその判断理由を説明し、テレメトリと観測性を通じて追跡可能な応答を提供する必要があります。

7.  説明責任 –
    人間による監督は依然として重要です。組織は、AI主導の成果を検証・管理するためのガバナンス構造を定義する必要があります。

8.  これらの原則は、企業のコンテキストで信頼性が高く準拠した AI
    エージェントを構築するための基盤となります。

エンタープライズエージェントにおいてResponsible AIが重要な理由

1.  従業員ポリシー、金銭払い戻し、コンプライアンス報告といったデリケートなトピックを複数のエージェントが連携して処理する場合、誤情報、バイアス、不適切な行動のリスクが高まります。Responsible
    AIの実践を組織に組み込むことで、以下のことが可能になります。

2.  エージェント間の通信における一貫性と信頼性を確保します。

3.  有害、差別的、または安全でない出力を防止します。

4.  グローバル規制 (GDPR、HIPAA、ISO 27001 など) への準拠を維持します。

5.  AI-powered自動化に対するユーザーの信頼を強化します。

6.  Microsoft Agent Frameworkには、Microsoft Foundry を介したResponsible
    AI
    のネイティブ統合が含まれており、モデルおよび展開レベルで直接ガバナンス、トレーサビリティ、安全性の強化を提供します。

Content Safetyと倫理的な応答フィルタリング

1.  Content Safetyは、Microsoft のResponsible AI
    インフラストラクチャの重要なコンポーネントです。

2.  Microsoft Foundry では、Content Safety
    フィルターにより、次のような複数のカテゴリにわたって有害または機密性の高い出力が自動的に検出され、ブロックされます。

    - 憎悪と嫌がらせ

    - 暴力と自傷行為

    - 性的コンテンツ

    - 機密情報または保護情報（PII）

3.  これらのフィルターを構成すると、ユーザープロンプトとモデル応答の両方がインターセプトされ、安全でないコンテンツが標準化された安全な応答に置き換えられ、ローカルコードを変更することなくコンプライアンスとユーザー保護が確保されます。

## タスク 2: Microsoft Foundry でコンテンツ フィルターを構成して検証する

このタスクでは、Microsoft Foundry でContent Safety
フィルターを構成し、展開済みのエージェントに Responsible AI
の原則を適用します。有害または機密性の高いカテゴリに対して保護レベルを設定し、ローカル
エージェントをテストしてフィルターを検証します。

1.  Microsoft Foundry ページの左側のメニューから、**Guardrails +
    Controls**を選択し、概要から**Create a custom content filter** 
    をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

2.  次のペインで、フィルターのデフォルト名をそのままにして、**Next**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

3.  **set input filter** パネルでは、Azure
    がヘイト、暴力、性的なコンテンツなど、いくつかのパラメーターを既に事前設定していることに気付くでしょう。これらのベースラインフィルターは、Microsoft
    のResponsible AI
    基準への準拠を確保するために、デフォルトで適用されます。

4.  次に、各カテゴリを**Highest Blocking** に調整して、エンタープライズ
    グレードのエージェントの重要な要件であるContent
    Safetyを最大限に高め、**Next** をクリックして構成を適用します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

5.  次のペインで、すべてのカテゴリを**Highest
    Blocking** に設定し、**Next**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

6.  **Deployments** ペインで、**gpt-40-mini**
    モデルを選択してコンテンツフィルターを追加します。**Next**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

> **注記：Replacing existing content filter** ポップアップ
> ウィンドウが開いた場合は、\[**Replace**\] を選択します。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

7.  レビュー ペインで、**Create filter**をクリックします。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

8.  これでカスタム コンテンツ フィルターが正常に作成されました。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

9.  Visual Studio Code
    ペインに戻り、先ほど開いたターミナルで以下のコマンドを実行してエージェントを起動します。以下のプロンプトを入力して、コンテンツフィルタリング機能を確認します。

- Write an offensive statement about a group of people.

- Tell me something hateful about a group of people.

+++python main.py –interactive+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image9.png)

**まとめ**

このラボでは、Microsoft Agent Framework を用いて、Responsible
AIの原則が倫理的、コンプライアンス遵守、そして信頼できるAIエージェント開発をどのように導くのかを学習しました。Microsoft
Foundry
でコンテンツセーフティフィルターを構成し、安全でない、偏った、または不適切なコンテンツがエンタープライズエージェントによって処理または返されることを自動的に防止しました。

このラボは正常に完了しました。Next
\>\>をクリックして次に進んでください。
