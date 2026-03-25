# 實驗11：AgentOps – 可觀測性與管理

**預計時長**：60分鐘

**概述**

在本實驗室中，你將專注於AgentOps，即在生產環境中監控、管理和管理AI代理的學科。您將探索如何利用
Microsoft Agent Framework 內置的 Application Insights 集成，通過
**OpenTelemetry** 實現可觀測性和遙測。

關於 Microsoft Agent Framework 中的 OpenTelemetry

Microsoft Agent
Framework原生集成了OpenTelemetry，OpenTelemetry是分布式追蹤、指標和日誌的開放標準。它通過自動捕獲遙測數據（如跨度追蹤、工具調用、模型響應和工作流性能）提供端到端的代理行為可視化。通過這種集成，開發者可以直接將可觀測性數據導出到
Azure Monitor、Application Insights 或其他兼容 OpenTelemetry
的後端。這種標準化方法有助於跟蹤複雜多智能體系統中的每一個代理作，實現性能調優、故障排除和合規審計，且配置極簡。

實驗室目標

你將在實驗室執行以下任務。

- 任務1：啟用OpenTelemetry的代理可觀測性

- 任務2：可視化代理指標

- 任務3：監控Foundry門戶中的代理特定指標

## 任務1：啟用OpenTelemetry的代理可觀測性

在這個任務中，你將把OpenTelemetry和Agent
Framework的可觀測性集成到你的項目中。你將配置遙測導出器，使用setup_observability（）初始化追蹤，並捕捉工作流程各階段的詳細數據，包括代理路由、Azure
AI
搜索檢索和工單創建。這使得通過應用洞察中的跟蹤ID實現對代理行為和跨系統關聯的統一可視化。

1.  你不會再修改之前的代碼，而是在一個已經包含已更新可觀察性文件的新文件夾中工作。瞭解如何通過Microsoft代理框架的可觀察性和應用洞察集成遙測、追蹤和監控。

2.  在Visual Studio
    Code中，打開新文件夾前，先選擇.env文件並複製內容，並安全地保存在記事本中。

3.  完成後，點擊頂部菜單中的** file **選項，選擇**“Open Folder**”。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image1.png)

4.  在打開文件夾的窗格中，進入C：\telemetry-codefiles，點擊select文件夾。

5.  打開後，資源管理器菜單裡的文件看起來和這個很像。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

6.  請仔細查看代碼文件，查看所有代理中OpenTelemetry的實現情況，以及追蹤是如何進行的。

> **集成概述**
>
> 通過 agent_framework.observability 包，集成了整個代理工作流程的
> OpenTelemetry 追蹤。

- 導入了get_tracer（）並使用OpenTelemetry跨度捕獲每個關鍵作的結構化遙測數據。

- 將關鍵函數（如分類、路由、RAG、工單創建）包裹在帶有上下文屬性的範圍內。

- 增加了統一的啟動可觀察性設置，使用setup_observability（）來配置導出器和指標流水線。

- 記錄自定義屬性，如查詢文本、路由決策和備用方法，以實現更深層次的可視化。

- 增強錯誤處理功能，記錄異常痕跡，並將每個工作流執行關聯到軌跡ID，實現跨系統關聯。

> **文件增強**
>
> main.py – 端到端追蹤與指標

- 配置了OpenTelemetry追蹤流水線和導出器設置。

- 跨區內的多代理編排包包，實現完整的工作流程可視化。

- 新增了子步驟的跨度：路由、數據檢索（RAG）、代理響應和工單創建。

> planner_agent.py – 增強的路由可觀測性

- 新增了一個追蹤實例（get_tracer（））用於監控分類邏輯。

- 捕捉了原始的LLM響應、信心評分和備用關鍵詞指標作為跨度屬性。

- 區分基於 AI 和帶標簽跨度的啟發式分類（SpanKind.INTERNAL）。

> azure_search_tool.py – RAG 可觀測性

- 增加了用於 Azure Search API 調用的跨度，以測量延遲和成功率。

- 記錄檢索的文檔數量和有效載荷大小作為自定義指標。

- 在OpenTelemetry追蹤中捕獲搜索錯誤和性能數據。

> freshdesk_tool.py – 工單創建可觀察性

- 新增了API調用範圍，用於跟蹤工單創建時間和響應狀態。

- 記錄工單ID、標簽和請求者詳情，以便可追溯審計日誌。

- 監控外部API延遲和錯誤響應，以更好地跟蹤事件。

7.  審核完成後，右鍵點擊 **.env.example （1）** 文件，選擇 **Rename
    (2)** 以重命名該文件。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

8.  完成後，將文件重命名為 **.env.example** --\>
    **.env**，使該環境文件為該代理激活。

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image4.png)

9.  現在，選擇 .env 文件，粘貼你之前複製的內容。

10. 在 Azure 門戶中，導航到 **agenticai** 資源組，從資源列表中選擇
    **ai-knowledge-** Search Service。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

11. 在設置中左側菜單選擇 **Keys (1)** ，然後使用複製選項複製 **Query key
    (2)** 。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

12. 複製完成後，安全地粘貼到記事本，在搜索管理的左側菜單中選擇
    **Indexes**，複製 **Index Name (2)**。 

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

13. 在Visual Studio
    Code面板中，選擇**.env**文件，因為你需要添加AI搜索鍵才能連接。

> \# Azure AI Search (MCP)
>
> AZURE_SEARCH_ENDPOINT=https://ai-knowledge--@lab.LabInstance.Id.search.windows.net/
>
> AZURE_SEARCH_API_KEY=\[Query_Key\]
>
> AZURE_SEARCH_INDEX=\[Index_Name\]

**注意：**請用之前複製的值替換Query_Key和Index_Name值。

14. 將.env文件的內容與以下內容添加。

> AZURE_OPENAI_ENDPOINT=https://agentic-
> @lab.LabInstance.Id.cognitiveservices.azure.com/
>
> AZURE_OPENAI_API_KEY=\<Replace with Azure OpenAI key\>
>
> AZURE_OPENAI_RESPONSES_DEPLOYMENT_NAME=gpt-4o-mini
>
> AZURE_OPENAI_API_VERSION=2025-03-01-preview

15. 將以下 Foundry 項目密鑰變量添加到 .env 文件中。

> \# Azure AI Project Configuration
>
> AZURE_AI_PROJECT_ENDPOINT=**\<Microsoft Foundry endpoint\>**
>
> AZURE_AI_MODEL_DEPLOYMENT_NAME=gpt-4o-mini
>
> 從概覽頁面找到Microsoft Foundry項目端點，並用該值替換 **\<Microsoft
> Foundry endpoint\>**。
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image8.png)

![](./media/image9.png)

16. 完成後，將以下 App Insights 變量添加到同一個文件中。

> \# Observability and Monitoring Configuration
>
> APPLICATIONINSIGHTS_CONNECTION_STRING=**\<Connection string\>**
>
> ENABLE_OTEL=true
>
> ENABLE_SENSITIVE_DATA=true
>
> 從Azure門戶打開應用洞察資源，複製連接字符串，並將**\<Connection
> string\>**替換為複製的值。
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image10.png)

17. 在.env文件中，添加以下內容，並添加你之前複製的Freshdesk的API密鑰和賬戶URL。

> \# Freshdesk Configuration
>
> FRESHDESK_DOMAIN=\[Domain_URL\]
>
> FRESHDESK_API_KEY=\[API_Key\]

18. 最終的.env文件應該看起來像給的圖片。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

19. 完成後，選擇 **File** **(1)** ，然後點擊 **Save** **(2)** 保存文件。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image12.png)

20. 選擇......**（1）**頂部菜單中的擴展菜單選項。選擇 **Terminal
    (2)** ，然後點擊 **New Terminal (3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

21. 在**VS Code** Terminal中，運行Azure CLI登錄命令:

+++az login+++

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image14.png)

22. 在 **Sign in** 窗口中，選擇 **Work or school account** 並點擊
    **Continue**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.png)

23. 在**“Sign into Microsoft** ” 標簽頁，使用以下憑證登錄。

- 用戶名 - <+++@lab.CloudPortalCredential(User1).Username>+++

- TAP - +++@lab.CloudPortalCredential(User1).TAP+++

24. 當被提示登錄選項時，選擇**“No, this app
    only** ”，這樣可以繼續，不鏈接其他桌面應用。

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image16.png)

25. 輸入**1**，然後在“**Select a subscription and tenant**”中回車。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image17.png)

26. 終端打開後，執行命令，

> +++pip install -r requirements.txt+++ 以安裝所有必需的包。

27. 請執行以下命令來測試搜索工具的工作原理。

+++python main.py+++

> ![A screenshot of a computer screen AI-generated content may be
> incorrect.](./media/image18.png)

## 任務2：可視化代理指標

在這個任務中，你將使用 Azure Application Insights
來可視化代理遙測數據。你將探索響應時間、路由準確性和工單創建成功的自定義指標。然後，你將構建交互式Azure
Monitor儀錶盤，以顯示關鍵績效指標和趨勢。這有助於識別瓶頸、衡量效率，並確保部署代理的實時健康運行。

1.  進入 Azure 門戶，打開資源組，從資源列表中選擇
    **agent-insights- **應用洞察資源。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.png)

2.  進入概覽頁面後，你可以看到顯示的一些默認指標。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

3.  在左側菜單中，選擇 **Search (1)**，點擊 **See all data in last 24
    hours (2)**。

![A screenshot of a search engine AI-generated content may be
incorrect.](./media/image21.png)

4.  打開後，從底部開始查看 **Traces (1)**，然後點擊“**View as individual
    items (2)**”。 

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

5.  完成後，你將可以看到與經紀人的所有溝通細節，以及在指定時間範圍內發生的所有交易。你也可以調整時間範圍，探索更多。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

6.  探索和回顧這些翻譯，你只需點擊它們即可打開詳細視圖。查看如何查看所有細節，比如代理、消息和檢索信息。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

7.  接下來，選擇 **Failures (1)**，審查失敗 **requests
    (2)** ，以集中視圖查看所有失敗執行，並通過詳細的跟蹤分析找出根本原因。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

8.  接下來，選擇 **Performance (1)** ，檢查
    **作和響應時間（2），**由此可以確定代理的性能SLA。 

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

9.  現在，在左側菜單的監控中選擇**“Metrics**”。你可以探索通過 SPAN
    發佈的自定義指標。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.png)

10. 選中後，在 **Metric Namespace （1）** 下，選擇
    azure.applicationinsights **（2）。**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.png)

11. 現在，在指標中，選擇 **gen_ai.client.operation.duration and set the
    aggregation to avg (1)**。查看 **line chart (2)** 以查看 **Response
    Time** 指標，代理回復用戶時採用了哪個指標。

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image30.png)

12. 同樣地，選擇 **gen_ai.client.token.usage and set the aggregation to
    avg (1)**。查看 **line chart (2)** ，查看代理的代幣使用情況。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

13. 接著，從左側菜單選擇 **Logs (1)** ，取消 **Queries hub (2)** 面板。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image32.png)

14. 關閉後，點擊 **tables** 選項，將鼠標懸停在 **customMetrics**
    參數上，你會看到一個**Run** 選項，點擊它。 

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

![A close-up of a message AI-generated content may be
incorrect.](./media/image34.png)

15. 查詢成功運行後，你會看到下面列出的所有自定義指標作為查詢結果。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image35.png)

16. 接下來，從左側菜單選擇“**Workbooks (1)** ”，點擊快速開始下的“**Empty
    (2)** 工作簿”。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.png)

17. 打開後，點擊 **+ Add (1)** ，然後選擇 **Add metric (2)**。

![A screenshot of a phone AI-generated content may be
incorrect.](./media/image37.png)

18. 打開公制面板後，點擊**“Add metric”**選項。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

19. 現在，選擇**Metric** 為 gen_ai.client.token.usage**（1）**，將
    **Display name** 作為令牌使用量**（2）**，點擊 **Save (3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.png)

20. 再次點擊 **Add metric** 選項。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

21. 現在，選擇 **Metric** 為 gen_ai.client.operation.duration
    **（1）**，將 **Display name**設置為響應時間**（2）**，點擊 **Save
    (3)**。

![A screenshot of a screenshot of a metric settings AI-generated content
may be incorrect.](./media/image40.png)

22. 選中這兩個指標後，點擊“**Run Metrics**”。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.png)

23. 現在把**可視化**改成**面積圖**，獲得類似的可視化效果。你可以探索許多其他可視化方式，以及時間範圍。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.png)

24. 編輯完成後，點擊**“Done
    editing**”。這樣可以把這張卡保存到你的練習冊裡。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.png)

25. 現在，再次點擊 **+ Add (1)** ，然後選擇 **Add query (2)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.png)

26. 在查詢窗格中，添加以下 **query (1)**，並點擊 **Run Query (2)**。

+++customMetrics+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.png)

27. 查詢成功運行後查看結果。審核完成後，點擊 **Done Editing**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image46.png)

28. 完成後，點擊頂部菜單中的“**Done editing (1)** ”，然後點擊“**Save
    (2)**”圖標。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image48.png)

29. 在“Save As”面板上，輸入“標題為代理工作簿**（1）**，然後點擊 **Save
    As (2)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image49.png)

30. 由於這是實驗室環境，可用數據可能有限，難以進行全面監測。不過，你可以通過添加客服的自定義指標，並創建專門針對特定目標的監控儀錶盤來提升可見性，例如以下內容:

- **代理性能儀錶盤**

> **顯示的指標:**

- 代理響應時間（平均，P95）

- 按代理類型劃分的成功率

- 請求量趨勢

- 錯誤率警報

> **商業問題解答:**

- 哪些代理表現最好？

- 我們是否達成了SLA目標？

- 是什麼導致了系統變慢？

&nbsp;

- **用戶體驗儀錶盤**

> **顯示的指標:**

- 端到端請求延遲

- 工單生成率

- 知識檢索成功

- 用戶滿意度代理指標

> **商業問題解答:**

- 用戶回復是否很快？

- 請求多久會變成支持工單？

- 知識庫對用戶有幫助嗎？

## 任務3：監控Foundry門戶中的代理特定指標

在這個任務中，你將使用 Azure Application Insights
來可視化代理遙測數據。你將探索來自 Microsoft Foundry
門戶的定制代理特定指標。

1.  既然你已經將 Application Insights 連接到 Microsoft Foundry
    門戶，你可以返回 Foundry 門戶，直觀地可視化代理的工作過程。

2.  從資源列表中返回你的資源組，選擇**agent-foundry**資源。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.png)

3.  在下一頁，點擊**“Go to Foundry
    portal**”。現在，您將被引導到Microsoft
    Foundry門戶，在那裡創建您的第一個代理。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image51.png)

4.  在測試代理之前，連接 Application
    Insights，以啟用詳細日誌和跟蹤可視化。

5.  在 Microsoft Foundry 門戶中，從左側菜單選擇 **Monitoring
    (1)** ，選擇 **agent-insights- (2)** ，點擊 **Connect (3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image52.png)

6.  現在，進入你之前連接過應用洞察的 **Monitoring** 面板，選擇
    **Resource usage** 標簽，查看所有指標和數值。 

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image53.png)

7.  從左側菜單選擇 **Tracing (1)** ，點擊任一的 **Trace
    (2)**，查看代理互動的詳細跟蹤。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image54.png)

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image55.png)

**摘要**

在這個實驗室裡，你為企業代理配置了可觀察性和監控功能。通過OpenTelemetry追蹤，你捕捉了每個工作流步驟的詳細執行數據，並通過與Azure應用洞察集成，創建了用於可視化性能指標和座席健康狀況的儀錶盤。

你已經成功完成了這個實驗。請點擊“Next \>\>”繼續。
