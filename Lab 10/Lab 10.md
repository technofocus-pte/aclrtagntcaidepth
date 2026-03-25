# Gerenciar, Proteger e Monitorar Agentes de AI em Escala

**Visão Geral**

Este laboratório prático tem como foco o gerenciamento, a segurança e o
monitoramento de agentes de AI em escala, utilizando o Azure AI Agent
Service SDK e o Microsoft Foundry. Os participantes irão se aprofundar
em práticas de nível de produção essenciais para implantações
corporativas de AI, começando por AgentOps — a disciplina de observar e
governar agentes de AI por meio da integração com OpenTelemetry e Azure
Application Insights. O workshop enfatiza a importância da AI
Responsável, implementando os seis princípios fundamentais da Microsoft,
incluindo equidade, confiabilidade, privacidade e responsabilidade, por
meio de filtros configuráveis de Segurança de Conteúdo que detectam e
bloqueiam saídas prejudiciais, como discurso de ódio, violência e
informações sensíveis. Além disso, os participantes irão construir
fluxos de trabalho sofisticados de human-in-the-loop (HITL),
exemplificados por um sistema de detecção de fraude no qual agentes
especializados de AI analisam atividades suspeitas e direcionam de forma
inteligente casos de alto risco para analistas humanos, apoiando a
tomada de decisões críticas. Ao longo do laboratório, você trabalhará
com sistemas multiagente que colaboram em tarefas de recuperação,
validação e orquestração, adquirindo experiência prática com
rastreamento de ponta a ponta, visualização de métricas personalizadas,
painéis de monitoramento de desempenho e gerenciamento de fluxos de
trabalho em tempo real. Ao final deste workshop, os participantes terão
dominado as habilidades essenciais para implantar, monitorar e governar
agentes de AI em ambientes corporativos, garantindo que operem de forma
segura, ética e eficiente em escala, mantendo a conformidade com
políticas organizacionais e requisitos regulatórios.

**Objetivos**

Ao final deste laboratório, você será capaz de:

- **Habilitar observabilidade e monitoramento:** implementar
  rastreamento de ponta a ponta e telemetria para agentes de AI usando
  OpenTelemetry integrado ao Azure Application Insights, capturando
  comportamento dos agentes, métricas de desempenho e rastreamentos de
  execução.

- **Visualizar métricas do agente:** criar painéis e workbooks
  personalizados no Application Insights para monitorar o desempenho do
  agente, os tempos de resposta, o uso de tokens, a precisão do
  roteamento e a integridade do sistema em tempo real.

- **Implementar práticas responsáveis de AI:** configurar filtros de
  segurança de conteúdo no Microsoft Foundry para detectar e bloquear
  resultados prejudiciais (discurso de ódio, violência, conteúdo
  sensível) e garantir um comportamento ético e compatível da AI.

- **Criar fluxos de trabalho com intervenção humana:** projetar e
  implementar sistemas de detecção de fraudes nos quais agentes de AI
  analisam alertas e encaminham casos de alto risco a analistas humanos
  para revisão e tomada de decisão.

- **Monitorar sistemas multiagentes:** acompanhar a comunicação entre
  agentes, rastreie fluxos de trabalho distribuídos entre vários agentes
  especializados e identifique gargalos ou falhas em orquestrações
  complexas de agentes.

Explicação dos componentes

- **Microsoft Foundry:** plataforma baseada em nuvem para
  desenvolvimento, implantação e gerenciamento de modelos de AI, com
  recursos centralizados de governança, observabilidade e conformidade
  para aplicações corporativas de AI.

- **Azure AI Hub:** recurso de nível superior no Azure que fornece um
  ambiente centralizado, seguro e colaborativo para equipes criarem,
  gerenciarem e implementarem aplicações de AI com recursos
  compartilhados e políticas de governança.

- **Azure AI Search:** serviço de pesquisa baseado em vetores que
  possibilita Retrieval-Augmented Generation (RAG) por meio da indexação
  e recuperação de documentos relevantes, aprimorando as respostas
  geradas por AI com informações fundamentadas.

- **Azure AI Services:** conjunto de serviços de AI baseados em nuvem
  que oferecem APIs e modelos pré-criados e personalizáveis para
  recursos de visão, linguagem, fala e tomada de decisão.

- **OpenTelemetry:** padrão aberto para rastreamento distribuído,
  métricas e logs, integrado nativamente ao Microsoft Agent Framework
  para capturar rastreamentos de execução dos agentes, métricas de
  desempenho e monitoramento de erros.

- **Filtros de Segurança de Conteúdo:** sistema de filtragem integrado
  ao Microsoft Foundry que detecta e bloqueia automaticamente saídas
  prejudiciais em categorias como discurso de ódio, violência, conteúdo
  sexual e sensitive information (PII).

- **LLMs e Embeddings:** Large Language Models (LLMs) fornecem
  compreensão e geração de linguagem natural, enquanto embeddings são
  representações vetoriais usadas para similaridade de texto, pesquisa e
  recuperação de conhecimento em aplicativos de AI.

# Laboratório 10: Pré-requisitos – Configuração do Índice de Conhecimento e do Sistema de Tíquetes

**Duração Estimada:** 30 minutos

**Visão Geral**

Neste laboratório de pré-requisitos, você irá configurar os componentes
fundamentais necessários para um fluxo de trabalho orientado por AI
capaz de recuperar conhecimento corporativo e criar tíquetes de suporte
automaticamente. O foco está na preparação de uma base de conhecimento
pesquisável, na habilitação de agentes de AI para consultar esse
conhecimento usando uma ferramenta MCP (Model Context Protocol) e na
integração de um sistema de tíquetes para ações posteriores.

Ao concluir essas tarefas, você estabelecerá a infraestrutura principal
que permite aos agentes:

- Recuperar informações relevantes a partir de dados indexados

- Utilizar essas informações de forma contextual durante conversas ou
  fluxos de trabalho

- Escalar problemas por meio da criação de tíquetes em um serviço
  externo

Essa configuração garante que os laboratórios subsequentes sejam
executados de forma fluida e reflitam um cenário corporativo do mundo
real.

Objetivos do Laboratório

Você executará as seguintes tarefas neste laboratório:

- Tarefa 1: Preparar o Índice de Conhecimento

- Tarefa 2**:** Configurar o Freshworks para Gerenciamento de Tíquetes

## Tarefa 1: Criar os recursos do Azure

Nesta tarefa, você irá criar todos os recursos do Azure que são
necessários para executar este laboratório.

### Tarefa 1.1: Criar uma conta de armazenamento

1.  Faça login no Azure Portal em +++https://portal.azure.com+++ usando
    as credenciais abaixo e selecione Storage accounts.

- Username - +++@lab.CloudPortalCredential(User1).Username+++

- TAP - <+++@lab.CloudPortalCredential(User1).TAP>+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image1.png)

2.  Selecione **Create**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

3.  Insira os detalhes abaixo e selecione **Review + create**. Na tela
    seguinte, selecione Create.

- Storage account name - +++aistorage@lab.LabInstance.Id+++

- Preferred storage type – Selecione **Azure Blob Storage** ou **Azure
  Data Lake Storage Gen2**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image3.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image4.png)

4.  Após o recurso ser criado, selecione **Go to resource**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

5.  Selecione **Upload**, escolha **Create new** para criar um novo
    contêiner. Nomeie-o como +++**datasets**+++ e, em seguida, selecione
    **Ok**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

![A screenshot of a login box AI-generated content may be
incorrect.](./media/image7.png)

6.  Selecione **Browse for files**, escolha os arquivos de política em
    **C:\Labfiles\Day 2** e clique em **Upload**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

![A screenshot of a upload box AI-generated content may be
incorrect.](./media/image9.png)

Agora, a storage account foi criada com sucesso e carregada com os
documentos de política.

### Tarefa 1.2: Criar recurso do Foundry

Nesta tarefa, você criará um recurso do Foundry, que é necessário para
acessar o Microsoft Foundry.

1.  Na página inicial do portal do Azure
    (+++https://portal.azure.com+++), selecione **Foundry**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.png)

2.  Selecione **Foundry** no painel esquerdo e, em seguida, selecione
    **Create** para criar o recurso do Foundry.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

3.  Insira os detalhes abaixo e selecione **Review + create**.

- Name – <+++agentic-@lab.LabInstance.Id>+++

- Default project name – <+++agentic-ai-project-@lab.LabInstance.Id>+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.png)

4.  Selecione **Create** após a validação.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

5.  Verifique se o recurso foi criado.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

6.  Abra **<agentic-ai-project-@lab.LabInstance.Id>** e selecione **Go
    to Foundry portal**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.png)

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image16.png)

7.  No Microsoft Foundry, selecione Models + endpoints no painel
    esquerdo. Selecione **+ Deploy model** → **Deploy base model**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.png)

8.  Pesquise por +++gpt-4o-mini+++, selecione-o e clique em Confirm para
    implementar o modelo.

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image18.png)

9.  Selecione **Deploy** na janela de implementação.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.png)

10. Da mesma forma, pesquise por +++text-embedding-ada-002+++ e
    implemente-o.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image21.png)

Nesta tarefa, você criou com sucesso o recurso do Foundry e implementou
um modelo de chat e um modelo de embedding nele.

### Tarefa 1.3: Criar Application Insights

Nesta tarefa, você criará um recurso do Application Insights, que é
necessário para monitoramento.

1.  Na página inicial do portal do Azure, selecione **Subscriptions** e
    selecione a assinatura atribuída.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

2.  Selecione **Resource providers** no painel esquerdo.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

3.  Pesquise por +++Operational+++, selecione os três pontos ao lado de
    **Microsoft.OperationalInsights** e clique em **Register**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

4.  No painel esquerdo do Microsoft Foundry, selecione **Monitoring**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

5.  Selecione **Create New** → forneça o nome como
    <+++agent-insights-@lab.LabInstance.Id>+++ e, em seguida, selecione
    **Create**.

![A screenshot of a application AI-generated content may be
incorrect.](./media/image27.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.png)

Nesta tarefa, você criou o recurso do Application Insights.

### Tarefa 1.4: Criar recurso de Search

Antes que um Agente de AI possa responder com precisão a perguntas
corporativas, ele precisa acessar fontes de dados confiáveis. O Azure AI
Search permite a Retrieval-Augmented Generation (RAG) ao indexar
documentos como políticas, contratos e manuais. Um índice funciona como
um catálogo pesquisável que divide o conteúdo em partes, adiciona
metadados e permite que o agente recupere as informações corretas
durante uma conversa.

Nesta tarefa, você irá indexar os documentos carregados usando o Azure
AI Search para criar uma base de conhecimento pesquisável.

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

4.  Selecione **Create** após a validação ser concluída. Selecione Go to
    resource quando o recurso for criado.

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

7.  No painel seguinte, selecione a opção **RAG**, pois estamos criando
    um agente baseado em recuperação.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image37.png)

> Aqui está o que cada uma dessas opções significa:

1.  **Keyword Search:** usado para experiências de pesquisa tradicionais
    baseadas em palavras-chave exatas. Ele indexa o texto para que os
    usuários possam encontrar informações por meio da correspondência de
    palavras-chave, sem raciocínio de AI.

2.  **RAG (Retrieval-Augmented Generation):** combina a recuperação de
    documentos com a geração por AI. Ele ingere texto (e imagens simples
    via OCR) para que um agente de AI possa fornecer respostas
    fundamentadas e com contexto.

3.  **Multimodal RAG:** Estende o RAG para lidar com conteúdo visual
    complexo, como diagramas, tabelas, fluxos de trabalho ou gráficos.
    Ele permite que a AI interprete tanto elementos textuais quanto
    visuais para fornecer respostas mais ricas e baseadas em insights.

&nbsp;

8.  Em **Storage account**, selecione <aistorage@lab.LabInstance.Id> e,
    em **Storage account** e **datasets** em **Blob container**, e
    depois selecione **Next**.

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

12. Selecione **Close** na caixa de diálogo Create succeeded.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.png)

Você ingeriu com sucesso o conjunto de dados no Azure AI Search e criou
um índice pesquisável. Na próxima tarefa, você criará um agente de AI e
conectará esse índice como sua fonte de conhecimento.

# Tarefa 2: Configurar o Freshworks para gerenciamento de tíquetes

Nesta tarefa, você irá configurar o Freshworks para habilitar o
gerenciamento de tíquetes e a integração corporativa para seu sistema
multiagente.

O **Freshworks** é uma plataforma de atendimento e engajamento ao
cliente baseada em nuvem, projetada para aprimorar as operações de
suporte e aumentar a satisfação dos usuários. Ela oferece um conjunto de
ferramentas para gerenciamento de tíquetes, chat ao vivo, criação de
central de ajuda e autoatendimento ao cliente. O Freshworks oferece
suporte à comunicação omnichannel, permitindo que as empresas gerenciem
interações com clientes por e-mail, chat, telefone e redes sociais a
partir de uma interface centralizada. Seus recursos de automação ajudam
a simplificar fluxos de trabalho, atribuir tíquetes e fornecer análises
para acompanhamento de desempenho. Agora, você irá configurar a conta do
Freshworks.

.

1.  Copie a URL e cole-a em uma nova aba do navegador dentro da VM para
    abrir o portal do **Freshworks**.

    - URL:

> +++https://www.freshworks.com/freshdesk/lp/home/?tactic_id=3387224&utm_source=google-adwords&utm_medium=FD-Search-Brand-India&utm_campaign=FD-Search-Brand-India&utm_term=freshdesk&device=c&matchtype=e&network=g&gclid=EAIaIQobChMIuOK90qvLjQMV_dQWBR3JAi9VEAAYASAAEgK87_D_BwE&audience=kwd-30002131023&ad_id=282519464145&gad_source=1&gad_campaignid=671502402+++

2.  No portal, selecione **Start free trial** para iniciar o período de
    avaliação gratuita.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.png)

3.  No painel seguinte, forneça os detalhes solicitados e clique em
    **Try it free (6)**:

    - **First name:** LODS

    - **Last name:** User1

    &nbsp;

    - **Work
      email:** **+++@lab.CloudPortalCredential(User1).Username+++**

    &nbsp;

    - **Company name:** Zava

    - **Organization size:** Selecione **1-10**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image45.png)

4.  No painel seguinte, forneça os detalhes solicitados e clique em
    **Next (4)**:

    - **What industry are you from?:** na lista, selecione **Software
      and internet (1)**

    - **How many employees are there in your company?:** selecione
      **1-10 (2)**

    - Selecione **I'm trying customer service software for the first
      time (3)**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image46.png)

5.  Após concluir, copie a URL fornecida e cole-a em uma nova aba do
    navegador dentro da VM para abrir o **Outlook.**

    - URL:

> +++https://go.microsoft.com/fwlink/p/?LinkID=2125442&clcid=0x409&culture=en-us&country=us+++

6.  No painel Pick an account, selecione a conta que foi atribuída para
    este laboratório.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.png)

7.  No e-mail de verificação do Freshworks, abra a mensagem e clique em
    **Activate Account**.

> ![A screenshot of a computer screen AI-generated content may be
> incorrect.](./media/image48.png)

**Observação:** Caso não consiga localizar o e-mail de ativação do
Freshworks, aguarde alguns minutos, pois pode haver um atraso na entrega
do e-mail. Se o e-mail não chegar após algum tempo, considere reiniciar
as etapas para ativar o período de avaliação gratuita em uma nova janela
privada/anônima. Além disso, verifique as pastas de spam ou lixo
eletrônico, pois o e-mail pode ter sido filtrado para lá.

8.  No painel seguinte, informe o valor em **Enter password (1)** e
    forneça a mesma senha em **Confirm password (2)**. Em seguida,
    clique em **Activate your account (3).**

> ![A screenshot of a login screen AI-generated content may be
> incorrect.](./media/image49.png)

9.  Após acessar o portal, clique no ícone **Profile (1)** no canto
    superior direito e selecione **Profile settings (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.png)

10. Na página de perfil, clique em **View API Key** para obter as API
    Keys.

![A screenshot of a web page AI-generated content may be
incorrect.](./media/image51.png)

**Observação:** caso não consiga localizar essa opção, reduza o tamanho
da tela usando **CTRL + -**.

11. No painel seguinte, conclua o **CAPTCHA**.

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image52.png)

12. Copie a API Key para um Bloco de Notas, pois você a utilizará
    posteriormente.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image53.png)

13. Na aba do navegador, copie a **Account URL** conforme exibido e cole
    o valor no Bloco de Notas. Você usará essa informação
    posteriormente.

![](./media/image54.png)

**Resumo**

Ao concluir este laboratório de pré-requisitos, você configurou a base
essencial para um fluxo de trabalho de agentes de ponta a ponta. Você
preparou um índice de conhecimento pesquisável, habilitou os agentes a
consultar esses dados por meio de uma ferramenta MCP construída sobre o
**Azure AI Search** e integrou o **Freshworks** para o gerenciamento
automatizado de tíquetes.

Essa base garante que os agentes possam recuperar contexto preciso,
tomar decisões bem fundamentadas e escalar problemas de forma eficiente,
preparando o ambiente para cenários mais avançados orientados por
agentes nos próximos laboratórios.

Você concluiu este laboratório com sucesso. Clique em Next \>\> para
prosseguir.
