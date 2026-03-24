# 实验12：在企业AI代理中实施 Responsible AI 与内容安全

**预计持续时间**：15分钟

**概述**

在该实验室中，参与者探讨了 Responsible AI
在企业级代理系统中的重要性。他们将理解Microsoft如何将负责任的AI原则——如公平、安全、问责和透明度——整合进代理框架和Microsoft
Foundry。参与者还将学习如何在 Microsoft Foundry
门户中直接配置和验证内容安全过滤器，以确保部署的代理能够道德且安全地响应。

**实验室目标**

你将在实验室执行以下任务。

- 任务1：理解 Responsible AI 与内容安全

- 任务2：配置并验证Microsoft Foundry中的内容过滤器

## 任务1：理解 Responsible AI 与内容安全【仅阅读】

在本任务中，您将学习Microsoft的负责任AI原则，并理解其如何应用于Microsoft代理框架和Microsoft
Foundry。Responsible AI
确保智能系统安全、合乎道德且公平地运作，这是在企业环境中部署多智能体解决方案时的关键要求。

什么是 Responsible AI？

1.  Microsoft的 Responsible AI 框架基于六个指导 AI
    系统开发、部署和运行的基础原则:

2.  公平——AI
    系统应公平对待所有个人和群体。对于企业代理来说，这意味着确保决策或响应不会反映或放大人力资源、合规或财务用例中的偏见。

3.  可靠性与安全性——AI模型必须稳定地表现，并优雅地处理故障。代理应返回事实、可验证的信息，避免不安全或误导性输出。

4.  隐私与安全——AI 系统必须保护用户数据和组织信息。代理框架与Azure
    Identity（Entra ID）安全集成，并尊重企业数据边界。

5.  包容性——代理必须设计成赋能所有用户，支持跨语言、地域和背景的无障碍性。

6.  透明度——用户应了解 AI
    决策的过程。代理应尽可能解释其推理，并通过遥测和可观察性提供可追踪的响应。

7.  问责制——人工监督依然是核心。组织必须定义治理结构，以审查和管理AI驱动的结果。

8.  这些原则构成了在企业环境中构建可信且合规的AI代理的基础。

为什么 Responsible AI 在企业代理中如此重要

1.  当多名代理人协作处理敏感话题——如员工政策、财务报销或合规报告时——错误信息、偏见或不当行为的风险会增加。通过嵌入负责任的
    AI 实践，组织能够:

2.  确保代理间通信的一致性和可靠性。

3.  防止有害、歧视性或不安全的输出。

4.  保持符合全球法规（GDPR、HIPAA、ISO 27001等）。

5.  增强用户对 AI 自动化的信任。

6.  Microsoft代理框架通过Microsoft Foundry原生集成 负Responsible
    AI，直接在模型和部署层面提供治理、可追溯性和安全执法。

内容安全与伦理回应过滤

1.  内容安全是Microsoft Responsible AI 基础设施的关键组成部分。

2.  在 Microsoft Foundry
    中，内容安全过滤器会自动检测并阻止多个类别的有害或敏感输出，包括:

    - 仇恨与骚扰

    - 暴力与自残

    - 性内容

    - 敏感或受保护信息（PII）

3.  配置后，这些过滤器能截取用户提示和模型响应，用标准化的安全响应替代不安全内容，确保合规和用户保护，无需修改本地代码。

## 任务2：配置并验证Microsoft Foundry中的内容过滤器

在此任务中，您将在 Microsoft Foundry 中配置内容安全过滤器，以强制执行
Responsible AI
原则。你将为有害或敏感类别设定保护级别，然后通过测试本地代理验证过滤器。

1.  在 Microsoft Foundry 页面，从左侧菜单选择 **Guardrails +
    Controls**，点击从总览中 **Create a custom content filter** 。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

2.  在下一页，保留过滤器的默认名称，点击**“Next**”。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

3.  在** set input filter**
    窗格中，你会注意到Azure已经预设了多个参数，如仇恨、暴力、性内容等。这些基线过滤器默认应用，以确保符合Microsoft的负责任人工智能标准。

4.  现在，将每个类别调整为 **Highest
    Blocking** ，以强制执行最大内容安全，这是企业级代理的关键要求，然后点击“**Next** ”应用配置。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

5.  在下一窗格，将所有类别配置为**Highest
    Blocking**，然后点击**“Next**”。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

6.  在**Deployments**面板中，选择 **gpt-40-mini**
    模型以添加内容过滤器。点击“**Next**”。 

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

> **注意：**如果“**Replacing existing content
> filter**”弹窗会打开，请选择 **Replace。** 

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

7.  在审核面板中，点击 **Create filter**。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

8.  现在自定义内容过滤器已经成功创建。

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

9.  回到之前打开的终端的 Visual Studio Code
    窗格，执行以下命令启动该代理。请提供以下提示以检查内容过滤功能。

- 写一篇针对一群人的冒犯性陈述。

- 告诉我关于一群人有什么仇恨的。

+++python main.py –interactive+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image9.png)

**摘要**

在本实验室中，您探讨了 Responsibe AI
原则如何指导利用Microsoft代理框架进行伦理、合规且值得信赖的AI代理开发。你在
Microsoft Foundry
中配置了内容安全过滤器，自动防止不安全、偏见或不当内容被你的企业代理处理或返回。

你已经成功完成了这个实验。请点击“Next \>\>”继续。
