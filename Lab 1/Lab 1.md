# 構建與擴展智能代理

**概述**

這個動手實驗室介紹利用 Azure AI 服務和 Microsoft 365 Copilot 構建智能 AI
代理。參與者將學習如何利用Copilot進行人力資源工作流程，搭建Microsoft
Foundry項目，構建簡單的AI代理，創建RAG（檢索增強生成）代理，並開發具備編排能力的多代理系統。

**目標**

到這個實驗結束時，你就能:

- **使用 Copilot Studio 構建人力資源助理代理**——使用 Microsoft 365
  Copilot 自動化員工招聘、篩選、培訓材料開發、反饋收集和績效評估。

- **搭建AI項目並完成聊天完成**——在Microsoft
  Foundry中配置AI項目，部署大型語言模型（LLM）和嵌入模型，並建立VS代碼連接以完成聊天。

- **構建健康保險計劃分析器 AI 代理** - 創建 AI 代理，利用 Azure AI
  服務處理數據並生成可視化（例如比較健康福利計劃的條形圖）。

- **開發健康計劃報告生成多代理系統**——設計和實施協調的多代理系統，由專業代理（搜索代理、報告代理、驗證代理和編排代理）協同完成複雜任務。

**前提條件**

參與者應當有:

- **Visual Studio Code（VS Code）：**熟練使用VS
  Code進行編碼、調試和管理各種編程語言和框架的擴展。

- **開發技能：具備**Python或JavaScript的基礎編程知識，API、SDK的使用經驗，以及Visual
  Studio Code的作。

- **命令行/終端**：熟悉運行PowerShell命令和管理虛擬環境。

**組件說明**

- **Azure AI Search**：基於矢量的搜索服務，通過索引和檢索相關文檔實現
  RAG。

- **Azure OpenAI 服務**：通過 Azure 的企業基礎設施提供對 GPT-4o
  和嵌入模型的訪問。

- **大型語言模型（LLM）：**用於文本理解和生成的高級 AI 模型，如GPT-4o。

- **嵌入模型**：將文本轉換為語義搜索和檢索的向量表示（例如，文本嵌入-3-large）。

- **Microsoft 365
  Copilot**：基於AI的文檔分析和工作流程自動化生產力工具。

- **Semantic Kernel**: 用於將LLM與編程語言集成並構建編排能力的SDK。

# 實驗1：使用Copilot Studio構建人力資源助理代理

預計時長：30分鐘

概述

在本實驗室中，你將專注於通過使用 Microsoft 365 Copilot 和 Copilot Studio
簡化和改進組織內員工的過渡和入職流程。你將學習如何識別合適候選人，制定定制化的過渡和入職計劃，生成有效的溝通和培訓材料，自動化人力資源工作流程，收集反饋，並建立績效監控和評估機制。通過利用這些AI驅動的工具，本實驗室展示了組織如何確保順利高效的過渡流程，提升內部流動性，並支持員工成功適應新崗位。

實驗室目標

你將在實驗室執行以下任務。

- 任務1：快速篩選候選人

- 任務2：開發培訓材料

- 任務3：收集反饋

- 任務4：績效評估

架構圖

![image](./media/image1.png)

## 任務一：快速篩選候選人

在這項任務中，你將使用Microsoft 365
Copilot快速評估大量數據分析師職位的申請，並根據相關經驗、技術技能和教育背景等具體標準篩選候選人，從而突出顯示最佳候選人供進一步審查。

1.  在 Edge 瀏覽器中添加一個新標簽頁，使用以下鏈接打開 Microsoft 365
    Copilot 應用，點擊 **Sign in (2)。**

+++https://m365.cloud.microsoft/+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

2.  在“**Sign into Microsoft Azure
    tab**”中，你會看到一個登錄界面。請使用以下憑證登錄。

- Username - +++@lab.CloudPortalCredential(User1).Username+++

- TAP - +++@lab.CloudPortalCredential(User1).TAP+++

3.  如果你看到彈窗“**Welcome to your Microsoft 365 Copilot
    app**，請點擊” **Get started** “。

![A screenshot of a computer application AI-generated content may be
incorrect.](./media/image3.png)

4.  在左側窗格選擇 **Apps** **(1)**，然後從應用部分點擊
    **OneDrive（2）。**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

**注意**：如果你看到彈窗“**Welcome to Apps”**，請點擊 **X** 關閉彈窗。

![A screenshot of a computer application AI-generated content may be
incorrect.](./media/image5.png)

5.  進入“**My files**”，然後點擊 **+ Create or upload (1)** 按鈕，選擇
    **Folder upload (2)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

6.  進入C：\LabFiles\Day-1\data **（1）**，點擊CV **（2）**文件夾，選擇
    **Upload (3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

7.  在“Upload 5 files to this site?”中選擇 **Upload**  彈出窗口。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

8.  再次點擊**+ Create or upload (1)** ，然後選擇 **Folder upload
    (2)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

9.  進入C：\LabFiles\Day-1**（1）**，點擊數據**（2）**文件，點擊
    **Upload 3**。 

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.png)

10. 選擇上傳“**Upload** ”，在“Upload 19 files to this site?” 彈出窗口。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.png)

11. 從左側面板返回 **M365 Copilot**，選擇
    **Apps** **(1)**，然後從應用部分點擊 **Copilot** **(2)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

12. 從左側面板進入 **Copilot**，點擊 **Chat
    (1)**。然後點擊聊天面板底部的**+ (Add)** 圖標**（2）**，選擇
    **Upload images and files (3)**。

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image12.png)

13. 在文件資源管理器彈窗中，進入 C：\LabFiles\Day-1\data\CV
    **（1）**文件夾，選擇 **first 3** **(2)**  個文件，點擊
    **Open** **(3)**。

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image13.png)

14. 在 **Copilot chat** 中，**三個文件**上傳成功後，點擊 **enter**。 

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

15. 在活躍的 Copilot chat 中，點擊消息框下方的 **+ (Add) (1)** 
    圖標，然後選擇 **Upload images and files (2)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.png)

16. 在文件資源管理器彈窗中，進入C：\LabFiles\Day-1\Data\CV
    **（1）**文件夾，選擇 **最後2（2）**個文件，點擊 **Open** **(3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image16.png)

17. 在 **Copilot chat** 中，**兩個文件（1）**成功上傳後，點擊 **enter
    (2)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.png)

18. 在聊天框中，輸入以下提示**（1），**並點擊 **Sent (2) **按鈕:

> Microsoft 365
> Copilot，請幫我篩選和篩選數據分析師候選人，基於SQL、Python和數據可視化工具等必要資質篩選和篩選簡歷。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.png)

19. 跟進下面的提示，點擊 **Sent **按鈕

> 製作一份頂級數據分析師候選人的總結報告，包括他們的技能、工作經驗和教育背景。
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

**成果**：人力資源團隊高效篩選最合格的候選人，節省時間並確保招聘工作有針對性。

## 任務2：開發培訓材料。

在這項任務中，你將利用Microsoft
Copilot為新員工準備全面的培訓材料，創建個性化的入職內容，包括針對崗位的指南、公司政策以及所用工具和技術概述，確保培訓材料詳盡、結構合理，並針對員工的角色量身定制。

1.  在聊天框中，輸入以下提示**（1），**並點擊 **Sent (2) **按鈕:

> 為新數據分析師制定全面的入職培訓計劃，包括公司政策、數據工具培訓和團隊介紹等主題。
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image20.png) ![A screenshot of a web page
> AI-generated content may be incorrect.](./media/image21.png)

2.  接下來是下面的提示（**1），**然後點擊 **Sent (2)** 按鈕。

> 製作一個互動式培訓演示，涵蓋數據分析最佳實踐和關鍵績效指標，並生成可下載的PPT。
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image22.png)

**注意**：執行此提示後，您將下載一份PowerPoint演示文稿，然後您可以編輯或設計。如果文件未被下載，請嘗試找到帶有演示標題的超鏈接，如截圖所示。

**注意**：執行此提示後，“待下載的PowerPoint演示文稿”選項未顯示。請重演上述提示。

結果：新員工會獲得組織良好的培訓材料，幫助他們快速上手並高效完成職責。

## 任務3：收集反饋

在此任務中，你將利用Microsoft
Copilot生成和分發反饋調查，收集和分析反饋，深入瞭解招聘和入職流程的優勢以及需要改進的領域。

1.  在聊天框中，輸入以下提示並點擊**Sent **按鈕 :

> 為面試官創建一個反饋表，基於技術能力、問題解決能力和文化契合度評估數據分析師候選人。生成可下載的Word或PDF版本。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

2.  跟著下面的提示，點擊 **Sent **按鈕。

> 向新員工發送調查問卷，收集入職體驗反饋並找出改進空間。生成可下載的Word或PDF版本的問卷。
>
> ![A screenshot of a survey AI-generated content may be
> incorrect.](./media/image23.png)
>
> 結果：人力資源部門獲得了寶貴反饋，幫助他們完善招聘和入職流程，確保未來員工獲得更好的體驗。

## 任務4：績效評估

在這項任務中，你將定期進行績效評估，通過使用 Microsoft Copilot
創建績效評估模板、安排評估會議、跟蹤成就、收集同事反饋以及編制結構化績效報告，評估新員工的進展和發展。

1.  在聊天框中，輸入以下提示並點擊 **Sent **按鈕 :

> 為新數據分析師制定績效評估計劃，包含季度評估和目標設定會議，並生成日曆CSV文件。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

2.  跟著下面的提示，點擊 **Sent** 按鈕。

> 生成績效評估報告模板，包括成就、改進領域和未來目標的部分
> 生成績效評估模板。
>
> ![A screenshot of a report AI-generated content may be
> incorrect.](./media/image25.png)
>
> 結果：新員工獲得建設性的反饋和支持，有助於其職業成長，並為公司內的長期成功做出貢獻。
>
> **摘要**
>
> 在本實驗室中，你成功地利用 Microsoft 365 Copilot
> 構建了一個人力資源助理代理，以簡化員工招聘和入職流程。你學會了如何通過分析簡歷快速篩選數據分析師候選人，並基於SQL、Python和數據可視化等技術技能進行篩選，然後為新員工制定了全面的入職培訓計劃和互動演示。你為面試官製作了反饋表，並為新員工製作了調查問卷，以評估和改進招聘流程，並制定了季度績效評估計劃，並用結構化模板跟蹤成就和目標。通過利用
> AI
> 驅動的工具，你展示了組織如何自動化人力資源工作流程，提升效率，並確保新員工順利過渡。
>
> 你已經成功完成了這個實驗。請點擊“Next \>\>”繼續。
