# Laboratório 11: AgentOps – Observabilidade e gerenciamento

**Duração estimada:** 60 minutos

**Visão geral**

Neste laboratório, você se concentrará no AgentOps, a disciplina de
monitoramento, governança e gerenciamento de agentes de AI em ambientes
de produção. Você explorará como habilitar a observabilidade e a
telemetria usando a integração incorporada do Microsoft Agent Framework
com o Application Insights usando o **OpenTelemetry**.

Sobre o OpenTelemetry no Microsoft Agent Framework

O Microsoft Agent Framework integra-se nativamente ao OpenTelemetry, o
padrão aberto para rastreamento distribuído, métricas e logs. Essa
integração fornece visibilidade completa do comportamento dos agentes ao
capturar automaticamente dados de telemetria, como rastreamentos de
spans, chamadas de ferramentas, respostas do modelo e desempenho dos
fluxos de trabalho. Com essa integração, os desenvolvedores podem
exportar dados de observabilidade diretamente para o Azure Monitor,
Application Insights ou qualquer outro backend compatível com
OpenTelemetry. Essa abordagem padronizada ajuda a rastrear cada ação dos
agentes em sistemas multiagente complexos, permitindo otimização de
desempenho, solução de problemas e auditoria de conformidade com
configuração mínima.

Objetivos do laboratório

Você executará as seguintes tarefas neste laboratório:

- Tarefa 1: habilitar a observabilidade do agente com o OpenTelemetry

- Tarefa 2: visualizar as métricas do agente

- Tarefa 3: monitorar métricas específicas do agente no Portal do
  Foundry

## Tarefa 1: habilitar a observabilidade do agente com o OpenTelemetry

Nesta tarefa, você integrará a observabilidade do OpenTelemetry e do
Agent Framework ao seu projeto. Você configurará exportadores de
telemetria, inicializará o rastreamento com setup_observability() e
capturará intervalos detalhados para cada estágio do seu fluxo de
trabalho, incluindo roteamento do agente, recuperação do Azure AI Search
e criação de tíquetes. Isso permite visibilidade unificada do
comportamento do agente e correlação entre sistemas usando IDs de
rastreamento no Application Insights.

1.  Em vez de modificar novamente o código anterior, você trabalhará em
    uma nova pasta que já contém os arquivos atualizados com
    observabilidade habilitada. Compreenda como telemetria, rastreamento
    e monitoramento são integrados usando o Microsoft Agent Framework
    Observability e o Application Insights.

2.  No Visual Studio Code, antes de abrir uma nova pasta, selecione o
    arquivo .env, copie todo o conteúdo e guarde-o com segurança em um
    bloco de notas.

3.  Após concluir, clique na opção **File** no menu superior e selecione
    **Open Folder**.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image1.png)

4.  No painel Open Folder, navegue até C:\telemetry-codefiles e clique
    em Select Folder.

5.  Após a abertura, os arquivos exibidos no menu Explorer terão uma
    aparência semelhante a esta.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

6.  Analise os arquivos de código e revise como o OpenTelemetry foi
    implementado em todos os agentes, bem como a forma como o
    rastreamento está sendo realizado.

> **Visão Geral da Integração**
>
> O rastreamento do OpenTelemetry foi integrado em todo o fluxo de
> trabalho dos agentes usando o pacote agent_framework.observability.

- Importou get_tracer() e usou spans do OpenTelemetry para capturar
  telemetria estruturada para cada operação crítica.

- Integrou funções-chave (por exemplo, classificação, roteamento, RAG,
  criação de tíquetes) em spans com atributos contextuais.

- Adicionou configuração unificada de observabilidade na inicialização
  usando setup_observability() para configurar exportadores e pipelines
  de métricas.

- Registramos atributos personalizados, como texto de consulta, decisões
  de roteamento e métodos de fallback para obter maior visibilidade.

- Aprimoramos o tratamento de erros para registrar rastreamentos de
  exceção e vincular cada execução de fluxo de trabalho a um ID de
  rastreamento para correlação entre sistemas.

> **Melhorias nos Arquivos**
>
> main.py – Rastreamento e Métricas de Ponta a Ponta

- Configuração do pipeline de rastreamento do OpenTelemetry e da
  configuração de exportadores.

- Delimitação da orquestração multiagente dentro de spans para garantir
  visibilidade completa do fluxo de trabalho.

- Adição de spans para subetapas, incluindo roteamento, recuperação de
  dados (RAG), respostas dos agentes e criação de tíquetes.

> planner_agent.py – Observabilidade Aprimorada de Roteamento

- Adição de uma instância de tracer (get_tracer()) para monitorar a
  lógica de classificação.

- Captura de respostas do LLM, pontuações de confiança e métricas de
  palavras-chave de fallback como atributos de span.

- Diferenciação entre classificação baseada em AI e classificação
  heurística por meio de spans rotulados (SpanKind.INTERNAL).

> azure_search_tool.py – Observabilidade de RAG

- Adicionou spans para chamadas da Azure Search API para medir latência
  e taxas de sucesso.

- Registrou contagens de documentos recuperados e tamanhos de payload
  como métricas personalizadas.

- Capturou erros de pesquisa e dados de desempenho em traces do
  OpenTelemetry.

> freshdesk_tool.py – Observabilidade de criação de tíquetes

- Adicionou spans de chamadas de API para acompanhar a duração da
  criação de tíquetes e o status da resposta.

- Registrou IDs de tíquetes, tags e detalhes do solicitante para logs de
  auditoria rastreáveis.

- Monitorou a latência de APIs externas e respostas de erro para melhor
  rastreamento de incidentes.

7.  Após a revisão, clique com o botão direito no arquivo **.env.example
    (1)** e selecione **Rename (2)** para renomear o arquivo.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

8.  Após concluir, renomeie o arquivo de
    **.env.example** --\> **.env** para tornar este arquivo de ambiente
    ativo para este agente.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image4.png)

9.  Agora, selecione o arquivo .env e cole o conteúdo que você copiou
    anteriormente.

10. No Portal do Azure, navegue até o grupo de recursos **agenticai** e,
    na lista de recursos, selecione o serviço **ai-knowledge-Search**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

11. Selecione **Keys (1)** no menu esquerdo, em Settings, e copie o
    **Query key (2)** usando a opção de cópia conforme mostrado.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

12. Após copiar, cole-o com segurança em um Bloco de Notas. Em seguida,
    selecione **Indexes** no menu esquerdo, em Search Management, e
    copie o **Index Name (2).**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

13. No painel do Visual Studio Code, selecione o arquivo **.env**, pois
    será necessário adicionar as chaves do AI Search para a conexão.

> \# Azure AI Search (MCP)
>
> AZURE_SEARCH_ENDPOINT=https://ai-knowledge--@lab.LabInstance.Id.search.windows.net/
>
> AZURE_SEARCH_API_KEY=\[Query_Key\]
>
> AZURE_SEARCH_INDEX=\[Index_Name\]

**Observação:** Substitua os valores Query_Key e Index_Name pelos
valores que você copiou anteriormente.

14. Adicione o conteúdo abaixo ao arquivo .env:

> AZURE_OPENAI_ENDPOINT=https://agentic-
> @lab.LabInstance.Id.cognitiveservices.azure.com/
>
> AZURE_OPENAI_API_KEY=\<Replace with Azure OpenAI key\>
>
> AZURE_OPENAI_RESPONSES_DEPLOYMENT_NAME=gpt-4o-mini
>
> AZURE_OPENAI_API_VERSION=2025-03-01-preview

15. Adicione as seguintes variáveis de chave do projeto do Foundry ao
    arquivo .env.

> \# Azure AI Project Configuration
>
> AZURE_AI_PROJECT_ENDPOINT=**\<Microsoft Foundry endpoint\>**
>
> AZURE_AI_MODEL_DEPLOYMENT_NAME=gpt-4o-mini
>
> Encontre o endpoint do projeto do Microsoft Foundry na página Overview
> e substitua **\<Microsoft Foundry endpoint\>** por esse valor.
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image8.png)

![](./media/image9.png)

16. Após concluir, adicione as seguintes variáveis do App Insights ao
    mesmo arquivo.

> \# Observability and Monitoring Configuration
>
> APPLICATIONINSIGHTS_CONNECTION_STRING=**\<Connection string\>**
>
> ENABLE_OTEL=true
>
> ENABLE_SENSITIVE_DATA=true
>
> Abra o recurso do Application Insights no portal do Azure, copie a
> connection string e substitua **\<Connection string\>** pelo valor
> copiado.
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image10.png)

17. No arquivo .env, adicione o conteúdo a seguir e informe a API key e
    a Account URL do Freshdesk que você copiou anteriormente.

> \# Freshdesk Configuration
>
> FRESHDESK_DOMAIN=\[Domain_URL\]
>
> FRESHDESK_API_KEY=\[API_Key\]

18. O arquivo .env final deve ter a mesma aparência da imagem fornecida.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

19. Após concluir, selecione **File (1)** e, em seguida, clique em
    **Save (2)** para salvar o arquivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image12.png)

20. Selecione a opção **... (1)** no menu superior para expandir o menu.
    Selecione **Terminal (2)** e clique em **New Terminal (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

21. No **VS Code** Terminal, execute o comando de login da Azure CLI:

+++az login+++

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image14.png)

22. Na janela **Sign in**, selecione **Work or school account** e clique
    em **Continue**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.png)

23. Na guia **Sign into Microsoft**, faça login usando as credenciais
    abaixo:

- Username - <+++@lab.CloudPortalCredential(User1).Username>+++

- TAP - +++@lab.CloudPortalCredential(User1).TAP+++

24. Quando solicitado com as opções de login, selecione **No, this app
    only** para continuar sem vincular outros aplicativos da área de
    trabalho.

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image16.png)

25. Digite **1** e pressione Enter na tela **Select a subscription and
    tenant**.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image17.png)

26. Após o terminal estar aberto, execute o comando:

> +++pip install -r requirements.txt+++ to install all the required
> packages.

27. Run the command given below to test out the working of the search
    tool.

+++python main.py+++

> ![A screenshot of a computer screen AI-generated content may be
> incorrect.](./media/image18.png)

## Tarefa 2: Visualizar métricas do agente

Nesta tarefa, você usará o Azure Application Insights para visualizar os
dados de telemetria do agente. Você explorará métricas personalizadas
para tempo de resposta, precisão de roteamento e sucesso na criação de
tíquetes. Em seguida, você criará painéis interativos do Azure Monitor
para exibir indicadores-chave de desempenho e tendências. Isso ajuda a
identificar gargalos, medir a eficiência e garantir a operação saudável
dos agentes implantados em tempo real.

1.  Navegue até o Portal do Azure, abra seu grupo de recursos e, na
    lista de recursos, selecione o recurso **agent-insights-** app
    insight.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.png)

2.  Após acessar a página Overview, você poderá ver algumas das métricas
    padrão exibidas.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

3.  No menu esquerdo, selecione **Search (1)** e clique em **See all
    data in last 24 hours (2)**.

![A screenshot of a search engine AI-generated content may be
incorrect.](./media/image21.png)

4.  Após abrir, na parte inferior, revise **Traces (1)** e, em seguida,
    clique em **View as individual items (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

5.  Após concluir, você poderá ver todos os detalhes de comunicação que
    ocorreram com o agente, bem como todas as transações que aconteceram
    dentro do intervalo de tempo definido. Você também pode ajustar o
    intervalo de tempo para explorar mais informações.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

6.  Explore e revise essas transações; você pode abrir uma visualização
    detalhada simplesmente clicando nelas. Revise como é possível
    visualizar todos os detalhes, como agentes, mensagens e detalhes de
    recuperação.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

7.  Em seguida, selecione **Failures (1)** e **Review failed requests
    (2)** para obter uma visão centralizada de todas as execuções
    malsucedidas e identificar as causas subjacentes por meio de uma
    análise detalhada do *rastreamento*.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

8.  Em seguida, selecione **Performance (1)** e verifique as **operações
    e os tempos de resposta (2)**, com base nos quais você pode
    determinar o SLA de desempenho do agente.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

9.  Agora, em Monitoring, no menu esquerdo, selecione **Metrics**. Você
    pode explorar as métricas personalizadas que são publicadas por meio
    de *spans*.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.png)

10. Após selecionar, em **Metric Namespace (1)**, selecione
    azure.applicationinsights **(2)**. ![A screenshot of a computer
    AI-generated content may be incorrect.](./media/image29.png)

11. Agora, em Metrics, selecione **gen_ai.client.operation.duration**
    **e defina a agregação como** **avg (1)**. Verifique o **line chart
    (2)** para revisar a métrica de **Response Time**, que indica o
    tempo que o agente levou para responder ao usuário.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image30.png)

12. De forma semelhante, selecione **gen_ai.client.token.usage** **e
    defina a agregação como** **avg (1)**. Verifique o **line chart
    (2)** para revisar o uso de tokens pelo agente.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

13. Em seguida, no menu esquerdo, selecione **Logs (1)** e cancele o
    painel **Queries hub (2)**..

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image32.png)

14. Após fechar, clique na opção **Tables**, passe o cursor sobre o
    parâmetro **customMetrics**; você verá a opção **Run**. Clique nela.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

![A close-up of a message AI-generated content may be
incorrect.](./media/image34.png)

15. Após a consulta ser executada com sucesso, você verá todas as
    métricas personalizadas listadas abaixo como resultados da consulta.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image35.png)

16. Em seguida, selecione **Workbooks (1)** no menu esquerdo e clique no
    workbook **Empty (2)** em Quick start.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.png)

17. Após abrir, clique em **+ Add (1)** e selecione **Add metric (2)**.

![A screenshot of a phone AI-generated content may be
incorrect.](./media/image37.png)

18. Após o painel de métricas ser aberto, clique na opção **Add
    metric**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

19. Agora, selecione **Metric** como gen_ai.client.token.usage **(1)**,
    informe **Display name** como **Token Usage (2)** e clique em **Save
    (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.png)

20. Clique novamente na opção **Add metric**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

21. Agora, selecione **Metric** como gen_ai.client.operation.duration
    **(1)**, informe **Display name** como Response Time **(2)** e
    clique em **Save (3)**.

![A screenshot of a screenshot of a metric settings AI-generated content
may be incorrect.](./media/image40.png)

22. Após selecionar ambas as métricas, clique em **Run Metrics**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.png)

23. Agora, altere a **Visualization** para **Area Chart** para obter uma
    visualização semelhante. Você pode explorar várias outras opções de
    visualização, assim como o intervalo de tempo.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.png)

24. Após a edição ser concluída, clique em **Done editing**. Isso
    salvará este cartão no seu workbook.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.png)

25. Agora, clique novamente em **+ Add (1)** e selecione **Add query
    (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.png)

26. No painel de consulta, adicione a seguinte **query (1)**, e clique
    em **Run Query (2)**.

+++customMetrics+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.png)

27. Verifique os resultados após a consulta ser executada com sucesso.
    Depois de revisar, clique em **Done editing**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image46.png)

28. Após concluir, clique em **Done editing (1)** no menu superior e, em
    seguida, clique no ícone **Save (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image48.png)

29. No painel Save As, informe o Title como agent-workbook **(1)** e, em
    seguida, clique em **Save As (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image49.png)

30. Como este é um ambiente de laboratório, os dados disponíveis podem
    ser limitados para um monitoramento abrangente. No entanto, você
    pode aumentar a visibilidade adicionando métricas personalizadas dos
    seus agentes e criando painéis de monitoramento específicos, focados
    em objetivos determinados, como os seguintes:

- **Painel de desempenho do agente**

> **Métricas exibidas:**

- Tempos de resposta dos agentes (média, P95)

- Taxas de sucesso por tipo de agente

- Tendências do volume de solicitações

- Alertas de taxa de erro

> **Perguntas comerciais respondidas:**

- Quais agentes têm o melhor desempenho?

- Estamos cumprindo as metas do SLA?

- O que está causando lentidão no sistema?

&nbsp;

- **Painel de Experiência do Usuário**

> **Métricas exibidas:**

- Latência de solicitação de ponta a ponta

- Taxas de criação de tíquetes

- Sucesso na recuperação de conhecimento

- Métricas proxy de satisfação do usuário

> **Perguntas comerciais respondidas:**

- Os usuários estão recebendo respostas rápidas?

- Com que frequência as solicitações se transformam em tíquetes de
  suporte?

- A base de conhecimento está ajudando os usuários?

## Tarefa 3: Monitorar métricas específicas do agente no Portal do Foundry

Nesta tarefa, você usará o Azure Application Insights para visualizar os
dados de telemetria do agente. Você explorará métricas personalizadas
específicas do agente no Portal do Microsoft Foundry.

1.  Como você já conectou o Application Insights ao portal do Microsoft
    Foundry, pode navegar de volta ao portal do Foundry e visualizar o
    funcionamento do seu agente.

2.  Navegue de volta ao seu grupo de recursos e, na lista de recursos,
    selecione o recurso **agent-**foundry.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.png)

3.  No painel seguinte, clique em **Go to Foundry portal**. Você será
    direcionado para o portal do Microsoft Foundry, onde criará seu
    primeiro agente.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image51.png)

4.  Antes de testar o agente, conecte o Application Insights para
    habilitar logs detalhados e visibilidade de rastreamento.

5.  No portal do **Microsoft Foundry**, selecione **Monitoring (1)** no
    menu esquerdo, selecione **agent-insights- (2)** e clique em
    **Connect (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image52.png)

6.  Agora, navegue até o painel **Monitoring**, onde você conectou o
    Application Insights anteriormente, selecione a aba **Resource
    usage** e revise todas as métricas e valores.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image53.png)

7.  Selecione **Tracing (1)** no menu esquerdo, clique em qualquer um
    dos **Trace (2)**, e analise os rastreamentos detalhados das
    interações do agente.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image54.png)

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image55.png)

**Resumo**

Neste laboratório, você configurou a observabilidade e o monitoramento
para os agentes da sua empresa. Usando o recurso de rastreamento do
OpenTelemetry, você capturou dados detalhados de execução para cada
etapa do fluxo de trabalho e, ao integrar com o Azure Application
Insights, criou painéis para visualizar métricas de desempenho e
integridade do agente.

Você concluiu este laboratório com sucesso. Clique em Next \>\> para
prosseguir.
