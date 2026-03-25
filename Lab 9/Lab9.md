# 實驗9：利用Azure AI框架實現單代理和多代理工作流程

**預計時長**：45分鐘

**概述**

您是Contoso有限公司的AI工程師，負責利用Azure
AI框架開發智能代理工作流程。在本實驗室中，你將創建一個單代理系統，通過MCP與外部工具集成，然後設計多代理工作流程，使多個專業代理根據用戶意圖動態協作或交付任務。

實驗室目標

你將在實驗室執行以下任務。

- 任務1：構建並測試一個Azure OpenAI聊天代理

- 任務2：創建帶有工具集成的單代理工作流程

- 任務3：多代理工作流程設計

  - 任務3.1：協調多智能體工作流程

  - 任務3.2：切換模式多智能體系統

## 任務0：實驗室環境搭建

1.  從 C：\Labfiles\Day 2 中，解壓 **OpenAIWorkshop-Framework** 文件。

2.  點擊 LabVM桌面上的**Visual Studio Code** 。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

3.  選擇 **Open Folder** **(2)** 並點擊 **Open
    Folder** **(2)** 以打開**OpenAIWorkshop-Framework**文件夾。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

4.  進入 C：\Labfiles\Day 2\\**OpenAIWorkshop-Framework** 路徑，選擇
    **OpenAIWorkshop-Framework**，然後**選擇文件夾**。

5.  選擇**“Yes, I trust the authors”**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

6.  點擊 **省略號（...）（1）**然後是 **Terminal** **(2)**，然後是 **New
    Terminal** **(3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

7.  輸入以下命令，導航到**應用程序**目錄，**並從requirements.txt**文件安裝所有必需的依賴
    。

> cd agentic_ai/applications
>
> pip install -r requirements.txt

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image5.png)

8.  該指令可能需要5到10分鐘完成。

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image6.png)

## 任務1：構建並測試一個Azure OpenAI聊天代理

在這個任務中，你將用Visual Studio Code構建並測試一個簡單的Azure
OpenAI聊天代理。你將配置環境變量，將代理連接到已部署的模型，並觀察它如何根據不同提示生成動態響應。

1.  返回**Visual** **Studio Code**。

2.  確保 pip install -r requirements.txt
    命令已成功完成。如果還在運行，請等它結束。

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image6.png)

3.  從**Explorer**中展開 **agentic_ai** **(1)
    \>** **applications** **(2)**。右鍵點擊 .env.sample **（3）** 和
    **Rename （4）** 。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

4.  把文件重命名為.env，然後點擊它打開文件。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

5.  將AZURE_OPENAI_API_KEY**（1）**和AZURE_OPENAI_ENDPOINT**（2）**的值替換為實際值。從Microsoft
    Foundry **Overview**頁面獲取。

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image9.png)

6.  把AZURE_OPENAI_CHAT_DEPLOYMENT 加成 **gpt-40-mini（3）**

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image10.png)

7.  選擇 **File (1)**，然後選擇 **Save(2)**。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

8.  右鍵點擊 **application** **(1)** 文件夾，然後點擊 **New
    file** **(2)** 創建新文件，配置一個簡單的代理。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.png)

9.  將代理文件命名為 +++simple_agent_test.py+++。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

10. 複製並粘貼以下代碼到文件中。

> import asyncio
>
> import os
>
> from dotenv import load_dotenv
>
> from agent_framework.azure import AzureOpenAIChatClient
>
> from azure.identity import AzureCliCredential
>
> \# Load .env file (same folder or specify full path)
>
> load_dotenv(dotenv_path=".env")
>
> \# Retrieve values from .env
>
> endpoint = os.getenv("AZURE_OPENAI_ENDPOINT")
>
> deployment_name = os.getenv("AZURE_OPENAI_CHAT_DEPLOYMENT")
>
> api_version = os.getenv("AZURE_OPENAI_API_VERSION")
>
> print("Using Azure OpenAI endpoint:", endpoint)
>
> print("Deployment name:", deployment_name)
>
> print("API version:", api_version)
>
> \# ✅ Correct parameter name is deployment_name (not deployment)
>
> agent = AzureOpenAIChatClient(
>
> api_key=os.getenv("AZURE_OPENAI_API_KEY"),
>
> endpoint=endpoint,
>
> deployment_name=deployment_name,
>
> api_version=api_version
>
> ).create_agent(
>
> instructions="You are a helpful and funny assistant who tells short
> jokes.",
>
> name="Joker"
>
> )
>
> async def main():
>
> result = await agent.run("Tell me a joke about the cloud.")
>
> print("\nAgent response:\n", result.text)
>
> asyncio.run(main())

![A computer screen shot of a program AI-generated content may be
incorrect.](./media/image14.png)

11. 選擇 **File** **(1)** ，然後選擇 **Save** **(2)**。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

12. 右鍵點擊**simple_agent_test.py（1）**，然後選擇 **Open in Integrated
    Terminal** **(2)**中打開。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image15.png)

13. 執行以下命令運行代理並觀察輸出，以瞭解代理的工作原理。

+++python simple_agent_test.py+++

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image16.png)

14. 我們修改指令，觀察代理的反應。將指令提供“Tell me a joke about the
    Earth”
    **(1)** （第31行），然後**Save** 文件。然後執行下面的命令**（2），**並查看代理的響應**（3）。**

+++python simple_agent_test.py+++

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image17.png)

15. 這表明代理的響應會根據所提供的指令而變化，凸顯其適應不同提示的能力。

## 任務2：創建帶有工具集成的單代理工作流程

在此任務中，您將構建並測試一個單代理工作流程，並與使用MCP（模型上下文協議）的外部工具集成。你將配置環境變量，本地運行MCP服務器、後端和前端，並觀察代理如何利用MCP工具處理用戶查詢，提供智能且具上下文感知的響應。

1.  在Visual Studio Code中，展開 **agents** **(1)
    \>** **agent_framework** **(2)
    \>** **single_agent** **(3)** ，並查看集成MCPStreamableHTTPTool工具**（4）**的單代理工作流程。

    - MCPStreamableHTTPTool 允許代理通過 MCP 服務器調用基於 HTTP
      的外部服務，並在對話中包含工具輸出。

    - 傳遞到ChatAgent，並根據指令和用戶提示自動使用。

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image18.png)

2.  仔細閱讀代碼，瞭解它是如何集成的:

    - 在_maybe_create_tools法中:

> ![A screen shot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

- 這會創建一個可流式的 HTTP 工具，連接到你的 MCP 服務器。

- 它允許代理通過MCP向外部服務發送HTTP調用，作為其工作流程的一部分。

&nbsp;

- 工具在初始化時傳遞給ChatAgent:

> ![A screen shot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

- 當用戶提示觸發工具調用時，代理就可以使用該工具。

- WebSocket
  的流媒體支持：當在流式對話中調用工具/功能時，它會通過_chat_async_streaming廣播工具名稱和轉機。

3.  進入 .env 文件（1），在 .env 文件中添加以下 Environment
    變量以指定運行 **Single agent workflow**  **(2)** :

+++AGENT_MODULE=agents.agent_framework.single_agent+++

- 添加 DISABLE_AUTH=true **（3）**
  環境變量，用於禁用應用程序中的認證。它使本地開發和測試更加便捷。

> +++DISABLE_AUTH=true+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

4.  選擇 **File (1)** ，然後選擇 **Save(2)**。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

5.  現在你將啟動**MCP服務器、後端**和**React前端**，在本地運行完整的代理環境，允許UI與代理和工具交互。

6.  在Visual Studio代碼窗口中，點擊**省略號（...）（1），**然後是
    **Terminal (2)**，然後是 **New Terminal (3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

7.  等待上一步完成後，再進行下一步。

8.  **啟動MCP服務器（Terminal 1）**: (mcp directory is at project root
    level)

    - 執行以下命令啟動**MCP服務器**，該服務器會暴露代理可以調用的API，作為工具調用。
      (服務器運行在 [http://localhost:8000](http://localhost:8000/))

> cd mcp
>
> uv run python mcp_service.py
>
> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image21.png)
>
> 注意：如果遇到任何錯誤，請執行以下命令:

+++pip install uv+++

+++uv run python mcp_service.py+++

9.  讓命令運行，打開一個新的 terminal。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

10. **啟動後端（Terminal 2）**:

    - 執行以下命令啟動託管代理工作流、會話管理和API端點的後端服務器。

> cd agentic_ai/applications
>
> uv run python backend.py

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image22.png)

- 本地運行於: [http://localhost:7000](http://localhost:7000/).

- 這是前端通信的核心應用邏輯。確保**連接是開啟**的。

> ![A screen shot of a computer AI-generated content may be
> incorrect.](./media/image23.png)

11. 讓命令運行，打開一個新的 terminal。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

12. **啟動React前端（Terminal 3）**:

    - 輸入下面給出的命令，導航到 react-frontend 目錄。

> +++cd agentic_ai/applications/react-frontend+++

- 輸入以下命令即可啟動代理界面的**React前端**。提供用戶界面，可與代理互動並實時查看其響應。

> +++npm start+++

- 編譯可能需要一些時間。請忽略警告，等待它完成。一旦**webpack成功編譯完成**，代理應用程序將在本地運行於: [http://localhost:3000](http://localhost:3000/).

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image24.png)

13. 當三個終端都運行時，代理應用會在你的瀏覽器中啟動，你可以用它與代理交互並測試其功能。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

**注意**：確保三個終端均在運行。如果有停止的指令，請重新執行相應的命令。如果這三個都不激活，可能會遇到連接錯誤。

14. 在聊天中發送以下提示（**1），**查看回復（**2）**:

+++Customer 251, what's my billing summary?+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

**注意**：確保三個終端均在運行。如果有停止的指令，請重新執行相應的命令。如果這三個都不激活，可能會遇到連接錯誤。

15. 查看輸出，是 ChatAgent（self.\_agent）解釋了提示，可能稱為 **MCP
    工具**，並生成了輸出。

    - 客服將您的請求理解為對**客戶251的賬單查詢**。

    - 它使用 **MCP 工具**獲取結構化計費數據。

    - 該智能體按預期工作——它動態集成工具輸出和 AI
      推理，以回答用戶特定的問題。

16. 完成測試後，返回 VS Code
    並終止所有正在運行的終端會話。這確保了即將到來的多智能體工作流程無幹擾地運行。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

## 任務3：多代理工作流程設計

在這項任務中，你將設計和實施先進的多代理工作流程，展示不同的協調模式。您將首先通過中央管理器協調多個專業代理，協同處理複雜查詢，然後探索基於切換的系統，該系統根據用戶意圖動態切換控制權。

### 任務3.1：協調多智能體工作流程

在這項任務中，你將協調一個多代理工作流程，中央編排者協調多個專業代理，協作處理複雜的用戶查詢，生成準確且基於工具的響應。

1.  導航到 **agent (1) \> agent_framework (2) \> multi_agent (3) \>
    magentic_group (4)**並查看代碼**(5)**。 

    - 該代碼代表了**多代理編排**框架，因為它定義了一個系統，多個專業代理在中央編排器的指導下協作。

> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image28.png)

- \_create_participants
  初始化多個專業代理（CRM/計費、產品/促銷、安全/認證）。

- 每個代理人:

  - 有特定的領域和工具。

  - 只與編排器通信，不直接與用戶溝通。

  - 提供事實性、工具支持的回答。

- 以下是多代理工作流程中使用的代理

  - **CRM與計費代理**–
    利用事實工具支持的數據處理客戶賬戶、訂閱、賬單、發票、付款及相關查詢。

  - **產品與促銷代理**——通過結構化資源提供產品供應情況、促銷、折扣、資格和條款。

  - **安全與認證代理**——通過日誌和工具管理安全事件、認證問題、賬戶鎖定及風險緩解指導。

2.  導航到.env文件**（1）**，注釋單個代理變量（2），然後輸入以下命令添加**編排多代理**變量**（3）**。

+++AGENT_MODULE=agents.agent_framework.multi_agent.magentic_group+++

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image29.png)

3.  選擇 **File (1)** ，然後選擇 **Save(2)**。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

4.  現在，按照步驟啟動完整的代理應用，啟動其三個核心組件:

5.  在Visual Studio代碼窗口中，點擊**省略號（...）（1），**然後是
    **Terminal (2)**，然後是 **New Terminal (3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

6.  **啟動MCP服務器（Terminal 1）**: (MCP 目錄位於項目根級)

    - 執行以下命令啟動**MCP服務器**，該服務器會暴露代理可以調用的API，作為工具調用。
      (服務器運行在 [http://localhost:8000](http://localhost:8000/))

> cd mcp
>
> uv run python mcp_service.py

7.  讓命令運行，打開一個新的終端。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

8.  **啟動後端（Terminal 2）**:

    - 執行以下命令啟動託管代理工作流、會話管理和API端點的後端服務器。

> cd agentic_ai/applications
>
> uv run python backend.py

- 這是前端通信的核心應用邏輯。確保**連接是開啟**的。

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image23.png)

9.  讓命令運行，打開一個新的終端。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

10. **啟動React前端（Terminal 3）**:

    - 輸入下面給出的命令，導航到 react-frontend 目錄。

> +++cd agentic_ai/applications/react-frontend+++

- 輸入以下命令即可啟動代理界面的**React前端**。提供用戶界面，可與代理互動並實時查看其響應。

> +++npm start+++

- 一旦**webpack成功編譯完成**，代理應用程序將在: [http://localhost:3000](http://localhost:3000/).

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

11. 在聊天中發送以下提示，並在左側面板查看回復:

+++Customer 251, what's my billing summary?+++

12. 編排器就像管理器或路由器。它讀取用戶查詢並決定由哪個專業代理處理。它會根據上下文和關鍵詞（如“計費”、“促銷”、“登錄”）來做出這個決定。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.png)

13. 編排器將任務分配給域代理。編排器將查詢發送給這些內部代理之一:

    - crm_billing – 賬單、發票、付款

    - product_promotions – 產品、折扣、優惠

    - security_authentication – 安全、登錄、賬戶鎖定

14. 針對你的查詢（“計費摘要”），編排器會將其路由到**crm_billing**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

- 域代理使用連接工具。每個代理都可以通過MCP服務器訪問特定的工具（API）。

- 例如：crm_billing可以呼叫get_customer_detail、get_billing_summary、get_invoice_payment等。

- 代理調用合適的工具，獲取結構化數據，並生成事實回應。

15. 完成測試後，返回 VS Code
    並終止所有正在運行的終端會話。這確保了即將到來的多智能體工作流程無幹擾地運行。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

### 任務3.2：切換模式多智能體系統

在這項任務中，你將探索一種基於切換的多代理系統，該系統根據用戶意圖，對話在專業代理（如計費、促銷或安全）之間無縫切換，確保跨域的交互流暢且具上下文感知。

- **工作原理**

  - 用戶直接與域代理交互——例如CRM和計費代理。

  - 意圖分類器檢查用戶的新消息是否屬其他域名（如促銷或安全）。

  - 如果是這樣，系統會自動將對話（“切換”）給相應的專業代理。

  - 每個代理都有與其領域相關的篩選工具（計費、促銷或安全）。

  - 切換過程很順暢，有上下文轉移，這樣新客服就能理解對話歷史。

1.  展開 **agents (1) \> agent_framework (2) \> multi_agent (3) \>
    handoff_multi_domain_agent (4)**  並查看代碼**（5）**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image32.png)

2.  導航到.env文件**（1）**，注釋“**Handoff Pattern Multi-Agent
    System** ”變量**（2）**，並輸入以下命令添加切換模式多智能體系統變量**（3）**。

+++AGENT_MODULE=agents.agent_framework.multi_agent.handoff_multi_domain_agent+++

- 輸入以下命令，控制切換過程中傳遞多少過去對話上下文。-1表示轉移所有之前對話回合（**4）。**

> +++HANDOFF_CONTEXT_TRANSFER_TURNS=-1+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

3.  選擇 **File (1)** ，然後選擇 **Save(2)**。

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

4.  現在，按照步驟啟動完整的代理應用，啟動其三個核心組件:

5.  在Visual Studio代碼窗口中，點擊**省略號（...）（1），**然後是
    **Terminal (2)**，然後是 **New Terminal (3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

6.  **啟動MCP服務器（Terminal 1）**: (MCP 目錄位於項目根級)

    - 執行以下命令啟動**MCP服務器**，該服務器會暴露代理可以調用的API，作為工具調用。
      (服務器運行於http://localhost:8000)

> cd mcp
>
> uv run python mcp_service.py

7.  讓命令運行，打開一個新的終端。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

8.  **啟動後端（Terminal 2）**:

    - 執行以下命令啟動託管代理工作流、會話管理和API端點的後端服務器。

> cd agentic_ai/applications
>
> uv run python backend.py

- 這是前端通信的核心應用邏輯。確保**連接是開啟**的。

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image23.png)

9.  讓命令運行，打開一個 new terminal。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

10. **啟動React前端（Terminal 3）**:

    - 輸入下面給出的命令，導航到 react-frontend 目錄。

> +++cd agentic_ai/applications/react-frontend+++

- 輸入以下命令即可啟動代理界面的**React前端**。提供用戶界面，可與代理互動並實時查看其響應。

> +++npm start+++

- 一旦**webpack成功編譯完成**，代理應用程序將在: [http://localhost:3000](http://localhost:3000/)。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

11. 在聊天中發送以下提示，並在左側面板查看回復:

+++Customer 251, what's my billing summary?+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image34.png)

- 這裡，意圖分類器會路由到crm_billing域

- get_billing_summary工具為客戶251呼叫

12. 你可以就賬單問題繼續提出以下問題:

+++Yes, I would like to view the invoice details+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image35.png)

**注意**：如果你收到回復，因為我無法獲取發票詳情，因為引用的數字是發票ID，不是訂閱ID。請提供訂閱ID，或者如果你需要特定發票的詳細信息，請告訴我，以便我能正確協助你。請提供以下提示。

+++Yes, I would like to view the invoice details for customer 251+++

13. 現在讓我們嘗試一個與另一個域名相關的查詢，以測試切換的運作方式。

14. 輸入以下與產品與促銷相關的查詢並查看回復。

+++Are there any promotions available for my subscription plan+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image36.png)

- 由於之前的對話由CRM和計費專家處理，系統檢測到域名變更。它決定將對話交給產品與促銷專員。

- 系統根據HANDOFF_CONTEXT_TRANSFER_TURNS設置，可以選擇性地將之前的對話上下文（比如我們討論的是哪位客戶）轉移到新的客服上。

- 產品與促銷專員只能使用與促銷、計劃和產品信息相關的工具（例如get_promotions、get_eligible_promotions）。

15. 完成測試後，返回 VS Code
    並終止所有正在運行的終端會話。這確保了即將到來的多智能體工作流程無幹擾地運行。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

**摘要**

在這個實驗室中，你創建了一個單代理工作流程，通過MCP與外部工具集成，並探索了多代理設計，即多個專業代理根據用戶意圖協作或交接對話。你配置了環境變量，啟動了完整的代理環境，並測試代理如何智能響應特定領域的查詢。
