# Projetar Agentes de AI Escaláveis com Microsoft Foundry e Agent Framework

**Visão Geral**

Neste laboratório prático, realizado ao longo de 3 dias, você irá
projetar e construir agentes de AI escaláveis utilizando o Microsoft
Foundry e o Microsoft Agent Framework. Os participantes começarão
criando seu primeiro Agente de AI por meio do portal do Microsoft
Foundry, onde aprenderão a carregar documentos corporativos de políticas
e ingeri-los no Azure AI Search para preparar uma base de conhecimento
pesquisável. Em seguida, o workshop avança para a criação de sistemas
multiagentes utilizando o Microsoft Agent Framework SDK, no qual
múltiplos agentes especializados colaboram por meio de padrões de
comunicação Agent-to-Agent (A2A). Os alunos irão estender as capacidades
de seus agentes integrando ferramentas externas e fontes de dados usando
o Model Context Protocol (MCP), conectando-se tanto ao Azure AI Search
para recuperação de conhecimento quanto a APIs externas, como o
Freshdesk, para gerenciamento de tíquetes. O treinamento avança para a
implementação de agentes no Microsoft Foundry Agent Service como
soluções persistentes hospedadas na nuvem com gerenciamento de estado e
confiabilidade de nível empresarial. Por fim, os participantes
implementarão padrões avançados de fluxo de trabalho, incluindo sistemas
multiagentes orquestrados com coordenação centralizada e sistemas
baseados em transferência, nos quais as conversas transitam de forma
contínua entre agentes especializados com base na intenção do usuário e
no conhecimento do domínio.

**Objetivos**

Ao final deste laboratório, você será capaz de:

- **Configurar o projeto de AI e executar a conclusão do chat a partir
  do VS Code:** configure um ambiente de desenvolvimento de AI pronto
  para produção criando um projeto no Microsoft Foundry, implementando
  modelos GPT-4 e de embedding, e estabelecendo conexões seguras a
  partir do Visual Studio Code. Você validará a configuração executando
  chamadas de conclusão de chat, garantindo integração contínua entre o
  ambiente local de desenvolvimento e os serviços do Azure AI, com
  autenticação adequada e configuração correta do projeto.

- **Criar um agente de AI para análise de planos de seguro
  saúde:** desenvolva um agente de AI inteligente especializado em
  analisar e visualizar dados de de planos de seguro saúde. Você criará
  um agente capaz de processar informações complexas de benefícios de
  saúde e gerar automaticamente gráficos de barras comparativos,
  demonstrando capacidades essenciais de agentes de AI, incluindo
  interpretação de dados, compreensão de linguagem natural, execução de
  código e geração automatizada de visualizações para suporte à tomada
  de decisão.

- **Desenvolver um sistema colaborativo multiagente:** projete e
  implemente uma arquitetura multiagente avançada, na qual agentes de AI
  especializados trabalham em conjunto para analisar documentos de
  planos de saúde e gerar relatórios abrangentes. Você criará um agente
  de pesquisa para recuperação inteligente de documentos usando o Azure
  AI Search, um agente de relatórios para gerar relatórios analíticos
  detalhados, um agente de validação para garantir a conformidade e a
  precisão e um agente orquestrador para gerenciar a comunicação entre
  os agentes e a coordenação do fluxo de trabalho, apresentando padrões
  de colaboração entre agentes de nível empresarial.

**Pré-requisitos**

Os participantes devem ter:

- **Azure & Cloud Experience** - Familiaridade com o Azure Portal,
  Resource Groups e serviços do Azure AI.

- **Habilidades de Programação** – Conhecimento básico em Python
  (async/await, variáveis de ambiente e chamadas de API).

- **Conceitos de AI** – Compreensão de LLMs, embeddings, RAG
  (Retrieval-Augmented Generation) e prompt engineering.

- **Ferramentas de Desenvolvimento** – Proficiência no uso do Visual
  Studio Code, terminal e Git.

- **Conhecimento em Agent Framework** – Noções básicas sobre
  arquiteturas de agentes, ferramentas e padrões de orquestração.

Explicação dos Componentes

- **Microsoft Foundry:** o Microsoft Foundry é uma plataforma em nuvem
  para desenvolver, implementar e gerenciar agentes de AI corporativos.
  Ele fornece tempo de execução gerenciado do Agent Service,
  gerenciamento centralizado de projetos e monitoramento com Application
  Insights, garantindo confiabilidade, segurança e observabilidade de
  nível corporativo durante todo o ciclo de vida do agente.

- **Microsoft Agent Framework SDK**: SDK oficial em Python para criar
  agentes inteligentes e modulares, substituindo AutoGen e Semantic
  Kernel. Oferece comunicação nativa Agent-to-Agent (A2A), integração
  com Model Context Protocol (MCP) e suporte ao Microsoft Foundry,
  possibilitando sistemas de agentes corporativos prontos para produção
  com uso padronizado de ferramentas.

- **Azure AI Search**: mecanismo de busca baseado em vetores que
  habilita fluxos de Retrieval-Augmented Generation (RAG). Fornece
  recuperação híbrida combinando similaridade vetorial e busca por
  palavras-chave, classificação semântica para maior relevância e
  recursos de indexação de documentos, garantindo que os agentes
  forneçam respostas fundamentadas e factualmente precisas a partir de
  fontes de conhecimento corporativas.

- **Model Context Protocol (MCP)**: interface padronizada que permite
  aos agentes acessar conhecimento externo e ferramentas de forma
  segura. O MCP conecta-se a fontes de dados corporativas, APIs externas
  como Freshdesk e ferramentas customizadas com esquemas estruturados,
  garantindo interações confiáveis, auditáveis e extensíveis para
  sistemas de AI corporativos.

- **Chat Response Agent**: agente sem estado e de uma única interação,
  voltado para desenvolvimento local e testes. Processa solicitações de
  forma independente, sem manter contexto, executando em ambientes
  locais e respondendo imediatamente. Ideal para prototipação de lógica
  central e validação de comportamento antes da evolução para agentes
  persistentes em produção.

- **Persistent Agent**: serviço persistente e de longa duração,
  hospedado na nuvem no Microsoft Foundry, que mantém o status ao longo
  das conversas. Ele oferece suporte à integração de ferramentas
  externas por meio do MCP, colaboração entre agentes e confiabilidade
  em escala empresarial com monitoramento integrado, fornecendo bases
  para aplicativos de produção que exigem experiências conversacionais
  com status e múltiplas interações.

- **Planner Agent**: um orquestrador inteligente que analisa as
  consultas dos usuários para direcioná-las aos agentes especializados
  apropriados. Utiliza raciocínio de AI e heurísticas de palavras-chave
  para classificar solicitações em domínios como RH, Finanças ou
  Conformidade, garantindo distribuição otimizada de tarefas e atuando
  como ponto central de coordenação.

- **Worker Agents**: especialistas em domínios com experiência em áreas
  específicas, como RH, Finanças ou Conformidade. Cada agente tem
  instruções específicas do domínio, ferramentas especializadas e fontes
  de conhecimento relevantes. Eles colaboram com o Planner Agent por
  meio de comunicação A2A, fornecendo respostas autoritativas e precisas
  para consultas complexas.

- **Azure OpenAI**: serviço corporativo que fornece acesso a LLMs
  avançados por meio de endpoints de API seguros. Ele oferece conclusão
  de chat, modelos de embedding, filtragem de conteúdo e recursos de
  conformidade. Integra-se de forma nativa ao Microsoft Foundry,
  permitindo que agentes utilizem GPT-4 mantendo privacidade de dados e
  controles de governança.

# Laboratório 5: Criando um Agente de AI com Retrieval Augmented usando o Microsoft Foundry

**Visão Geral**

Neste laboratório, você criará seu primeiro Agente de AI usando o portal
Microsoft Foundry. Você começará carregando documentos de políticas
corporativas e ingerindo-os no Azure AI Search para preparar uma base de
conhecimento. Em seguida, configurará o agente usando o Microsoft Agent
Framework para habilitar retrieval augmented generation (RAG). Por fim,
você testará as respostas do agente e analisará os logs de execução para
observar como ele recupera e processa informações.

**Objetivos do laboratório**

Neste laboratório, você realizará as seguintes tarefas:

- Tarefa 1: Criar os recursos do Azure

- Tarefa 2: Criar um Agente de AI no Microsoft Foundry

- Tarefa 3: Conectar o Azure AI Search para RAG

- Tarefa 4: Testar e observar os logs de execução do agente

## Tarefa 1: Criar os recursos do Azure

Nesta tarefa, você criará todos os recursos do Azure necessários para
executar este laboratório.

### Tarefa 1.1: Criar uma conta de armazenamento

1.  Faça login no portal do Azure em +++https://portal.azure.com+++
    usando as credenciais abaixo e selecione Storage accounts.

- Username - +++@lab.CloudPortalCredential(User1).Username+++

- TAP - <+++@lab.CloudPortalCredential(User1).TAP>+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image1.png)

2.  Selecione **Create**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

3.  Insira os detalhes abaixo e selecione **Review + create**. Na
    próxima tela, selecione **Create**.

- Storage account name - +++aistorage@lab.LabInstance.Id+++

- Preferred storage type – Selecione **Azure Blob Storage or Azure Data
  Lake Storage Gen2**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image3.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image4.png)

4.  Após a criação do recurso, selecione **Go to resource**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

5.  Selecione **Upload** e, em seguida, selecione **Create new** para
    criar um novo contêiner. Nomeie-o como **+++datasets+++** e depois
    selecione **Ok**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

![A screenshot of a login box AI-generated content may be
incorrect.](./media/image7.png)

6.  Selecione **Browse for files**, escolha os arquivos de política
    localizados em **C:\Labfiles\Day 2** e clique em **Upload**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

![A screenshot of a upload box AI-generated content may be
incorrect.](./media/image9.png)

Agora, a conta de armazenamento foi criada com sucesso e carregada com
os documentos da política.

### Tarefa 1.2: Criar o recurso Foundry

Nesta tarefa, você criará um recurso Foundry, que é necessário para
acessar o Microsoft Foundry.

1.  Na página inicial do portal do Azure
    (+++https://portal.azure.com+++), selecione **Foundry**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.png)

2.  Selecione **Foundry** no painel esquerdo e, em seguida, selecione
    **Create** para criar o recurso Foundry.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

3.  Insira os detalhes abaixo e selecione **Review + create**.

- Name – <+++agentic-@lab.LabInstance.Id>+++

- Default project name – <+++agentic-ai-project-@lab.LabInstance.Id>+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.png)

4.  Após a validação, selecione **Create**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

5.  Certifique-se de que o recurso foi criado com sucesso.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

6.  Abra o **<agentic-ai-project-@lab.LabInstance.Id>** e selecione **Go
    to Foundry portal**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.png)

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image16.png)

7.  No Microsoft Foundry, selecione Models + endpoints no painel
    esquerdo. Em seguida, selecione **+ Deploy model** → **Deploy base
    model**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.png)

8.  Pesquise por +++gpt-4o-mini+++, selecione-o e clique em Confirm para
    implementar o modelo.

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image18.png)

9.  Selecione **Deploy** na janela de implementação.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.png)

10. Da mesma forma, pesquise por +++text-embedding-ada-002+++ e realize
    a implementação do modelo.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image21.png)

Nesta tarefa, você implementou com sucesso o recurso Foundry e
implementou um modelo de chat e um modelo de embeddings nele.

### Tarefa 1.3: Criar o Application Insights

Nesta tarefa, você irá implementar um recurso Application Insights, que
é necessário para monitoramento.

1.  Na página inicial do portal do Azure, selecione **Subscriptions** e
    escolha a assinatura atribuída.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

2.  Selecione **Resource providers** no painel esquerdo.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

3.  Pesquise por +++Operational+++, selecione os 3 pontos ao lado de
    **Microsoft.OperationalInsights** e clique em **Register**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

4.  No painel esquerdo do Microsoft Foundry, selecione **Monitoring**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

5.  Selecione **Create New**, forneça o nome
    <+++agent-insights-@lab.LabInstance.Id>+++ e, em seguida, selecione
    **Create**.

![A screenshot of a application AI-generated content may be
incorrect.](./media/image27.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.png)

Nesta tarefa, você implementou o recurso Application Insights.

### Tarefa 1.4: Criar o recurso de Search

Antes que um Agente de AI possa responder com precisão a perguntas
corporativas, ele precisa acessar fontes de dados confiáveis. O Azure AI
Search possibilita Retrieval-Augmented Generation (RAG) ao indexar
documentos como políticas, contratos e manuais. Um índice funciona como
um catálogo pesquisável que divide o conteúdo em partes menores,
adiciona metadados e permite que o agente recupere as informações
corretas durante uma conversa.

Nesta tarefa, você irá implementar a indexação dos documentos carregados
usando o Azure AI Search para criar uma base de conhecimento
pesquisável.

1.  Na página inicial do portal do Azure, selecione **Foundry**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.png)

2.  Selecione **AI Search** no painel esquerdo e, em seguida, selecione
    **+ Create.**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

3.  Insira os detalhes abaixo e selecione **Review + create**.

- Service name - +++ai-knowledge-@lab.LabInstance.Id+++

- Region - East US2

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image32.png)

4.  Após a validação, selecione **Create**. Quando o recurso for criado,
    selecione Go to resource.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image34.png)

5.  Selecione **Import data (new)**.

![A screenshot of a search engine AI-generated content may be
incorrect.](./media/image35.png)

6.  Em **Choose data source**, selecione **Azure Blob Storage**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.png)

7.  No próximo painel, selecione a opção **RAG**, pois você está
    implementando um agente baseado em recuperação.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image37.png)

> Veja o que cada uma dessas opções representa:

1.  **Keyword Search:** usada para experiências de pesquisa tradicionais
    baseadas em palavras-chave exatas. Indexa texto para que os usuários
    encontrem informações por correspondência de palavras-chave, sem
    raciocínio de AI.

2.  **RAG (Retrieval-Augmented Generation):** combina recuperação de
    documentos com geração por AI. Ingere texto (e imagens simples via
    OCR) para que um agente de AI forneça respostas fundamentadas e
    sensíveis ao contexto.

3.  **Multimodal RAG:** Estende o RAG para lidar com conteúdo visual
    complexo, como diagramas, tabelas, fluxos de trabalho ou gráficos.
    Permite que a AI interprete texto e elementos visuais para respostas
    mais ricas e baseadas em insights.

&nbsp;

8.  Selecione <aistorage@lab.LabInstance.Id> em **Storage account** e
    **datasets** em **Blob container**, depois selecione **Next**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

9.  Selecione os detalhes abaixo e clique em **Next**.

- Kind – Azure AI Foundry (Preview)

- Azure AI Foundry/Hub project –
  <agentic-ai-project-@lab.LabInstance.Id>

- Model deployment – text-embedding-002-ada

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.png)

10. Selecione **Next** nas telas seguintes até que a tela **Review and
    create** seja exibida.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image40.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.png)

11. Selecione **Create** na tela **Review and create**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.png)

12. Selecione **Close** na janela Create succeeded.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.png)

Você implementou com sucesso a ingestão do conjunto de dados no Azure AI
Search e criou um índice pesquisável. Na próxima tarefa, você irá
implementar um Agente de AI e conectar esse índice como sua fonte de
conhecimento.

## Tarefa 2: Criar um Agente de AI no Microsoft Foundry

Nesta tarefa, você irá implementar um novo Agente de AI no Microsoft
Foundry e configurar seu propósito principal, instruções e modelo usando
a interface do Microsoft Agent Framework.

1.  Volte ao seu resource group e, na lista de recursos, selecione o
    recurso **agentic-foundry**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.png)

2.  No painel seguinte, clique em **Go to Foundry portal**. Você será
    redirecionado para o Microsoft Foundry portal, onde irá implementar
    seu primeiro agente.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.png)

3.  No Portal do Foundry, selecione **Agents (1)** no menu esquerdo.
    Você já verá um agente **pré-criado**. Caso não exista, clique em
    **+ New agent (2)** para criá-lo.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image46.png)

4.  Selecione o **agente** recém-criado. Um painel de configuração será
    aberto à direita. Forneça os seguintes detalhes:

[TABLE]

> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image47.png)

5.  Você implementou com sucesso um agente no Microsoft Foundry. Em
    seguida, você irá enriquecê-lo com conhecimento conectando os dados
    indexados na próxima tarefa.

## Tarefa 3: Conectar o Azure AI Search para RAG

Nesta tarefa, você irá implementar a integração do Azure AI Search com o
agente usando o painel de integração de conhecimento, habilitando
respostas baseadas em retrieval augmented por meio do MCP (Model Context
Protocol).

1.  No mesmo painel de configuração do agente, role para baixo e clique
    em **+ Add** no parâmetro **Knowledge**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image48.png)

2.  No painel **Add knowledge**, selecione **Azure AI Search**, pois o
    índice já foi implementado no recurso de AI Search.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image49.png)

3.  No próximo painel, em **Azure AI Search resource connection**,
    clique na **seta do menu suspenso (1)** e selecione **Connect other
    Azure AI Search resource (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.png)

4.  No painel seguinte, confirme que o recurso correto de AI Search está
    selecionado e clique em **Add connection**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image51.png)

5.  Na etapa **Adding Azure AI Search**, configure os detalhes abaixo e
    clique em **Connect (5)** ao concluir.

[TABLE]

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image52.png)

6.  O agente agora foi enriquecido de forma bem-sucedida com
    conhecimento usando o índice do Azure AI Search, que atua como uma
    base de conhecimento pesquisável para recuperar informações precisas
    durante as conversas.

## Tarefa 4: Testar e observar os logs de execução do agente

Nesta tarefa, você irá testar o agente fazendo perguntas relacionadas a
políticas e revisar logs estruturados para verificar o uso de
ferramentas, chamadas de pesquisa e respostas fundamentadas.

1.  Antes de testar o agente, conecte o Application Insights para
    habilitar logs detalhados e visibilidade de rastreamento.

2.  No **Microsoft Foundry portal**, selecione **Monitoring (1)** no
    menu esquerdo, selecione **agent-insights- (2)** e clique em
    **Connect (3)**.

![](./media/image53.png)

3.  Em seguida, selecione **Agents (1)** no menu esquerdo, escolha o
    agente **EnterpriseAssistant (2)** e clique em **Try in playground
    (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image54.png)

4.  Um painel de chat será aberto, onde você poderá inserir seus
    prompts. O agente agora responderá usando os documentos e conjuntos
    de dados que você implementou.

Exemplos de prompts:

- +++What is the employee travel reimbursement policy?+++

- +++Summarize the contract approval rules and cite the document.+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image55.png)

5.  Após o agente responder às perguntas, clique em **Thread logs** no
    menu superior para verificar os logs e rastreamentos da thread
    atual.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image56.png)

6.  Explore e revise as métricas, rastreamentos e avaliações, que
    apresentam uma visão detalhada da execução do agente.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image57.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image58.png)

7.  Em seguida, navegue até o painel de **Monitoring**, onde você
    conectou o Application Insights, selecione a aba **Resource usage**
    e revise todas as métricas e valores.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image59.png)

8.  Você implementou com sucesso um agente baseado em RAG alimentado por
    conjuntos de dados corporativos curados. A seguir, você avançará
    para habilitar colaboração multiagente, onde agentes podem delegar,
    raciocinar e trabalhar juntos de forma inteligente.

Resumo

Neste laboratório, você implementou seu primeiro Agente de AI no
Microsoft Foundry e o conectou a uma base de conhecimento indexada. Você
carregou documentos, implementou a ingestão no Azure AI Search e
habilitou RAG por meio da integração com o Microsoft Agent Framework. Ao
testar o agente e revisar os logs de execução, você obteve experiência
prática em como agentes recuperam informações fundamentadas e geram
respostas prontas para ambientes corporativos.
