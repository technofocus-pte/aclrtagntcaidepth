# 實驗3：構建健康保險計劃分析器AI代理

**概述**

在本實驗室中，您將構建一個專門用於處理和分析健康保險計劃數據的健康保險計劃分析器AI代理。該智能代理通過解讀計劃細節、分析保障選項並生成支持決策的可視化表示，自動比較不同健康福利計劃。利用Microsoft
Foundry和Azure
AI服務，客服人員將創建對比條形圖，清晰顯示保險計劃之間的差異，幫助用戶更輕鬆地瞭解選項並選擇最合適的健康保險。

**實驗室目標**

你將在實驗室完成以下任務。

- 任務一：創建一個簡單的AI代理

## 任務一：創建一個簡單的AI代理

在此任務中，您將構建一個簡單的AI代理，處理數據並生成條形圖，利用Azure
AI服務比較不同健康福利計劃進行分析和可視化。

1.  打開 **實驗2 - 創建一個簡單的 AI agent.ipynb** 文件。本 **實驗2 -
    創建一個簡單的 AI agent.ipynb**
    筆記本引導你構建一個簡單的AI代理，它能處理數據並生成比較不同健康福利計劃的條形圖。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

2.  選擇右上角可選的**“ Select kernel **”設置。從列表中選擇
    **venv（Python 3.x.x）。**

![A blue and red rectangle with white text AI-generated content may be
incorrect.](./media/image2.png)

3.  運行下面的單元格導入必要的庫並加載環境變量，以便使用 Azure AI
    項目。這種設置實現了與Azure AI服務的安全認證和交互。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image3.png)

4.  運行下面的單元，連接到你的 Microsoft Foundry 項目，訪問已部署的
    **gpt-4o** 模型。這通過項目連接字符串和 Azure 憑證建立安全連接。

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image4.png)

5.  運行該單元格創建一個 **simple AI Agent**，處理數據並生成條形圖，使用
    Microsoft Foundry 比較不同的健康福利計劃。該腳本初始化 AI
    代理，發送包含健康計劃數據的提示，並請求條形圖。代理處理請求，生成圖表，保存圖像文件，然後通過刪除代理進行清理。

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image5.png)

6.  最後觀察輸出。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image6.png)

**摘要**

在本實驗室中，您成功地利用 Microsoft Foundry 和 Azure AI
服務構建了健康保險計劃分析器 AI
代理，實現了健康福利計劃的分析與比較。你學會了如何連接你的Microsoft
Foundry項目，訪問已部署的GPT-4o模型，並創建一個能夠處理複雜健康保險計劃數據的智能代理。代理人解讀計劃細節，分析保障選項，並自動生成比較條形圖，以可視化保險計劃之間的差異。這次親身體驗展示了
AI
代理如何簡化數據分析、支持決策過程，並幫助用戶輕鬆理解和比較健康保險選項。

你已經成功完成了這個實驗。請點擊“Next \>\>”繼續。
