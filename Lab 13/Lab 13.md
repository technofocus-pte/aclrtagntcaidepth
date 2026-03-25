# 實驗13：利用人機環路AI實施企業欺詐檢測

**預計時長**：60分鐘

**概述**

您是Contoso有限公司的AI工程師，負責實施人機參與（HITL）AI工作流程。在本實驗室中，您將探索Contoso欺詐檢測與響應工作流程，AI代理分析可疑活動，並將高風險行為轉交人工分析師審核，同時使用實時React +
FastAPI儀錶盤進行監控和交互。

實驗室目標

你將在實驗室執行以下任務。

- 任務一：利用Azure Agent Framework實現人機參與AI工作流程

## 任務0：設置代碼 

1.  從 C：\Labfiles\Day 3 中，解壓 **OpenAIWorkshop-Framework** 文件。

2.  點擊 LabVM桌面上的 **Visual Studio Code。**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

3.  選擇 **File** **(1)**並點擊 **Open
    Folder** **(2)** 以打開**OpenAIWorkshop-Framework**文件夾。 

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

4.  進入 C：\Labfiles\Day 3\\**OpenAIWorkshop-Framework** 路徑，選擇
    **OpenAIWorkshop-Framework**，然後**選擇Folder**。

5.  選擇**“Yes, I trust the authors。”**

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image3.png)

6.  點擊**省略號（...）（1）**然後是 **Terminal** **(2)** ，然後是 **New
    Terminal** **(3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

7.  輸入以下命令，進入**應用程序**目錄，並從 **pyproject.toml /
    uv.lock** 文件安裝所有必需的依賴。

> cd agentic_ai/applications
>
> uv sync

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

**注意：**如果遇到任何錯誤，請執行以下命令

> +++pip install uv+++

+++uv sync+++

8.  該指令可能需要5到10分鐘完成。 **與此同時，你可以繼續進行任務1**。

## 任務1：利用Azure Agent Framework實現人機參與AI工作流程

在本實驗室中，您將為Contoso的欺詐檢測系統實施人工參與（HITL）工作流程。你將運行多代理欺詐檢測，審核高風險警報，做人工決策，並實時可視化React +
FastAPI儀錶盤的工作流程。

1.  在Visual Studio Code中，展開 **agentic_ai (1) \> workflow (2)\>
    fraud_detection
    (3)**，選擇**fraud_detection_workflow.py（4）。**查看代碼**（5）**。

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image6.png)

2.  在**fraud_detection（1）**下，右鍵點擊**.env.sample（2），**然後選擇
    **Rename (3)。**

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image7.png)

3.  將重命名為 .env，然後點擊它打開文件。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image8.png)

4.  用你在上一個實驗中複製的實際數值替換AZURE_OPENAI_API_KEY**（1）**和AZURE_OPENAI_ENDPOINT**（2）**的數值。

5.  把AZURE_OPENAI_CHAT_DEPLOYMENT加成**gpt-40-mini（3）**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.png)

- 進入 **Microsoft Foundry** 門戶，選擇 **Overview** **(1)**，選擇
  **Azure OpenAI （2）**。複製 **Azure OpenAI key** **(3)**  和 **Azure
  OpenAI endpoint** **(4)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.png)

6.  選擇 **File** **(1)** ，然後選擇 **Save** **(2)**。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

7.  在Visual Studio代碼窗口中，點擊**省略號（...）（1）**然後是
    **Terminal** **(2)** ，然後是 **New Terminal** **(3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

8.  執行以下命令。

> cd mcp
>
> uv run python mcp_service.py

9.  讓命令運行，打開一個新的終端。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

10. 請輸入以下命令，用命令行運行工作流程。

> cd agentic_ai/workflow/fraud_detection
>
> uv run python fraud_detection_workflow.py
>
> ![A black screen with white text AI-generated content may be
> incorrect.](./media/image12.png)

**注意**：該命令可能需要5到10分鐘完成。請等它結束。

11. 示例包含三個示例警報:

    - **警報1：多國登錄**（嚴重程度高）

    - alert_id: "ALERT-001"

    - customer_id: 1

    - alert_type: "multi_country_login"

    - 描述：“美國和俄羅斯在2小時內嘗試登錄。”

嚴重程度：“高”

- **警報2：數據激增**（中等嚴重度）

- alert_id: "ALERT-002"

- customer_id: 2

- alert_type: "data_spike"

- 描述：“過去24小時內數據使用量增加了500%。”

嚴重程度：“中等”

- **警報3：異常指控**（嚴重程度高）

- alert_id: "ALERT-003"

- customer_id: 3

- alert_type: "unusual_charges"

- 描述：“三筆大額購買，總共5000美元，耗時10分鐘。”

嚴重程度：“高”

12. 一旦運行成功，你可以看到下面的終端。根據風險嚴重度選擇行動。如果風險嚴重度≥0.6，則需進行人工審核。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

13. 由於風險嚴重性較高，您可以輸入2來鎖定客戶賬戶**（1）。**

    - 分析師筆記：三項分析均確認高風險。立即行動：鎖定賬戶以防止未經授權訪問。**（2）**

    - 輸入分析師ID（默認：analyst_cli）：按**Enter （3）**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

14. 一旦工作流程完成，你會收到這樣的輸出。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.png)

15. 命令成功後，**刪除所有現有的正在運行的終端會話**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image16.png)

## Contoso欺詐檢測與響應工作流程的實時工作流程可視化界面

您將使用實時工作流程可視化工具界面來監控並與Contoso欺詐檢測與響應工作流程交互。您將啟動所有服務（MCP服務器、後端、前端），選擇示例提醒，觀察實時工作流執行，審查高風險欺詐警報，提交分析師決策，並實時監控事件流。

1.  開一個新航站樓。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

2.  啟動所有服務（3 terminal）:

    - Terminal 1 - MCP服務器:

> cd mcp
>
> uv run mcp_service.py

- Terminal 2 - FastAPI 後端:

> cd agentic_ai/workflow/fraud_detection
>
> uv run --prerelease allow backend.py
>
> ![A screen shot of a computer program AI-generated content may be
> incorrect.](./media/image17.png)

- Terminal 3 - React前端:

> cd agentic_ai/workflow/fraud_detection/ui
>
> npm run dev
>
> **注意**：如果出現任何錯誤，先執行 +++npm install+++
> 命令，然後重新運行 +++npm run dev+++ 命令。
>
> ![A computer screen with white text AI-generated content may be
> incorrect.](./media/image18.png)

- **ctrl + 點擊** http://localhost:3000 在瀏覽器中打開應用

> ![A screen shot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

3.  查看實時工作流程可視化界面。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

4.  您可以通過“**Select Alert**”下拉菜單查看示例警報 。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image21.png)

**注意**：只有在第二個終端（backend.py）連接打開後，你才能通過下拉菜單看到警報。確保連接是開啟的。

5.  **選擇警報**：從3個示例警報中選擇（ALERT-001、ALERT-002、ALERT-003）（**1）**

    - 點擊 **Start Workflow (2**開始處理

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

6.  **觀看實時更新**: 節點在執行器運行時會變色

    - 🔵 藍色 = 運行

    - 🟢 綠色 = 完工

    - ⚪ 灰色 = 閒置

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

7.  **分析師審查**：當發現高風險欺詐時，會出現審查小組。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

8.  **提交決策**：選擇動作並添加備註

    - 你的決定：如果嚴重程度較高，選擇 **Lock Account (1)**

    - 分析師注：輸入“高風險”，三項分析均確認。立即行動：鎖定賬戶以防止未經授權訪問。**（2）**

    - 選擇 **SUBMIT WORKFLOW (3)**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

9.  **監控事件**：右側面板顯示完整的事件流。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

**摘要**

在本實驗室中，你實施了利用Azure Agent
Framework實現了欺詐檢測的人工流程（HITL）。你探討了 AI
代理如何分析可疑活動，將高風險案件轉交給人工分析師，以及如何與實時React +
FastAPI儀錶盤交互以監控工作流程執行和提交決策。
