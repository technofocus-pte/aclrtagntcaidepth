# 實驗12：在企業AI代理中實施 Responsible AI 與內容安全

**預計持續時間**：15分鐘

**概述**

在該實驗室中，參與者探討了 Responsible AI
在企業級代理系統中的重要性。他們將理解Microsoft如何將負責任的AI原則——如公平、安全、問責和透明度——整合進代理框架和Microsoft
Foundry。參與者還將學習如何在 Microsoft Foundry
門戶中直接配置和驗證內容安全過濾器，以確保部署的代理能夠道德且安全地響應。

**實驗室目標**

你將在實驗室執行以下任務。

- 任務1：理解 Responsible AI 與內容安全

- 任務2：配置並驗證Microsoft Foundry中的內容過濾器

## 任務1：理解 Responsible AI 與內容安全【僅閱讀】

在本任務中，您將學習Microsoft的負責任AI原則，並理解其如何應用於Microsoft代理框架和Microsoft
Foundry。Responsible AI
確保智能系統安全、合乎道德且公平地運作，這是在企業環境中部署多智能體解決方案時的關鍵要求。

什麼是 Responsible AI？

1.  Microsoft的 Responsible AI 框架基於六個指導 AI
    系統開發、部署和運行的基礎原則:

2.  公平——AI
    系統應公平對待所有個人和群體。對於企業代理來說，這意味著確保決策或響應不會反映或放大人力資源、合規或財務用例中的偏見。

3.  可靠性與安全性——AI模型必須穩定地表現，並優雅地處理故障。代理應返回事實、可驗證的信息，避免不安全或誤導性輸出。

4.  隱私與安全——AI 系統必須保護用戶數據和組織信息。代理框架與Azure
    Identity（Entra ID）安全集成，並尊重企業數據邊界。

5.  包容性——代理必須設計成賦能所有用戶，支持跨語言、地域和背景的無障礙性。

6.  透明度——用戶應瞭解 AI
    決策的過程。代理應盡可能解釋其推理，並通過遙測和可觀察性提供可追蹤的響應。

7.  問責制——人工監督依然是核心。組織必須定義治理結構，以審查和管理AI驅動的結果。

8.  這些原則構成了在企業環境中構建可信且合規的AI代理的基礎。

為什麼 Responsible AI 在企業代理中如此重要

1.  當多名代理人協作處理敏感話題——如員工政策、財務報銷或合規報告時——錯誤信息、偏見或不當行為的風險會增加。通過嵌入負責任的
    AI 實踐，組織能夠:

2.  確保代理間通信的一致性和可靠性。

3.  防止有害、歧視性或不安全的輸出。

4.  保持符合全球法規（GDPR、HIPAA、ISO 27001等）。

5.  增強用戶對 AI 自動化的信任。

6.  Microsoft代理框架通過Microsoft Foundry原生集成 負Responsible
    AI，直接在模型和部署層面提供治理、可追溯性和安全執法。

內容安全與倫理回應過濾

1.  內容安全是Microsoft Responsible AI 基礎設施的關鍵組成部分。

2.  在 Microsoft Foundry
    中，內容安全過濾器會自動檢測並阻止多個類別的有害或敏感輸出，包括:

    - 仇恨與騷擾

    - 暴力與自殘

    - 性內容

    - 敏感或受保護信息（PII）

3.  配置後，這些過濾器能截取用戶提示和模型響應，用標準化的安全響應替代不安全內容，確保合規和用戶保護，無需修改本地代碼。

## 任務2：配置並驗證Microsoft Foundry中的內容過濾器

在此任務中，您將在 Microsoft Foundry 中配置內容安全過濾器，以強制執行
Responsible AI
原則。你將為有害或敏感類別設定保護級別，然後通過測試本地代理驗證過濾器。

1.  在 Microsoft Foundry 頁面，從左側菜單選擇 **Guardrails +
    Controls**，點擊從總覽中 **Create a custom content filter** 。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

2.  在下一頁，保留過濾器的默認名稱，點擊**“Next**”。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

3.  在** set input filter**
    窗格中，你會注意到Azure已經預設了多個參數，如仇恨、暴力、性內容等。這些基線過濾器默認應用，以確保符合Microsoft的負責任人工智能標準。

4.  現在，將每個類別調整為 **Highest
    Blocking** ，以強制執行最大內容安全，這是企業級代理的關鍵要求，然後點擊“**Next** ”應用配置。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

5.  在下一窗格，將所有類別配置為**Highest
    Blocking**，然後點擊**“Next**”。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

6.  在**Deployments**面板中，選擇 **gpt-40-mini**
    模型以添加內容過濾器。點擊“**Next**”。 

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

> **注意：**如果“**Replacing existing content
> filter**”彈窗會打開，請選擇 **Replace。** 

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

7.  在審核面板中，點擊 **Create filter**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

8.  現在自定義內容過濾器已經成功創建。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

9.  回到之前打開的終端的 Visual Studio Code
    窗格，執行以下命令啟動該代理。請提供以下提示以檢查內容過濾功能。

- 寫一篇針對一群人的冒犯性陳述。

- 告訴我關於一群人有什麼仇恨的。

+++python main.py –interactive+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image9.png)

**摘要**

在本實驗室中，您探討了 Responsibe AI
原則如何指導利用Microsoft代理框架進行倫理、合規且值得信賴的AI代理開發。你在
Microsoft Foundry
中配置了內容安全過濾器，自動防止不安全、偏見或不當內容被你的企業代理處理或返回。

你已經成功完成了這個實驗。請點擊“Next \>\>”繼續。
