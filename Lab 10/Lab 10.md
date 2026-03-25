# 大規模管理、保護和監控AI代理

**概述**

該實踐實驗室專注于利用Azure AI Agent Service SDK和Microsoft
Foundry大規模管理、保護和監控AI代理。參與者將深入探討企業AI部署中必不可少的生產級實踐，首先是AgentOps——通過OpenTelemetry集成和Azure應用洞察來觀察和管理AI代理的學科。
研討會通過可配置的內容安全過濾器，通過檢測和阻止仇恨言論、暴力和敏感信息等有害輸出，實施Microsoft的六大基本原則，包括公平、可靠性、隱私和問責制，強調負責任
AI
的重要性。此外，參與者還將構建複雜的人工參與（HITL）工作流程，例如一個欺詐檢測系統，專業AI代理分析可疑活動，並將高風險案件智能地引導至人工分析師，以實現關鍵決策。在整個實驗室中，您將使用多代理系統，這些系統在檢索、驗證和編排任務中協作，積累端到端追蹤、自定義指標可視化、性能監控儀錶盤和實時工作流程管理的實踐經驗。到研討會結束時，參與者將掌握在企業環境中部署、監控和管理
AI
代理的關鍵技能，確保其在大規模安全、倫理且高效運行的同時，遵守組織政策和監管要求。

**目標**

到這個實驗結束時，你就能:

- **啟用可觀察性和監控**:
  利用OpenTelemetry集成Azure應用洞察，為AI代理實現端到端追蹤和遙測，捕捉代理行為、性能指標和執行跟蹤

- **可視化代理指標**: 在 Application Insights
  中創建自定義儀錶盤和工作簿，實時監控代理性能、響應時間、令牌使用情況、路由準確性和系統健康狀況

- **實施 Responsible AI 實踐**: 在 Microsoft Foundry
  中配置內容安全過濾器，以檢測並阻止有害輸出（仇恨言論、暴力、敏感內容），並確保
  AI 行為符合倫理和合規

- **構建人機作流程**:
  設計並部署欺詐檢測系統，讓AI代理分析警報，並將高風險案件轉交給人工分析師進行審核和決策

- **監控多智能體系統**:
  跟蹤代理間通信，追蹤多個專業代理間分布式工作流，並識別複雜代理編排中的瓶頸或故障

組件說明

- **Microsoft Foundry**:
  一個基於雲的平臺，用於開發、部署和管理具有集中治理、可觀察性和合規功能的AI模型，適用於企業AI應用。

- **Azure AI Hub**: 頂級 Azure
  資源為團隊提供一個集中、安全且協作的環境，以構建、管理和部署 AI
  應用，共享資源和治理策略。

- **Azure AI 搜索**:
  一項基於矢量的搜索服務，通過索引和檢索相關文檔，實現檢索增強生成（RAG），以提升基於信息的AI生成響應。

- **Azure AI 服務**: 一組基於雲的 AI
  服務，提供預構建且可定制的視覺、語言、語音和決策能力的API和模型。

- **OpenTelemetry**:
  一個面向分布式追蹤、指標和日誌的開放標準，原生集成於 Microsoft Agent
  Framework，用於捕捉代理執行跟蹤、性能指標和錯誤跟蹤。

- **內容安全過濾器**: Microsoft Foundry
  內置過濾系統，能夠自動檢測並屏蔽仇恨言論、暴力、性內容和敏感信息（PII）等類別的有害輸出。

- **LLMs與嵌入**：大型語言模型提供自然語言理解和生成，而嵌入則是用於文本相似度、搜索和知識檢索的向量表示，應用於AI應用。

# 實驗10：先決條件——建立知識索引和工單系統

**預計時長**：30分鐘

**概述**

在這個先修實驗室中，你將建立一個由 AI
驅動的工作流程所需的基礎組件，能夠檢索企業知識並自動創建支持工單。重點是準備一個可搜索的知識庫，使AI代理能夠使用MCP（模型上下文協議）工具查詢這些知識，並集成工單系統以實現下游作。

通過完成這些任務，您將建立核心基礎設施，使客服能夠:

- 從索引數據中檢索相關信息

- 在對話或工作流程中結合上下文運用這些信息

- 通過在外部服務中創建工單來升級問題

這種安排確保後續實驗室順利運行，反映真實的企業場景。

實驗室目標

你將在實驗室執行以下任務。

- 任務1：準備知識索引

- 任務2：設置Freshworks用於工單管理

## 任務1：創建Azure資源

在這個任務中，你將創建完成該實驗室所需的所有Azure資源。

### 任務1.1：創建存儲賬戶

1.  使用以下憑據登錄 Azure 門戶 +++https://portal.azure.com+++
    並選擇存儲賬戶。

- 用戶名 - +++@lab.CloudPortalCredential(User1).Username+++

- TAP - <+++@lab.CloudPortalCredential(User1).TAP>+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image1.png)

2.  選擇 **Create**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

3.  輸入以下信息，選擇 **Review + create**。在下一界面選擇“Create”。

- 存儲賬戶名稱 - +++aistorage@lab.LabInstance.Id+++

- 首選存儲類型 – 選擇 **Azure Blob Storage or Azure Data Lake Storage
  Gen2**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image3.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image4.png)

4.  資源創建後，選擇 **“Go to resource**”。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

5.  選擇 **Upload**，選擇**Create new**容器以創建新容器。命名為
    +++**datasets**+++，然後選擇 **Ok**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

![A screenshot of a login box AI-generated content may be
incorrect.](./media/image7.png)

6.  選擇“**Browse for files**”，從**C:\Labfiles\Day
    2**中選擇策略文件，點擊 **Upload**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

![A screenshot of a upload box AI-generated content may be
incorrect.](./media/image9.png)

現在，存儲賬戶已成功創建並加載了策略文檔。

### 任務1.2：創建Foundry資源

在此任務中，您將創建一個 Foundry 資源，訪問 Microsoft Foundry 是必要的。

1.  在Azure門戶（+++https：//portal.azure.com+++）主頁，選擇
    **Foundry**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.png)

2.  從左側窗格選擇**Foundry**，然後選擇**Create** Foundry資源。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

3.  輸入以下信息，選擇 **Review + create**。

- 名稱 – <+++agentic-@lab.LabInstance.Id>+++

- 默認項目名稱 – <+++agentic-ai-project-@lab.LabInstance.Id>+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.png)

4.  驗證後選擇**Create**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

5.  確保資源已經建立。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

6.  打開 **<agentic-ai-project-@lab.LabInstance.Id>** ，選擇 **“Go to
    Foundry portal**”。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.png)

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image16.png)

7.  在 Microsoft Foundry 中，從左側面板選擇 Models + 端點。選擇 +
    **Deploy model** -\> **Deploy base model**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.png)

8.  搜索 +++gpt-4o-mini+++，選擇並點擊確認以部署該模型。

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image18.png)

9.  在部署窗口中選擇 **Deploy**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.png)

10. 同樣，搜索 +++text-embedding-ada-002+++ 並部署。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image21.png)

在這個任務中，你已經成功創建了Foundry資源，並在其中部署了一個聊天和一個嵌入模型。

### 任務1.3：創建應用洞察

在此任務中，您將創建應用洞察資源，這是監控所必需的。

1.  在Azure門戶的主頁，選擇 **“Subscriptions”** 並選擇分配的訂閱。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

2.  從左側面板選擇 **Resource providers**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

3.  搜索 +++Operational+++，選擇 **Microsoft.OperationalInsights** 旁的
    3 個點，點擊 **Register**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

4.  在 Microsoft Foundry 的左側面板中，選擇 **“Monitoring**”。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

5.  選擇 **Create New** -\>，輸入名稱為 [as
    <+++agent-insights-@lab.LabInstance.Id>+++，然後選擇](mailto:+++agent-insights-@lab.LabInstance.Id+++，然后选择)
    **Create**。

![A screenshot of a application AI-generated content may be
incorrect.](./media/image27.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.png)

在這個任務中，你創建了應用洞察資源。

### 任務1.4：創建搜索資源

在AI代理能夠準確回答企業問題之前，必須訪問可信的數據源。Azure AI
搜索通過索引政策、合同和手冊等文檔，實現檢索增強生成（RAG）。索引就像一個可搜索的目錄，將內容拆分成塊，添加元數據，並使代理在對話中檢索到正確的信息。

在此任務中，利用 Azure AI Search 索引上傳的文檔，創建可搜索的知識庫。

1.  在 Azure 門戶的主頁，選擇 **Foundry**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.png)

2.  從左側窗格選擇**“AI Search**”，然後選擇 **+ Create。**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

3.  輸入以下信息，選擇 **Review + create**。

- 服務名稱 - +++ai-knowledge-@lab.LabInstance.Id+++

- 地區 - East US2

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image32.png)

4.  驗證通過後選擇 **Create**。創建資源後選擇“Go to resource”。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image34.png)

5.  選擇 **Import data (new)** 。

![A screenshot of a search engine AI-generated content may be
incorrect.](./media/image35.png)

6.  在“**Azure Blob Storage**”下選擇 **Choose data source**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.png)

7.  在下一格，選擇**RAG**選項，因為我們正在構建基於檢索的代理。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image37.png)

> 以下是這些選項的用途 -

1.  **關鍵詞搜索：**用於基於精確關鍵詞的傳統搜索體驗。它會索引文本，讓用戶通過關鍵詞匹配找到信息，無需AI推理。

2.  **RAG（檢索增強生成）：**結合文檔檢索與 AI
    生成。它會接收文本（以及簡單的OCR圖像），因此AI代理能夠提供紮實、具上下文感知的回答。

3.  **多模態RAG：**擴展RAG以處理複雜的視覺內容，如圖表、表格、工作流程或圖表。它使
    AI 能夠解讀文本和視覺元素，提供更豐富、基於洞察的回答。

&nbsp;

8.  在 **datasets** **under Blob containe**r 中的 **Storage account**
    下選擇 <aistorage@lab.LabInstance.Id>，然後選擇“**Next**”。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

9.  請選擇以下詳情，然後選擇 **“Next**”。

- Kind – Azure AI Foundry （預覽版）

- Azure AI Foundry/Hub 項目 – <agentic-ai-project-@lab.LabInstance.Id>

- 模型部署 – text-embedding-002-ada

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.png)

10. 在接下來的界面選擇“**Next**”，直到出現 **Review and create** 界面。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image40.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.png)

11. 在“**Review and create**”界面選擇 **Create**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.png)

12. 在“創建成功”對話框中選擇 **Close**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.png)

你已經成功將數據集導入 Azure AI
搜索並創建了可搜索索引。在下一個任務中，你將創建一個 AI
代理，並將該索引作為其知識來源連接起來。

# 任務2：設置Freshworks用於工單管理

在此任務中，您將搭建並配置Freshworks，以啟用工單管理和多代理系統的企業集成。

**Freshworks**
是一個基於雲的客戶服務和互動平臺，旨在提升客戶支持運營和用戶滿意度。它提供一套工具，包括工單管理、在線聊天、幫助中心創建和客戶自助服務。Freshworks
支持全渠道通信，使企業能夠通過集中界面管理電子郵件、聊天、電話和社交媒體上的客戶互動。其自動化功能有助於簡化工作流程、分配工單，並提供績效跟蹤的分析。現在你要創建Freshworks賬戶。

1.  複製URL並粘貼到虛擬機內瀏覽器的新標簽頁，打開**Freshworks**門戶。

    - URL:

> +++https://www.freshworks.com/freshdesk/lp/home/?tactic_id=3387224&utm_source=google-adwords&utm_medium=FD-Search-Brand-India&utm_campaign=FD-Search-Brand-India&utm_term=freshdesk&device=c&matchtype=e&network=g&gclid=EAIaIQobChMIuOK90qvLjQMV_dQWBR3JAi9VEAAYASAAEgK87_D_BwE&audience=kwd-30002131023&ad_id=282519464145&gad_source=1&gad_campaignid=671502402+++

2.  在門戶中，選擇 **Start free trial** 即可開始免費試用。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.png)

3.  在下一頁輸入這些信息，並點擊“**Try it free (6)”**:

    - **名字:** LODS

    - **姓氏:** User1

    &nbsp;

    - **工作郵箱:** **+++@lab.CloudPortalCredential(User1).Username+++**

    &nbsp;

    - **公司名稱:** Zava

    - **組織規模:** 選擇**1-10**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image45.png)

4.  在下一欄填寫這些信息，點擊**“Next (4)”**:

    - **What industry are you from ?:** 從列表中選擇**Software and
      internet (1)**

    - **How many employees are there in your company?:** 選擇 **1-10
      (2)**

    - 選擇 **I'm trying customer service software for the first time
      (3)**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image46.png)

5.  完成後，複製給出的URL，粘貼到虛擬機內瀏覽器的新標簽頁中打開**Outlook**。

    - URL:

> +++https://go.microsoft.com/fwlink/p/?LinkID=2125442&clcid=0x409&culture=en-us&country=us+++

6.  在“pick an account”面板中，選擇你被分配給這個實驗的賬戶。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.png)

7.  在Freshworks驗證郵件中，打開並點擊“**Activate Account**”。

> ![A screenshot of a computer screen AI-generated content may be
> incorrect.](./media/image48.png)

**注意：**如果您找不到Freshworks的激活郵件，請稍等幾分鐘，因為郵件發送可能會有延遲。如果郵件過了一段時間還沒到，可以考慮在新的私密/無痕窗口重新激活免費試用的步驟。另外，檢查垃圾郵件或垃圾郵件文件夾，因為郵件可能被過濾到了那裡。

8.  在下一欄， **Enter password (1)** ，**Confirm password (2)**
    輸入相同的密碼。點擊 **Activate your account (3)**。

> ![A screenshot of a login screen AI-generated content may be
> incorrect.](./media/image49.png)

9.  進入門戶後，點擊右上角的 **Profile (1)** 圖標，選擇 **Profile
    settings (2)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.png)

10. 在個人資料頁面，點擊“**View API Key** ”即可獲取 API 密鑰。

![A screenshot of a web page AI-generated content may be
incorrect.](./media/image51.png)

**注意：**如果您找不到該選項，請使用**CTRL + -**來最小化屏幕大小。

11. 在下一格，填寫 **CAPTCHA**。

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image52.png)

12. 請將API Key 複製到記事本，你將會繼續使用它。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image53.png)

13. 請在瀏覽器標簽頁複製顯示的
    **賬戶URL**，並將數值複製到記事本。你還會繼續使用它。

![](./media/image54.png)

**摘要**

完成這個先修實驗，你為端到端的代理工作流程奠定了基礎。你準備了一個可搜索的知識索引，使客服通過基於**Azure
AI Search**的MCP工具查詢這些數據，並集成
**了Freshworks**實現自動化工單管理。

這一基礎確保代理能夠獲取準確的上下文，做出明智決策，並高效升級問題，為即將到來的實驗室中更高級的代理驅動場景做好準備。

你已經成功完成了這個實驗。請點擊“Next \>\>”以繼續閱讀
