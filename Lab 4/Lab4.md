# 实验室4：开发多代理健康计划报告生成系统

**概述**

在这个实验室中，你将开发一个智能多代理系统，专门设计用于自动生成全面的健康计划报告。该系统利用四位专业AI代理协同工作的协作力量，检索、分析、生成并验证详细的健康保险文件。多智能体架构展示了自主智能体如何协同工作，完成单一智能体难以有效处理的复杂任务。

你将建造这4个AI代理:

- **搜索代理**——该代理将搜索Azure
  AI搜索索引，以获取有关特定健康计划政策的信息。

- **报告代理人**——该代理人将根据搜索代理人返回的信息生成一份关于健康计划政策的详细报告。

- **验证代理**——该代理将验证生成的报告是否符合指定要求。在我们的案例中，确保报告包含有关保险除外条款的信息。

- **编排代理**——该代理将作为编排者，管理搜索代理、报告代理和验证代理之间的通信。

![A diagram of a company AI-generated content may be
incorrect.](./media/image1.png)

编排是多智能体系统的关键组成部分，因为我们创建的智能体需要能够相互通信以实现目标。

我们将使用 Azure AI
代理服务来创建搜索、报告和验证代理。然而，为了创建编排代理，我们将使用
Semantic
Kernel。语义内核库提供了开箱即用的功能，用于多智能体系统的编排。

**实验室目标**

你将在实验室执行以下任务。

- 任务1：创建Azure AI搜索索引

- 任务二：创建搜索、报告和验证代理。

## 任务1：创建Azure AI搜索索引

在此任务中，您将创建Azure
**AI搜索索引**，以存储健康保险计划文件的矢量化表示，从而实现AI驱动的搜索和分析高效检索。

1.  进入**Azure portal**，搜索 **AI Search (1)** ，并从服务中选择 **AI
    Search (2)** 资源。

![](./media/image2.png)

2.  这会引导你进入 AI Foundry，在 **AI Search** (1) 中点击
    **Create**(2)。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

3.  在 **Create a Search service** 面板中输入以下信息，点击“**Review +
    Create** (4)

    - 订阅 : **Leave default subscription**

    - 资源组 : 选择 **AgenticAI (1)**

    - 服务名称 : **my-search-service- (2)**

    - 位置 : **(3)**

![](./media/image4.png)

4.  在 **Review + Create** 中，点击 **Create**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

5.  等部署完成后再点击“**Go to resource**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

6.  在左侧菜单的 **Settings** 里进入 **Keys (1)** 。在 **API Access
    control** 下选择 **Both（2）**。 

![](./media/image7.png)

7.  选择“**Yes**”，选择 **Are you sure want to update the API Access
    Control for this serach service**。  

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image8.png)

8.  导航 在 **Settings** 中导航到
    **Identity(1)** 。在系统分配中，将状态设置为 **On(2)** ，然后点击
    **Save(3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.png)

9.  选择“**Yes**”作为 **Enable System assigned managed identity**“。 

![A close-up of a computer error AI-generated content may be
incorrect.](./media/image10.png)

10. 在Azure门户中，搜索 **Storage accounts (1)** ，并从服务中选择
    **Storage accounts (2)** 。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

11. 选择以 **aifoundry** 开头的存储账户。

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image12.png)

12. 选择 **Access control (IAM) (1)**，然后点击 **Add(2)**，再选择 **Add
    role assignment**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

13. 在 **Job function roles** 中，搜索 **Storage Blob Data Reader
    (1)**，选择 **Storage Blob Data Reader (2)**，然后选择 **Next
    (3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

14. 在**“Add role assignment**”页面,

    - 在“Members”中，选择“ **Managed identity(1)**

    - 选择 **Members (2)**

    - 托管身份: **search service(1)** **(3)**

    - 然后选择 **my-search-service-**（4）搜索服务。

    - 点击**Select (5)**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.png)

15. 点击两遍**“Review + assign**”。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image16.png)

16. 访问 **Azure OpenAI，my-openai-service** 。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.png)

17. 选择 **Access control (IAM) (1)**，然后点击 **Add(2)**，再选择 **Add
    role assignment**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.png)

18. 在“**Job function roles**”中，搜索“**Cognitive Services OpenAI User
    (1)**，选择 **Cognitive Services OpenAI User (2)**，然后选择 **Next
    (3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.png)

19. 在**“Add role assignment**”页面,

    - 在“Members”栏目中，选择**Managed identity(1)**

    - 选择 **Members (2)**

    - 托管身份: **search service(1)** **(3)**

    - 然后选择**my-search-service-**（4）搜索服务。

    - 点击**Select (5)**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

20. 选择 **Review + assign** 两次。

![](./media/image21.png)

21. 进入 **Azure Portal**，搜索 **Storage account (1)** 并选择 **Storage
    account (2)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

22. 选择以**aifoundryhub**开头的存储账户。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

23. 点击数据存储下的 **Containers (1)** ，然后选择 **+Container(2)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

24. 在新容器页面输入 **healthplan（1）**作为名称，点击 **Create（2）。**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

25. 点击它打开 **healthplan **容器。

![](./media/image26.png)

26. 点击 **upload (1)** 来上传文件，然后点击 **browse for files (2)**。

> ![](./media/image27.png)

27. 进入C：\LabFiles\Day-1\azure-ai-agents-labs\data**（1）**，选择两个PDF上传**（2）**，点击
    **Open (3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.png)

28. 点击 **Upload**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.png)

**注意：**如果系统让你选择现有容器，请从下拉菜单选择健康计划。

29. 进入 **Azure AI 搜索**服务，选择
    **my-search-service**-。![](./media/image30.png)

30. 点击导入 **import data (new)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

31. 选择 **azure blob storage**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image32.png)

32. 选择**RAG**模型。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

33. 在配置你的Azure Blob存储中，输入以下信息并点击**“Next（5）**”：

[TABLE]

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image34.png)

34. 在“向量化你的文本”中，输入以下信息并点击 **Next (7)**:

[TABLE]

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image35.png)

35. 点击两次**“Next **”。

36. 输入 **health-plan (1)** 作为**Objects name prefix** ，点击 **Create
    (2)**。

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image36.png)

**注意**：在搜索服务中将数据上传到索引可能需要5-10分钟。

37. 点击弹窗中的**“Start searching**”。

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image37.png)

38. 请前往 **ai-foundry-project-** 的 **Overview** (1) 页面。并点击
    **Open In management center**(2)。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

39. 选择 **Connected resources** (1)，然后点击 **New connection** (2)。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.png)

40. 在搜索栏输入 **Azure AI Search**（1），并选择 **Azure AI
    Search**（2）。

![](./media/image40.png)

41. 点击 **Add connection** 以继续。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.png)

## 任务2：创建搜索、报告和验证代理

在此任务中，您将创建搜索、报告和验证代理，以检索、生成和验证健康计划报告。这些代理将协同工作，确保准确性和符合要求。每位代理在检索、汇编和确保报告准确性方面都扮演着独特角色。

1.  打开**实验4 - 开发多代理System.ipynb**文件，这本**实验4 -
    开发多代理System.ipynb**笔记本指导你如何开发包含搜索、报告、验证和编排代理的多代理系统，以生成和验证健康计划报告。每位代理在检索、汇编和确保报告准确性方面都扮演着独特角色。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.png)

2.  选择 右上角可选的 **Select kernel（1）** 设置，并从列表中选择 **venv
    （Python 3.x.x）（2） 。**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.png)

3.  运行该单元，开发一个集成 Azure AI 搜索、GPT-4o
    和语义内核的**多智能体系统**，实现智能任务执行。这种配置使多个AI代理能够协作获取信息、生成回复并处理复杂查询。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image44.png)

4.  运行该单元格创建**搜索代理**，利用GPT-4o从Azure
    AI搜索中获取健康计划详情。该代理能够高效检索健康计划文档中的结构化信息。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.png)

5.  运行该单元格创建**报告代理**，该代理使用GPT-4o生成详细的健康计划报告。该代理通过提供结构化的洞察、保障详情及各种计划的除外条款，丰富了相关文件。

![](./media/image46.png)

6.  运行该单元以创建**验证代理**，确保报告代理生成的报告符合质量标准，特别是检查覆盖除外。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.png)

7.  **创建一个多代理系统**：当你运行下面的单元格时，你会在VS
    Code顶部弹出一个聊天框，要求你输入健康计划名称。

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image48.png)

8.  如果你还记得，我们把两个健康计划上传到了搜索索引中。当提示时，请在顶部的框中输入以下任一健康计划，并按**回车**开始运行多智能体系统:

    - **Northwind Health Standard**

    - **Northwind Health Plus**1

![](./media/image49.png)

9.  当框框出现在顶部时，输入“exit”并按回车键停止运行代码块。

**注意**：成功运行该单元后，您将获得以下结果。

> Orchestrator Agent is starting...
>
> Calling SearchAgent...
>
> SearchAgent completed successfully.
>
> Calling ReportAgent...
>
> ReportAgent completed successfully.
>
> Calling ValidationAgent...
>
> ValidationAgent completed successfully.
>
> The report for Northwind Plus has been generated. Please check the
> Northwind Plus Report.md file for the report.
>
> Orchestrator Agent is starting...

**摘要**

在这个实验室里，你成功开发了一个智能多代理系统，通过协调四个专业的 AI
代理，自动生成全面的健康计划报告。你创建了一个 Azure AI
搜索索引来存储矢量化的健康保险文件，然后构建了一个搜索代理来获取保单信息，一个报告代理用于生成详细文档，一个验证代理用于确保符合要求，还有一个使用语义内核管理所有代理之间的通信的编排代理。通过运行多代理系统并结合真实健康计划数据，你展示了自主代理如何有效协作完成单一代理难以完成的复杂任务，展示了企业级代理编排模式，适用于实际业务应用。

恭喜你！你已经成功完成了实验。
