# 使用 Microsoft Foundry 和 Agent Framework 設計可擴展的 AI 代理

**概述**

這次為期三天的實踐實驗，利用Microsoft
Foundry和Microsoft代理框架設計和構建可擴展的AI代理。參與者將首先通過Microsoft
Foundry門戶創建他們的第一個AI代理，學習如何上傳企業政策文檔並將其導入Azure
AI Search，以準備可搜索的知識庫。研討會隨後進入使用 Microsoft Agent
Framework SDK
構建多代理系統，多個專業代理通過代理間（Agent-to-Agent，A2A）通信模式協作。
學習者將通過整合外部工具和數據源，使用模型上下文協議（MCP）擴展代理能力，連接Azure
AI搜索進行知識檢索，並使用Freshdesk等外部API進行工單管理。培訓進展到將代理部署到Microsoft
Foundry Agent
Service中，作為持久的雲託管解決方案，具備狀態管理和企業級可靠性。最後，參與者將實施先進的工作流程模式，包括集中協調的多代理系統和基於交接的系統，使對話能夠根據用戶意圖和領域專業知識在專業代理之間無縫切換。

**目標**

到這個實驗結束時，你就能:

- **搭建AI項目並從VS Code完成聊天:** 通過創建 Microsoft Foundry
  項目、部署 GPT-4 和嵌入模型，以及通過 Visual Studio Code
  建立安全連接，配置一個生產準備的 AI
  開發環境。您將通過執行聊天完成調用來驗證設置，確保本地開發環境與Azure
  AI服務之間的無縫集成，並進行正確的身份驗證和項目配置。

- **構建健康保險計劃分析AI代理:** 開發一個專注於分析和可視化健康保險數據的智能
  AI
  代理。您將創建一個代理，處理複雜的健康福利計劃信息並自動生成對比條形圖，展示核心AI代理能力，包括數據解釋、自然語言理解、代碼執行和自動可視化生成，以支持決策。

- **開發多代理協作系統:** 設計並實現先進的多智能體架構，使專業AI智能體協同分析健康計劃文檔並生成全面報告。你將構建一個用於使用
  Azure AI Search
  智能文檔檢索的搜索代理，一個用於生成詳細分析報告的報表代理，一個確保合規和準確性的驗證代理，以及一個用於管理代理間通信和工作流協調的編排代理代理，展示企業級代理協作模式。

**前提條件**

參與者應當有:

- **Azure 和雲經驗**- 熟悉 Azure Portal、Resource Groups 和 Azure AI
  services

- **編程技能——**基礎Python知識（異步/等待、環境變量、API調用）

- **AI概念**——理解大型語言模型、嵌入、RAG（檢索增強生成）和提示工程

- **開發工具**——熟練掌握Visual Studio Code、終端使用和Git。

- **代理框架意識——**對代理架構、工具和編排模式的基礎知識

組件說明

- **Microsoft** **Foundry**：Microsoft Foundry
  是一個用於開發、部署和管理企業級 AI
  代理的雲平臺。它提供託管代理服務運行時、集中項目管理和應用洞察監控，確保整個代理生命週期內企業級的可靠性、安全性和可觀察性。

- **Microsoft Agent Framework SDK**：官方 Python
  SDK，用於構建智能、模塊化代理，取代 AutoGen
  和語義內核。它具備原生代理間通信、模型上下文協議集成以及 Microsoft
  Foundry 支持，使得生產準備的企業代理系統能夠標準化使用工具。

- **Azure AI
  搜索**：基於矢量的搜索引擎，支持檢索增強生成工作流程。它提供結合向量相似度與關鍵詞搜索的混合檢索，提升相關性所需的語義排序，以及文檔索引功能，確保代理從企業知識源中提供紮實、事實準確的回答。

- **Model Context
  Protocol（MCP）：**一種標準化接口，使代理能夠安全地訪問外部知識和工具。MCP連接企業數據源、Freshdesk等外部API，以及帶有結構化結構的定制工具，確保交互可靠且可審計，並為可擴展的企業AI系統奠定基礎。

- **聊天響應代理**：一種單回合、無狀態代理模型，用於本地開發和測試。它獨立處理請求，無需保留上下文，在本地環境中運行並立即響應。非常適合在使用持久代理進入生產環境前，進行核心邏輯原型設計和驗證行為。

- **持久代理**：Microsoft Foundry
  中的一項雲託管、長壽命服務，能在對話間保持狀態。它支持通過MCP、代理間協作和企業級可靠性實現外部工具集成，並內置監控功能，為需要有狀態、多回合對話體驗的生產應用奠定基礎。

- **規劃代理**：一個智能編排器，分析用戶查詢並將其路由到合適的專業代理。它利用
  AI
  推理和關鍵詞啟發式，對人力資源、財務或合規等領域的查詢進行分類，確保任務分配最優，並作為核心協調點。

- **員工代理**：在人力資源、財務或合規等特定領域擁有專業知識的領域專家。每個代理都有領域特定的指令、專業工具和相關知識來源。他們通過A2A溝通與規劃代理人協作，為複雜的領域特定問題提供權威且準確的回答。

- **Azure
  OpenAI**：企業級服務，通過安全的API端點訪問高級LLM。它提供聊天完成、嵌入模型、內容過濾和合規功能。它與
  Microsoft Foundry
  無縫集成，使代理能夠在維護數據隱私和治理控制的同時利用 GPT-4。

# 實驗5：使用Microsoft Foundry構建檢索增強型AI代理

**概述**

在這個實驗室中，你將使用 Microsoft Foundry 門戶創建你的第一個 AI
代理。您將首先上傳企業政策文檔並將其導入Azure AI
Search，以準備知識庫。然後，你將使用Microsoft代理框架配置代理，以啟用檢索增強生成（RAG）。最後，你將測試代理的響應並分析執行日誌，觀察其如何檢索和處理信息。

**實驗室目標**

你將在實驗室執行以下任務。

- 任務1：創建Azure資源

- 任務二：在 Microsoft Foundry 中創建 AI 代理

- 任務3：連接Azure AI搜索RAG

- 任務4：測試並觀察代理執行日誌

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

5.  選擇 **Upload**，選擇 **Create new** 容器以創建新容器。命名為
    +++**datasets**+++，然後選擇 **Ok**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

![A screenshot of a login box AI-generated content may be
incorrect.](./media/image7.png)

6.  選擇“**Browse for files**”，從**C:\Labfiles\Day
    2**中選擇策略文件，點擊**Upload**。

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

2.  從左側窗格選擇**Foundry**，然後選擇 **Create** Foundry資源。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

3.  輸入以下信息，選擇 **Review + create**。

- 名稱 – <+++agentic-@lab.LabInstance.Id>+++

- 默認項目名稱 – <+++agentic-ai-project-@lab.LabInstance.Id>+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.png)

4.  驗證後選擇 **Create**。

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

7.  在 Microsoft Foundry 中，從左側面板選擇 Models + endpoints。選擇 +
    **Deploy model** -\> **Deploy base model**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.png)

8.  搜索 +++gpt-4o-mini+++，選擇並點擊確認以部署該模型。

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image18.png)

9.  在部署窗口中選擇**Deploy**。

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

5.  選擇 **Create New** -\>，輸入名稱為
    <+++agent-insights-@lab.LabInstance.Id>+++，然後選擇**Create**。

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

2.  從左側窗格選擇“**AI Search**”，然後選擇 **+ Create**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

3.  輸入以下信息，選擇 **Review + create。**

- 服務名稱 - +++ai-knowledge-@lab.LabInstance.Id+++

- 地區 - East US2

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image32.png)

4.  驗證通過後選擇 **Create**。創建資源後選擇“Go to resource”。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image34.png)

5.  選擇 **Import data (new)**。

![A screenshot of a search engine AI-generated content may be
incorrect.](./media/image35.png)

6.  在“**Choose data source**”下選擇 **Azure Blob Storage**。

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

8.  在 **Storage account** 和 **datasets** **under Blob
    containe**r下選擇
    <aistorage@lab.LabInstance.Id>，然後選擇“**Next**”。

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

11. 在“**Review and create**”屏幕中選擇“**Create**”。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.png)

12. 在“Create succeeded”對話框中選擇“**Close**”。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.png)

你已經成功將數據集導入 Azure AI
搜索並創建了可搜索索引。在下一個任務中，你將創建一個 AI
代理，並將該索引作為其知識來源連接起來。

## 任務二：在 Microsoft Foundry 中創建 AI 代理

在這項任務中，你將在 Microsoft Foundry 中創建一個新的 AI 代理，並通過
Microsoft 代理框架界面配置其核心目的、指令和模型。

1.  回到你的資源組，從資源列表中選擇**agentic-foundry**資源。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.png)

2.  在下一頁，點擊“**Go to Foundry
    portal**”。現在，您將被引導到Microsoft
    Foundry門戶，在那裡創建您的第一個代理。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.png)

3.  進入Foundry門戶後，從左側菜單選擇 **Agents
    (1)**，你會看到**已經預創建**的代理。如果沒有創建，請點擊**+ New
    agent (2)** ”選項以創建該代理。 

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image46.png)

4.  選擇新創建的**代理**，右側會打開一個配置面板。請提供以下詳細信息。

[TABLE]

> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image47.png)

5.  你已經成功在 Microsoft Foundry
    中創建了一個代理。接下來，是時候通過連接你索引數據來豐富它。

## 任務3：連接Azure AI搜索RAG

在此任務中，您將通過知識集成面板將Azure AI
Search與代理集成，通過MCP（模型上下文協議）實現檢索增強響應。

1.  在同一個代理配置窗格中，向下滾動並單擊“**+ Add** **知識**參數”。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image48.png)

2.  在 **Add knowledge** 面板中，選擇 **Azure AI
    Search**，因為你已經在AI搜索資源中準備好了索引。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image49.png)

3.  在下一窗格中，選擇 **Azure AI Search resource
    connection** 選項，點擊**下拉箭頭（1）**，然後選擇 **Connect other
    Azure AI Search resource (2)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.png)

4.  在下一欄，確認選中了正確的AI搜索資源，並點擊**“Add connection**”。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image51.png)

5.  在 **Adding Azure AI Search** 步驟中，配置以下細節，完成後點擊
    **Connect (5)**。

[TABLE]

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image52.png)

6.  該代理現已通過Azure
    AI搜索索引成功豐富知識，該索引作為可搜索的知識庫，用於在對話中獲取準確信息。

## 任務4：測試並觀察代理執行日誌

在此任務中，您將通過提出與政策相關的問題和結構化日誌來測試您的客服，以驗證工具使用情況、搜索調用和基於實際的響應。

1.  在測試代理之前，連接 Application
    Insights，以啟用詳細日誌和跟蹤可視化。

2.  在 Microsoft Foundry 門戶中，從左側菜單選擇 **Monitoring
    (1)** ，選擇 **agent-insights- (2)** ，點擊 **Connect (3)**

![](./media/image53.png)

3.  完成後，從左側菜單選擇 **Agents (1)** ，然後選擇
    **EnterpriseAssistant（2）**代理，點擊 **Try in playground (3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image54.png)

4.  會打開一個聊天面板，你可以在那裡輸入提示。客服現在會根據你連接的文檔和數據集進行響應。

示例提示 -

- +++What is the employee travel reimbursement policy?+++

- +++Summarize the contract approval rules and cite the document.+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image55.png)

5.  代理回答問題後，從頂部菜單點擊“**Thread
    logs** ”，查看當前線程的日誌和痕跡。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image56.png)

6.  請查看這些指標、記錄和評估，這些指標在代理日誌中展示了詳細的主張。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image57.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image58.png)

7.  現在，進入**監控**窗格，你之前已連接過應用洞察，選擇 **Resource
    usage** 標簽，查看所有指標和數值。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image59.png)

8.  你成功構建了一個基於RAG的代理，由精心策劃的企業數據集驅動。接下來，你將進一步實現多代理協作，讓代理能夠委託、推理並智能協作。

摘要

在這個實驗室裡，你成功地在Microsoft
Foundry創建了你的第一個AI代理，並將其連接到一個索引知識庫。你上傳了文檔，導入了
Azure AI 搜索，並通過 Microsoft Agent Framework 集成啟用了
RAG。通過測試代理並審查執行日誌，你親身體驗了代理如何獲取有根據的信息並生成企業級響應。
