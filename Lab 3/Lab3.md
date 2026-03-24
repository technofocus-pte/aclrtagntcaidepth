# 实验3：构建健康保险计划分析器AI代理

**概述**

在本实验室中，您将构建一个专门用于处理和分析健康保险计划数据的健康保险计划分析器AI代理。该智能代理通过解读计划细节、分析保障选项并生成支持决策的可视化表示，自动比较不同健康福利计划。利用Microsoft
Foundry和Azure
AI服务，客服人员将创建对比条形图，清晰显示保险计划之间的差异，帮助用户更轻松地了解选项并选择最合适的健康保险。

**实验室目标**

你将在实验室完成以下任务。

- 任务一：创建一个简单的AI代理

## 任务一：创建一个简单的AI代理

在此任务中，您将构建一个简单的AI代理，处理数据并生成条形图，利用Azure
AI服务比较不同健康福利计划进行分析和可视化。

1.  打开 **实验2 - 创建一个简单的 AI agent.ipynb** 文件。本 **实验2 -
    创建一个简单的 AI agent.ipynb**
    笔记本引导你构建一个简单的AI代理，它能处理数据并生成比较不同健康福利计划的条形图。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

2.  选择右上角可选的**“ Select kernel **”设置。从列表中选择
    **venv（Python 3.x.x）。**

![A blue and red rectangle with white text AI-generated content may be
incorrect.](./media/image2.png)

3.  运行下面的单元格导入必要的库并加载环境变量，以便使用 Azure AI
    项目。这种设置实现了与Azure AI服务的安全认证和交互。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image3.png)

4.  运行下面的单元，连接到你的 Microsoft Foundry 项目，访问已部署的
    **gpt-4o** 模型。这通过项目连接字符串和 Azure 凭证建立安全连接。

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image4.png)

5.  运行该单元格创建一个 **simple AI Agent**，处理数据并生成条形图，使用
    Microsoft Foundry 比较不同的健康福利计划。该脚本初始化 AI
    代理，发送包含健康计划数据的提示，并请求条形图。代理处理请求，生成图表，保存图像文件，然后通过删除代理进行清理。

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image5.png)

6.  最后观察输出。

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image6.png)

**摘要**

在本实验室中，您成功地利用 Microsoft Foundry 和 Azure AI
服务构建了健康保险计划分析器 AI
代理，实现了健康福利计划的分析与比较。你学会了如何连接你的Microsoft
Foundry项目，访问已部署的GPT-4o模型，并创建一个能够处理复杂健康保险计划数据的智能代理。代理人解读计划细节，分析保障选项，并自动生成比较条形图，以可视化保险计划之间的差异。这次亲身体验展示了
AI
代理如何简化数据分析、支持决策过程，并帮助用户轻松理解和比较健康保险选项。

你已经成功完成了这个实验。请点击“Next \>\>”继续。
