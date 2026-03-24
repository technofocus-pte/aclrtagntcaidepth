# 实验2：搭建AI项目并从VS Code中完成聊天

**概述**

在本实验室中，你将通过在 Microsoft Foundry 中创建和配置 AI
项目，部署大型语言模型（LLM）和嵌入模型，并将项目连接到 Visual Studio
Code，准备构建 AI
代理所需的完整开发环境。然后，你将通过运行代码中的简单聊天完成来验证设置，确保环境配置正确，准备好开发AI驱动应用。

实验室目标

你将在实验室执行以下任务。

- 任务1：在Microsoft Foundry建立AI项目

- 任务2：部署大型语言模型并嵌入模型

- 任务3：安装依赖，创建虚拟环境，并创建环境变量文件

## 任务一：在Microsoft Foundry建立AI项目

在这个任务中，你将在 Microsoft Foundry 中创建并配置一个 AI
项目。这包括设置必要资源、定义项目参数，并确保环境准备好部署AI模型。完成这项任务时，你将拥有一个完全初始化的AI项目，作为进一步开发和实验的基础。

1.  在 Azure Portal 页面顶部的搜索资源框中，输入 **Microsoft Foundry
    （1），**然后在 服务中选择 **Microsoft Foundry （2）。**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

2.  在左侧导航窗格“**Use with Foundry**”中，选择 **AI Hubs
    (1)**。在AI枢纽页面，点击 **Create (2)** ，从下拉菜单中选择 **Hub
    (3)** 。

![](./media/image2.png)

3.  在**Create an Azure AI hub **的面板中，输入以下细节，在
    **Basics (1)** 项下。 :

    - 订阅 : **Leave default subscription** **(2)**

    - 资源组 : **AgenticAI** **(3)**

    - 地区 : **East US2** (4)

    - 名称 : ** <+++ai-foundry-hub@lab.LabInstance.Id>+++ (5)**

    - 连接 AI 服务，包括OpenAI：点击 **Create New (6)。**

    - Connect AI Services，包括 OpenAI：提供名称
      **<+++my-ai-service@lab.LabInstance.Id>+++ （7）**。

    - 点击 **Save** **(8)**，然后点击 **Next:Storage** **(9)**,

> ![](./media/image3.png)

4.  点击“**Review + Create** 标签”，然后点击**“Create”。**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

![](./media/image5.png)

5.  等待部署完成后，点击**“Go to resource**”。

![](./media/image6.png)

6.  在概览面板中，点击**“Azure AI Foundry**”。这会引导你进入 Microsoft
    Foundry 门户。

![](./media/image7.png)

7.  向下滚动，点击 中心概览中的**+ New project**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

8.  输入项目名称为
    [**+++ai-foundry-project@lab.LabInstance.Id+++，**然后点击](mailto:+++ai-foundry-project@lab.LabInstance.Id+++，然后点击)
    **Create (2)。**

![](./media/image9.png)

9.  项目创建后，向下滚动并复制 **Project connection
    string**，然后粘贴到记事本或安全位置，因为这些字符串将是即将完成任务的必需品。

![A screenshot of a project AI-generated content may be
incorrect.](./media/image10.png)

## 任务2：部署 LLM 模型并嵌入模型

在这项任务中，你将在Microsoft
Foundry项目中部署一个大型语言模型（LLM）和一个嵌入模型。这些模型将在即将到来的实验室中用于AI驱动应用和基于矢量的搜索功能。

1.  在你的 **Microsoft Foundry project**中，进入“**My
    assets** **(1)** ”部分，然后选择 **Models +
    endpoints** **(2)**。点击 **Deploy model** **(3)**，然后选择
    **Deploy base model** **(4)** 以继续。

![](./media/image11.png)

2.  在“**Select a model** ”窗口中，搜索 **gpt-4o** **(1)**，选择
    **gpt-4o** **(2)** ，选择 **Confirm** **(3)**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.png)

3.  在 **Deploy model gpt-4o** 窗口中，选择 **Customize**。 

![](./media/image13.png)

- 部署名称: **gpt-4o** **(1)**

- 部署类型: **Global Standard** **(2)**

- 将**型号版本**更改**为2024-08-06（默认）（3）**

- 将每分钟代币的汇率限制改为**20万（4）**

- 点击 **Connect and Deploy (5)**

![](./media/image14.png)

4.  点击 **Model + Endpoints** **(1)**，你可以看到已部署的
    **gpt-4o** **(2)**  模型。 

![](./media/image15.png)

5.  返回 **Azure Portal**，搜索 **Open AI** **(1)** 并选择 **Azure Open
    AI** **(2)** 资源。 

![](./media/image16.png)

6.  关于**Microsoft Foundry | Azure OpenAI** 页面，选择 **+
    Create** **(1)** ，然后选择 **Azure OpenAI** **(2)** 以创建 Azure
    OpenAI 资源。

![](./media/image17.png)

7.  在 **Create Azure
    OpenAI** 页面，提供以下设置，点击“**Next** **(6)**” :

[TABLE]

> ![](./media/image18.png)

8.  点击**“Next**”，直到出现“审核+提交”标签。

9.  在“**Review + submit**”页面，点击“**Create**” 

![](./media/image19.png)

10. 等部署成功后，选择“**Go to resource**”。

![](./media/image20.png)

11. 在 **my-openai-service** 资源页面，选择 **Go to Foundry portal**。

![](./media/image21.png)

12. 在你的 AI Foundry 项目中，导航到 **Shared resources**  部分，选择
    **Deployments** **(1)**。点击 **Deploy model** **(2)**，然后选择
    **Deploy base model** **(3)** 继续。

![](./media/image22.png)

**注意**：Azure AI
Search中的导入和矢量化向导（后续实验室将使用）尚未支持在您的AI
Foundry项目中嵌入文本模型。因此，我们需要创建一个Azure
OpenAI服务，并在那里部署文本嵌入模型。我们稍后创建向量索引时会使用这个文本嵌入模型。

13. 在“**Select a model** ”窗口中，搜索
    **text-embedding-3-large** **(1)**，然后选择
    **text-embedding-3-large** **(2)** ，选择 **Confirm** **(3)**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

14. 在 **Deploy model text-embedding-3-large** 窗口中，

    - 部署类型: 选择 **Standard (1)**

    - 每分钟代币数的速率限制: **120K (2)**

    - 选择 **Deploy (3)** 以部署该模型。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

15. 点击 **Deployment (1)**，你可以看到已部署的 **text-embedding-3-large
    (2)**  模型。

![](./media/image25.png)

## 任务3：安装依赖，创建虚拟环境，并创建环境变量文件

在这个任务中，你将安装所需的依赖，搭建虚拟环境，并创建环境变量文件。这确保了受控的开发环境，并安全地管理您的
AI 项目配置设置。

1.  在你的 **Lab VM** 上，启动 **Visual Studio Code。**

2.  点击 **File** **(1)**，然后选择**Open Folder**。

![](./media/image26.png)

3.  进入 C：\LabFiles\Day-1 \\ **（1）**，选择 **azure-ai-agents-labs
    （2）** 文件夹，然后点击 **Select folder** **(3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

4.  点击 **Yes, I Trust the authors**,

![](./media/image28.png)

5.  点击 **elipses(...)** **(1)**，然后是 **Terminal** **(2)** ，然后是
    **New Terminal** **(3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.png)

6.  确保你在 **azure-ai-agents-labs**
    项目目录里。请执行以下PowerShell命令创建并激活您的虚拟环境:

7.  python -m venv venv

+++venv/Scripts/activate+++

![](./media/image30.png)

8.  运行下面的 PowerShell 命令。这样就能安装所有所需的软件包:

9.  pip install -r requirements.txt

+++pip install azure-ai-ml azure-identity+++

![A screen shot of a computer code AI-generated content may be
incorrect.](./media/image31.png)

10. 运行下面的 PowerShell 命令来安装或升级 PIP 到最新版本。

+++python.exe -m pip install --upgrade pip+++

![](./media/image32.png)

11. 执行以下命令登录你的Azure账户。

+++az login+++

12. 选择授权的用户账户。

13. 授权完成后，返回Visual Studio代码。

![](./media/image33.png)

14. 打开 **Sample.env** 文件，输入必要的环境变量。

![](./media/image34.png)

- 进入 Microsoft Foundry 门户，在“My assets”下的 **Models +
  endpoints(1)** 部分点击
  **gpt-4o** **(2)** 模型，从右侧面板复制底下，复制粘贴 **Target URI
  （1）** 和 **Key (2)** 在记事本中**Endpoint**

![](./media/image35.png)

![](./media/image36.png)

15. 在 **Sample.env** 文件中,

    - AIPROJECT_CONNECTION_STRING：提供你在任务1第9步复制的 **Project
      connection string**

    - CHAT_MODEL_ENDPOINT: 提供你在上一步复制的**gpt-4o**模型的 **Target
      URI** 

    - CHAT_MODEL_API_KEY: 提供你在上一步复制的**gpt-4o**模型的**Key** 值

    - CHAT_MODEL: **gpt-4o**

![](./media/image37.png)

16. 将更改保存到**Sample.env**文件中。

17. 运行下面的 PowerShell 命令。这样可以创建你的 **.env** 文件:

+++cp sample.env .env+++

![](./media/image38.png)

18. 稍后打开**实验室1 - Project Setup.ipynb**文件。**实验室 1 - Project
    Setup.ipynb** 笔记本指导你如何在 Microsoft Foundry 中搭建 AI
    项目，部署大型语言模型和嵌入模型，以及配置 VS Code
    连接。它还包含一个简单的聊天完成API调用来验证设置。运行此笔记本确保您的环境为开发AI驱动应用而配置正确。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image39.png)

19. 选择右上角可选的“**Select kernel (1)** ”设置，并选择
    **Install/enable selected extensions (python+jupyter) (2)**。

![](./media/image40.png)

20. 选择 **Python Environments **，确保 Jupyter Notebook 运行在正确的
    Python 解释器和必要的依赖中。

![](./media/image41.png)

21. 从列表中选择**venv（Python 3.x.x），**因为该版本很可能是与Microsoft
    Foundry SDK及其他依赖兼容的必要条件。

![](./media/image42.png)

**注意：**如果 **venv（Python 3.x.x）**未出现在列表中。关闭并打开Visual
Studio代码。

22. 运行第一个单元，导入用于使用 Azure AI 服务所需的 Python 库。

![](./media/image43.png)

23. 运行下面的单元格，从环境变量中获取项目连接字符串和模型名称。这些数值对于安全地与大型语言模型（LLM）交互，无需硬编码敏感信息。

![](./media/image44.png)

24. 运行下面的单元格，使用连接字符串连接到你的 Microsoft Foundry
    项目。这与 AIProjectClient 建立了安全连接，使您能够与项目资源交互。

![](./media/image45.png)

25. 运行下面的单元格，使用你的 Microsoft Foundry 项目与 GPT-4o
    模型交互。该代码初始化聊天客户端，发送关于泰迪熊的笑话请求，并打印回复。最后看看聊天模型提供的输出。

![](./media/image46.png)

> **注意：**这一步的输出由AI模型动态生成，每次可能有所不同。

**摘要**

在本实验室中，你通过在Microsoft
Foundry中创建和配置AI项目，部署GPT-4o大型语言模型和文本嵌入3-large嵌入模型，并建立Visual
Studio
Code的安全连接，成功搭建了一个完整的AI开发环境。你安装了所需的依赖，创建了虚拟环境，并配置了环境变量以安全地管理敏感信息。最后，你通过运行简单的聊天完成API调用验证了设置，确认环境配置正确，准备好开发AI驱动应用。

你已经成功完成了这个实验。请点击“Next \>\>”继续。
