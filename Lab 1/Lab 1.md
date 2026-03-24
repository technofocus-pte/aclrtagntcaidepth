# 构建与扩展智能代理

**概述**

这个动手实验室介绍利用 Azure AI 服务和 Microsoft 365 Copilot 构建智能 AI
代理。参与者将学习如何利用Copilot进行人力资源工作流程，搭建Microsoft
Foundry项目，构建简单的AI代理，创建RAG（检索增强生成）代理，并开发具备编排能力的多代理系统。

**目标**

到这个实验结束时，你就能:

- **使用 Copilot Studio 构建人力资源助理代理**——使用 Microsoft 365
  Copilot 自动化员工招聘、筛选、培训材料开发、反馈收集和绩效评估。

- **搭建AI项目并完成聊天完成**——在Microsoft
  Foundry中配置AI项目，部署大型语言模型（LLM）和嵌入模型，并建立VS代码连接以完成聊天。

- **构建健康保险计划分析器 AI 代理** - 创建 AI 代理，利用 Azure AI
  服务处理数据并生成可视化（例如比较健康福利计划的条形图）。

- **开发健康计划报告生成多代理系统**——设计和实施协调的多代理系统，由专业代理（搜索代理、报告代理、验证代理和编排代理）协同完成复杂任务。

**前提条件**

参与者应当有:

- **Visual Studio Code（VS Code）：**熟练使用VS
  Code进行编码、调试和管理各种编程语言和框架的扩展。

- **开发技能：具备**Python或JavaScript的基础编程知识，API、SDK的使用经验，以及Visual
  Studio Code的作。

- **命令行/终端**：熟悉运行PowerShell命令和管理虚拟环境。

**组件说明**

- **Azure AI Search**：基于矢量的搜索服务，通过索引和检索相关文档实现
  RAG。

- **Azure OpenAI 服务**：通过 Azure 的企业基础设施提供对 GPT-4o
  和嵌入模型的访问。

- **大型语言模型（LLM）：**用于文本理解和生成的高级 AI 模型，如GPT-4o。

- **嵌入模型**：将文本转换为语义搜索和检索的向量表示（例如，文本嵌入-3-large）。

- **Microsoft 365
  Copilot**：基于AI的文档分析和工作流程自动化生产力工具。

- **Semantic Kernel**: 用于将LLM与编程语言集成并构建编排能力的SDK。

# 实验1：使用Copilot Studio构建人力资源助理代理

预计时长：30分钟

概述

在本实验室中，你将专注于通过使用 Microsoft 365 Copilot 和 Copilot Studio
简化和改进组织内员工的过渡和入职流程。你将学习如何识别合适候选人，制定定制化的过渡和入职计划，生成有效的沟通和培训材料，自动化人力资源工作流程，收集反馈，并建立绩效监控和评估机制。通过利用这些AI驱动的工具，本实验室展示了组织如何确保顺利高效的过渡流程，提升内部流动性，并支持员工成功适应新岗位。

实验室目标

你将在实验室执行以下任务。

- 任务1：快速筛选候选人

- 任务2：开发培训材料

- 任务3：收集反馈

- 任务4：绩效评估

架构图

![image](./media/image1.png)

## 任务一：快速筛选候选人

在这项任务中，你将使用Microsoft 365
Copilot快速评估大量数据分析师职位的申请，并根据相关经验、技术技能和教育背景等具体标准筛选候选人，从而突出显示最佳候选人供进一步审查。

1.  在 Edge 浏览器中添加一个新标签页，使用以下链接打开 Microsoft 365
    Copilot 应用，点击 **Sign in (2)。**

+++https://m365.cloud.microsoft/+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

2.  在“**Sign into Microsoft Azure
    tab**”中，你会看到一个登录界面。请使用以下凭证登录。

- Username - +++@lab.CloudPortalCredential(User1).Username+++

- TAP - +++@lab.CloudPortalCredential(User1).TAP+++

3.  如果你看到弹窗“**Welcome to your Microsoft 365 Copilot
    app**，请点击” **Get started** “。

![A screenshot of a computer application AI-generated content may be
incorrect.](./media/image3.png)

4.  在左侧窗格选择 **Apps** **(1)**，然后从应用部分点击
    **OneDrive（2）。**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

**注意**：如果你看到弹窗“**Welcome to Apps”**，请点击 **X** 关闭弹窗。

![A screenshot of a computer application AI-generated content may be
incorrect.](./media/image5.png)

5.  进入“**My files**”，然后点击 **+ Create or upload (1)** 按钮，选择
    **Folder upload (2)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

6.  进入C：\LabFiles\Day-1\data **（1）**，点击CV **（2）**文件夹，选择
    **Upload (3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

7.  在“Upload 5 files to this site?”中选择 **Upload**  弹出窗口。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

8.  再次点击**+ Create or upload (1)** ，然后选择 **Folder upload
    (2)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

9.  进入C：\LabFiles\Day-1**（1）**，点击数据**（2）**文件，点击
    **Upload 3**。 

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.png)

10. 选择上传“**Upload** ”，在“Upload 19 files to this site?” 弹出窗口。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.png)

11. 从左侧面板返回 **M365 Copilot**，选择
    **Apps** **(1)**，然后从应用部分点击 **Copilot** **(2)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

12. 从左侧面板进入 **Copilot**，点击 **Chat
    (1)**。然后点击聊天面板底部的**+ (Add)** 图标**（2）**，选择
    **Upload images and files (3)**。

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image12.png)

13. 在文件资源管理器弹窗中，进入 C：\LabFiles\Day-1\data\CV
    **（1）**文件夹，选择 **first 3** **(2)**  个文件，点击
    **Open** **(3)**。

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image13.png)

14. 在 **Copilot chat** 中，**三个文件**上传成功后，点击 **enter**。 

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

15. 在活跃的 Copilot chat 中，点击消息框下方的 **+ (Add) (1)** 
    图标，然后选择 **Upload images and files (2)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.png)

16. 在文件资源管理器弹窗中，进入C：\LabFiles\Day-1\Data\CV
    **（1）**文件夹，选择 **最后2（2）**个文件，点击 **Open** **(3)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image16.png)

17. 在 **Copilot chat** 中，**两个文件（1）**成功上传后，点击 **enter
    (2)**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.png)

18. 在聊天框中，输入以下提示**（1），**并点击 **Sent (2) **按钮:

> Microsoft 365
> Copilot，请帮我筛选和筛选数据分析师候选人，基于SQL、Python和数据可视化工具等必要资质筛选和筛选简历。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.png)

19. 跟进下面的提示，点击 **Sent **按钮

> 制作一份顶级数据分析师候选人的总结报告，包括他们的技能、工作经验和教育背景。
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

**成果**：人力资源团队高效筛选最合格的候选人，节省时间并确保招聘工作有针对性。

## 任务2：开发培训材料。

在这项任务中，你将利用Microsoft
Copilot为新员工准备全面的培训材料，创建个性化的入职内容，包括针对岗位的指南、公司政策以及所用工具和技术概述，确保培训材料详尽、结构合理，并针对员工的角色量身定制。

1.  在聊天框中，输入以下提示**（1），**并点击 **Sent (2) **按钮:

> 为新数据分析师制定全面的入职培训计划，包括公司政策、数据工具培训和团队介绍等主题。
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image20.png) ![A screenshot of a web page
> AI-generated content may be incorrect.](./media/image21.png)

2.  接下来是下面的提示（**1），**然后点击 **Sent (2)** 按钮。

> 制作一个互动式培训演示，涵盖数据分析最佳实践和关键绩效指标，并生成可下载的PPT。
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image22.png)

**注意**：执行此提示后，您将下载一份PowerPoint演示文稿，然后您可以编辑或设计。如果文件未被下载，请尝试找到带有演示标题的超链接，如截图所示。

**注意**：执行此提示后，“待下载的PowerPoint演示文稿”选项未显示。请重演上述提示。

结果：新员工会获得组织良好的培训材料，帮助他们快速上手并高效完成职责。

## 任务3：收集反馈

在此任务中，你将利用Microsoft
Copilot生成和分发反馈调查，收集和分析反馈，深入了解招聘和入职流程的优势以及需要改进的领域。

1.  在聊天框中，输入以下提示并点击**Sent **按钮 :

> 为面试官创建一个反馈表，基于技术能力、问题解决能力和文化契合度评估数据分析师候选人。生成可下载的Word或PDF版本。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

2.  跟着下面的提示，点击 **Sent **按钮。

> 向新员工发送调查问卷，收集入职体验反馈并找出改进空间。生成可下载的Word或PDF版本的问卷。
>
> ![A screenshot of a survey AI-generated content may be
> incorrect.](./media/image23.png)
>
> 结果：人力资源部门获得了宝贵反馈，帮助他们完善招聘和入职流程，确保未来员工获得更好的体验。

## 任务4：绩效评估

在这项任务中，你将定期进行绩效评估，通过使用 Microsoft Copilot
创建绩效评估模板、安排评估会议、跟踪成就、收集同事反馈以及编制结构化绩效报告，评估新员工的进展和发展。

1.  在聊天框中，输入以下提示并点击 **Sent **按钮 :

> 为新数据分析师制定绩效评估计划，包含季度评估和目标设定会议，并生成日历CSV文件。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

2.  跟着下面的提示，点击 **Sent** 按钮。

> 生成绩效评估报告模板，包括成就、改进领域和未来目标的部分
> 生成绩效评估模板。
>
> ![A screenshot of a report AI-generated content may be
> incorrect.](./media/image25.png)
>
> 结果：新员工获得建设性的反馈和支持，有助于其职业成长，并为公司内的长期成功做出贡献。
>
> **摘要**
>
> 在本实验室中，你成功地利用 Microsoft 365 Copilot
> 构建了一个人力资源助理代理，以简化员工招聘和入职流程。你学会了如何通过分析简历快速筛选数据分析师候选人，并基于SQL、Python和数据可视化等技术技能进行筛选，然后为新员工制定了全面的入职培训计划和互动演示。你为面试官制作了反馈表，并为新员工制作了调查问卷，以评估和改进招聘流程，并制定了季度绩效评估计划，并用结构化模板跟踪成就和目标。通过利用
> AI
> 驱动的工具，你展示了组织如何自动化人力资源工作流程，提升效率，并确保新员工顺利过渡。
>
> 你已经成功完成了这个实验。请点击“Next \>\>”继续。
