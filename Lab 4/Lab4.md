# Laboratório 4: Desenvolver um sistema multiagente para geração de relatórios de planos de saúde

**Visão Geral**

Neste laboratório, você irá desenvolver um sistema inteligente
multiagente projetado especificamente para automatizar a geração de
relatórios abrangentes de planos de saúde. Esse sistema aproveita o
poder colaborativo de quatro agentes de AI especializados, que trabalham
de forma coordenada para buscar, analisar, gerar e validar documentação
detalhada de seguros de saúde. A arquitetura multiagente demonstra como
agentes autônomos podem trabalhar juntos para executar tarefas complexas
que seriam difíceis de serem realizadas de forma eficiente por um único
agente.

Você irá criar os seguintes 4 agentes de AI:

- **Agente de pesquisa -** Este agente pesquisará um índice do Azure AI
  Search em busca de informações sobre políticas específicas de planos
  de saúde.

- **Agente de Relatórios -** Este agente irá gerar um relatório
  detalhado sobre a apólice do plano de saúde com base nas informações
  retornadas pelo Agente de Pesquisa.

- **Agente de Validação -** Este agente irá validar se o relatório
  gerado atende aos requisitos especificados. No nosso caso,
  certificar-se de que o relatório contém informações sobre exclusões de
  cobertura.

- **Agente Orquestrador -** Este agente irá atuar como um orquestrador
  que gerencia a comunicação entre o Agente de Pesquisa, o Agente de
  Relatórios e o Agente de Validação.

![A diagram of a company AI-generated content may be
incorrect.](./media/image1.png)

A orquestração é uma parte fundamental de sistemas multiagentes, pois os
agentes que criamos precisam ser capazes de se comunicar entre si para
alcançar o objetivo proposto.

Usaremos o Azure AI Agent Service para criar os agentes de pesquisa,
relatório e validação. No entanto, para criar o agente orquestrador,
usaremos o Semantic Kernel. A biblioteca Semantic Kernel oferece
funcionalidade pronta para uso para orquestrar sistemas multiagentes.

**Objetivos do laboratório**

Você executará as seguintes tarefas neste laboratório.

- Tarefa 1: Criar o índice do Azure AI Search

- Tarefa 2: Criar os Agentes de Pesquisa, Relatório e Validação.

## Tarefa 1: Criar o índice do Azure AI Search

Nesta tarefa, você irá criar um **índice do Azure AI Search** para
armazenar representações vetorizadas de documentos de planos de seguro
saúde, permitindo recuperação eficiente para busca e análise orientadas
por AI.

1.  Navegue até o **Azure Portal**, pesquise por **AI Search (1)** e
    selecione o recurso **AI Search (2)** na lista de serviços.

![](./media/image2.png)

2.  This will navigate you to the AI Foundry, within **AI Search** (1),
    click on **Create**(2).

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

3.  No painel **Create a Search service**, informe os seguintes detalhes
    e clique em **Review + Create** (4)

    - Subscription : **Mantenha a assinatura padrão**

    - Resource Group : Selecione **AgenticAI (1)**

    - Service Name : **my-search-service- (2)**

    - Location : **(3)**

![](./media/image4.png)

4.  Na tela **Review + Create**, clique em **Create**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

5.  Aguarde até que a implementação seja concluída e, em seguida, clique
    em **Go to resource**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

6.  No menu esquerdo, navegue até **Keys (1)** na seção **Settings**. Em
    **API Access control**, selecione **Both (2)**.

![](./media/image7.png)

7.  Selecione **Yes** para confirmar a mensagem **Are you sure want to
    update the API Access Control for this search service**.

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image8.png)

8.  No menu esquerdo, navegue até **Identity (1)** em **Settings**. Em
    System-assigned, defina o Status como **On (2)** e clique em **Save
    (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.png)

9.  Selecione **Yes** para confirmar **Enable System assigned managed
    identity**.

![A close-up of a computer error AI-generated content may be
incorrect.](./media/image10.png)

10. No Portal do Azure, pesquise por **Storage accounts (1)** e
    selecione **Storage accounts (2)** na lista de serviços.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

11. Selecione a storage account que começa com **aifoundry**.

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image12.png)

12. Selecione **Access control (IAM) (1)**, depois clique em **Add (2)**
    e, em seguida, selecione **Add role assignment**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

13. Em **Job function roles**, pesquise por **Storage Blob Data Reader
    (1)**, selecione **Storage Blob Data Reader (2)** e clique em **Next
    (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

14. Na página **Add role assignment**:

- Em **Members**, selecione **Managed identity (1)**

- Clique em **Select Members (2)**

- Em **Managed identity**, selecione **search service (1) (3)**

- Em seguida, selecione o serviço de busca **my-search-service- (4)**

- Clique em **Select (5)**

  - 

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.png)

15. Clique em **Review + assign** duas vezes.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image16.png)

16. Navegue até o **Azure OpenAI**, recurso **my-openai-service**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.png)

17. Selecione **Access control (IAM) (1)**, clique em **Add (2)** e
    depois selecione **Add role assignment**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.png)

18. Em **Job function roles**, pesquise por **Cognitive Services OpenAI
    User (1)**, selecione **Cognitive Services OpenAI User (2)** e
    clique em **Next (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.png)

19. Na página **Add role assignment**:

    - Em **Members,** selecione **Managed identity (1)**

    - Clique em **Select Members (2)**

    - Em **Managed identity**, selecione **search service (1) (3)**

    - Em seguida, selecione o serviço de busca **my-search-service-
      (4)**

    - Clique em **Select (5)**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

20. Clique em **Review + assign** duas vezes.

![](./media/image21.png)

21. Navegue até o **Portal do Azure**, pesquise por **Storage account
    (1)** e selecione **Storage account (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

22. Selecione a Storage account que começa com **aifoundryhub**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

23. Clique em **Containers (1)** em Data storage e selecione **+
    Container (2).**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

24. Na página New Container, insira **healthplan (1)** como nome e
    clique em **Create (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

25. Abra o container **healthplan** clicando sobre ele.

![](./media/image26.png)

26. Clique em **Upload (1)** para enviar o arquivo e, em seguida, clique
    em **Browse for files (2)**.

> ![](./media/image27.png)

27. Navegue até C:\LabFiles\Day-1\azure-ai-agents-labs\data **(1)** ,
    selecione **ambos os arquivos PDF (2)** para upload e clique em
    **Open (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.png)

28. Clique em **Upload**..

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.png)

**Observação:** Se for solicitado que você selecione um container
existente, escolha healthplan no menu suspenso.

29. Navegue até o serviço **Azure AI Search** e selecione
    **my-search-service-**.

![](./media/image30.png)

30. Clique em **Import data (new)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

31. Selecione **azure blob storage**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image32.png)

32. Escolha o modelo RAG.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

33. Em Configure your Azure Blob Storage, informe os seguintes dados e
    clique em **Next (5)**:

[TABLE]

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image34.png)

34. Em Vectorize your text, informe os seguintes detalhes e clique em
    **Next (7)**:

[TABLE]

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image35.png)

35. Clique em **Next** duas vezes.

36. Informe **health-plan (1)** como **Objects name prefix** e clique em
    **Create (2)**.

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image36.png)

**Observação**: O envio dos dados para os índices do serviço de busca
pode levar de 5 a 10 minutos.

37. Clique em **Start searching** no pop-up exibido.

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image37.png)

38. Navegue até a página **Overview** (1) do **ai-foundry-project-** e
    clique em **Open in management center** (2).

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

39. Selecione **Connected resources (1)** e clique em **New connection
    (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.png)

40. Digite **Azure AI Search (1)** na barra de pesquisa e selecione
    **Azure AI Search (2)**.

![](./media/image40.png)

41. Clique em **Add connection** para prosseguir.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.png)

## Tarefa 2: Criar os agentes de pesquisa, relatório e validação

Nesta tarefa, você criará os agentes de pesquisa, relatório e validação
para recuperar, gerar e validar relatórios de planos de saúde. Esses
agentes trabalharão juntos para garantir a precisão e a conformidade com
os requisitos. Cada agente desempenha um papel distinto na recuperação,
compilação e validação das informações dos relatórios.

1.  Abra o arquivo **Lab 4 - Develop A Mult-Agent System.ipynb**. O
    notebook **Lab 4 - Develop A Multi-Agent System.ipynb** orienta você
    no desenvolvimento de um sistema multiagente com os agentes de
    pesquisa, relatório, validação e orquestrador para gerar e validar
    relatórios de planos de saúde. Cada agente desempenha uma função
    distinta na recuperação, compilação e garantia da precisão dos
    relatórios.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.png)

2.  Selecione a opção **Select kernel (1)** disponível no canto superior
    direito e escolha **venv (Python 3.x.x) (2)** na lista.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.png)

3.  Execute esta célula para desenvolver um **sistema multiagente** que
    integra o Azure AI Search, o GPT-4o e o Semantic Kernel para a
    execução inteligente de tarefas. Essa configuração permite que
    vários agentes de AI colaborem na recuperação de informações, na
    geração de respostas e no tratamento de consultas complexas.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image44.png)

4.  Execute esta célula para criar o **Agente de Pesquisa**, que
    recupera detalhes do plano de saúde do Azure AI Search usando o
    GPT-4o. Este agente permite a recuperação eficiente de informações
    estruturadas de documentos do plano de saúde.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.png)

5.  Execute esta célula para criar o **Agente de Relatórios**, que gera
    relatórios detalhados sobre planos de saúde usando o GPT-4o. Este
    agente aprimora a documentação, fornecendo insights estruturados,
    detalhes de cobertura e exclusões para vários planos.

![](./media/image46.png)

6.  Execute esta célula para criar o **Agente de Validação**, que
    garante que os relatórios gerados pelo Agente de Relatórios atendam
    aos padrões de qualidade, verificando especificamente se há
    exclusões de cobertura.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.png)

7.  **Criação do sistema multiagente:** ao executar a célula abaixo,
    será exibida uma caixa de chat no topo do VS Code, solicitando que
    você informe o nome de um plano de saúde.

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image48.png)

8.  Como você deve se lembrar, carregamos dois planos de saúde no índice
    de pesquisa. Quando solicitado, digite qualquer um dos seguintes
    planos de saúde na caixa que aparece na parte superior e pressione
    **Enter** para iniciar a execução do sistema multiagente:

    - **Northwind Health Standard**

    - **Northwind Health Plus**1

![](./media/image49.png)

9.  Quando a caixa aparecer na parte superior, digite exit na caixa e
    pressione Enter para interromper o bloco de código em execução.

**Observação:** após a execução bem-sucedida da célula, o seguinte
resultado será exibido:

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

**Resumo**

Neste laboratório, você desenvolveu com sucesso um sistema multiagente
inteligente projetado para automatizar a geração de relatórios
abrangentes de planos de saúde por meio da coordenação de quatro agentes
de AI especializados. Você criou um índice do Azure AI Search para
armazenar documentos de seguro saúde vetorizados e, em seguida, criou um
Agente de Pesquisa para recuperar informações sobre apólices, um Agente
de Relatórios para gerar documentação detalhada, um Agente de Validação
para garantir a conformidade com os requisitos e um Agente Orquestrador
usando o Semantic Kernel para gerenciar a comunicação entre todos os
agentes. Ao executar o sistema multiagente com dados reais de planos de
saúde, você demonstrou como agentes autônomos podem colaborar de forma
eficaz para realizar tarefas complexas que seriam desafiadoras para um
único agente, evidenciando padrões de orquestração de agentes em nível
corporativo para aplicações práticas de negócios.

Parabéns! Você concluiu o laboratório com sucesso.
