# Criar e Estender Agentes Inteligentes

**Visão Geral**

Este laboratório prático apresenta a criação de agentes inteligentes de
**AI** utilizando serviços do Azure AI e o Microsoft 365 Copilot. Os
participantes aprenderão a aproveitar fluxos de trabalho de RH com
Copilot, configurar projetos no Microsoft Foundry, criar agentes simples
de AI, criar agentes RAG (Retrieval-Augmented Generation) e desenvolver
sistemas multiagentes com capacidades de orquestração.

**Objetivos**

Ao final deste laboratório, você será capaz de:

- **Criar Agentes Assistentes de RH:** Automatizar recrutamento de
  colaboradores, triagem, desenvolvimento de materiais de treinamento,
  coleta de feedback e avaliações de desempenho utilizando o Microsoft
  365 Copilot.

- **Configurar um projeto de AI e concluir o chat:** Configurar um
  projeto de AI no Microsoft Foundry, implementar Large Language Models
  (LLMs) e modelos de integração e estabelecer conectividade VS Code
  para conclusões de chat.

- **Criar um Agente de AI para Análise de Planos de Seguro Saúde:**
  Criar agentes de AI que processam dados e geram visualizações (por
  exemplo, gráficos de barras comparando planos de benefícios de saúde)
  usando serviços do Azure AI.

- **Desenvolver um Sistema Multiagente para Geração de Relatórios de
  Planos de Saúde:** Projetar e implementar sistemas multiagentes
  coordenados, nos quais agentes especializados (agentes de pesquisa,
  relatório, validação e orquestração) trabalham juntos para realizar
  tarefas complexas.

**Pré-requisitos**

Os participantes devem possuir:

- **Visual Studio Code (VS Code)**: Proficiência no uso do VS Code para
  codificação, depuração e gerenciamento de extensões para diversas
  linguagens de programação e frameworks.

- **Habilidades de Desenvolvimento:** Conhecimento básico de programação
  em Python ou JavaScript, experiência com APIs e SDKs, e familiaridade
  com o Visual Studio Code.

- **Linha de Comando / Terminal:** Familiaridade com a execução de
  comandos no PowerShell e com o gerenciamento de ambientes virtuais.

**Explicação dos Componentes**

- **Azure AI Search**: serviço de busca baseado em vetores que habilita
  RAG por meio da indexação e recuperação de documentos relevantes.

- **Azure OpenAI Service**: fornece acesso ao GPT-4o e a modelos de
  embeddings por meio da infraestrutura corporativa do Azure.

- **Large Language Models (LLMs)**: modelos avançados de AI, como o
  GPT-4o, para compreensão e geração de texto.

- **Embedding Models**: convertem texto em representações vetoriais para
  busca e recuperação semântica (por exemplo, text-embedding-3-large).

- **Microsoft 365 Copilot**: ferramenta de produtividade baseada em AI
  para análise de documentos e automação de fluxos de trabalho.

- **Semantic Kernel**: SDK para integrar LLMs com linguagens de
  programação e construir capacidades de orquestração.

# Laboratório 1: Criar um Agente Assistente de RH com Copilot Studio

Duração estimada: 30 minutos

Visão Geral

Neste laboratório, você irá se concentrar em simplificar e aprimorar o
processo de transição e integração de colaboradores dentro de uma
organização usando o Microsoft 365 Copilot e o Copilot Studio. Você
aprenderá a identificar candidatos adequados, criar planos
personalizados de transição e integração, gerar materiais eficazes de
comunicação e treinamento, automatizar fluxos de trabalho de RH, coletar
feedback e configurar mecanismos de monitoramento e avaliação de
desempenho. Ao aproveitar ferramentas baseadas em AI, este laboratório
demonstra como as organizações podem garantir um processo de transição
tranquilo e eficiente, aprimorar a mobilidade interna e apoiar os
colaboradores na adaptação bem-sucedida a suas novas funções.

Objetivos do Laboratório

Você executará as seguintes tarefas neste laboratório:

- Tarefa 1: Realizar a triagem rápida de candidatos

- Tarefa 2: Desenvolver materiais de treinamento

- Tarefa 3: Coletar feedback

- Tarefa 4: Avaliações de desempenho

Diagrama de Arquitetura

![image](./media/image1.png)

## Tarefa 1: Realizar a triagem rápida de candidatos

Nesta tarefa, você avaliará rapidamente um grande número de candidaturas
para o cargo de Analista de Dados usando o Microsoft 365 Copilot para
analisar currículos e filtrar candidatos com base em critérios
específicos, como experiência relevante, habilidades técnicas e formação
acadêmica, permitindo que o Copilot destaque os melhores candidatos para
uma análise mais aprofundada.

1.  Adicione uma nova aba no navegador Edge e abra o aplicativo
    Microsoft 365 Copilot usando o seguinte link, e clique em **Sign in
    (2)**.

+++https://m365.cloud.microsoft/+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

2.  Na aba **Sign into Microsoft Azure**, você verá uma tela de login.
    Faça login usando as credenciais abaixo:

- Username - +++@lab.CloudPortalCredential(User1).Username+++

- TAP - +++@lab.CloudPortalCredential(User1).TAP+++

3.  Se você visualizar o pop-up **Welcome to your Microsoft 365 Copilot
    app**, clique em **Get started**.

![A screenshot of a computer application AI-generated content may be
incorrect.](./media/image3.png)

4.  No painel esquerdo, selecione **Apps (1)** e, em seguida, clique em
    **OneDrive (2)** na seção **Apps**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

**Observação:** Se aparecer o pop-up **Welcome to Apps**, clique em
**X** para fechá-lo.

![A screenshot of a computer application AI-generated content may be
incorrect.](./media/image5.png)

5.  Acesse **My files**, depois clique no botão **+ Create or upload
    (1)** e selecione **Folder upload (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

6.  Navegue até C:\LabFiles\Day-1\data **(1)**, clique na pasta CV
    **(2)** e selecione **Upload (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

7.  No pop-up **Upload** 5 files to this site?, selecione Upload.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

8.  Novamente, clique em **+ Create or upload (1)** e selecione **Folder
    upload (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

9.  Navegue até C:\LabFiles\Day-1 **(1)**, clique na pasta data **(2)**
    e clique em **Upload (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.png)

10. No pop-up **Upload** 19 files to this site? Selecione Upload.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.png)

11. Retorne ao **M365 Copilot**, no painel esquerdo selecione **Apps
    (1)** e, em seguida, clique em **Copilot (2)** na seção Apps.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

12. Navegue até **Copilot** pelo painel esquerdo e clique em **Chat
    (1)**. Em seguida, clique no ícone **+ (Add) (2)** na parte inferior
    do painel de chat e selecione **Upload images and files (3)**.

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image12.png)

13. No pop-up do explorador de arquivos, navegue até a pasta
    C:\LabFiles\Day-1\data\CV **(1)**, selecione os **3 primeiros**
    arquivos **(2)** e clique em **Open (3)**.

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image13.png)

14. No **chat do Copilot**, após os **3 arquivos** serem carregados com
    sucesso, pressione **Enter**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

15. No chat ativo do Copilot, clique no ícone **+ (Add) (1)** abaixo da
    caixa de mensagem e selecione **Upload images and files (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.png)

16. No pop-up do explorador de arquivos, navegue até a pasta
    C:\LabFiles\Day-1\Data\CV **(1)**, selecione os **2 últimos**
    arquivos **(2)** e clique em **Open (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image16.png)

17. No **chat do Copilot**, após os **2 arquivos (1)** serem carregados
    com sucesso, pressione **Enter (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.png)

18. Na caixa de chat, insira o seguinte prompt **(1)** e clique no botão
    **Sent (2)**:

> Microsoft 365 Copilot, ajude-me a filtrar e selecionar currículos de
> candidatos a analista de dados com base nas qualificações exigidas,
> como experiência em SQL, Python e ferramentas de visualização de
> dados.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.png)

19. Em seguida, utilize o prompt abaixo e clique em **Sent**:

> Crie um relatório resumido dos melhores candidatos a Analista de
> Dados, incluindo suas habilidades, experiência profissional e formação
> acadêmica.
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

**Resultado**: A equipe de RH identifica de forma eficiente os
candidatos mais qualificados, economizando tempo e garantindo um
processo de recrutamento mais focado.

## Tarefa 2: Desenvolver materiais de treinamento.

Nesta tarefa, você irá preparar materiais de treinamento abrangentes
para o novo colaborador usando o Microsoft Copilot para criar conteúdos
personalizados de onboarding, incluindo guias específicos da função,
políticas da empresa e uma visão geral das ferramentas e tecnologias
utilizadas, garantindo que os materiais sejam completos, bem
estruturados e adaptados à função do colaborador.

1.  Na caixa de chat, insira o seguinte prompt **(1)** e clique em
    **Sent (2)**:

> Crie um plano de treinamento abrangente para o novo analista de dados,
> incluindo tópicos como políticas da empresa, treinamento em
> ferramentas de dados e apresentações da equipe.
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image20.png) ![A screenshot of a web page
> AI-generated content may be incorrect.](./media/image21.png)

2.  Em seguida, utilize o prompt abaixo **(1)** e clique em **Sent
    (2)**:

> Crie uma apresentação de treinamento interativa que aborde as melhores
> práticas de análise de dados e as principais métricas de desempenho e
> gere um PPT para download.
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image22.png)

**Observação:** após executar esse prompt, será gerada uma apresentação
em PowerPoint para download, que poderá ser editada ou personalizada.
Caso o arquivo não seja baixado automaticamente, procure pelo hyperlink
com o título da apresentação conforme mostrado no screenshot.

**Observação**: se a opção de download do PowerPoint não aparecer,
execute novamente o prompt acima.

Resultado: o novo colaborador recebe materiais de treinamento bem
organizados, permitindo que ele se integre rapidamente e execute suas
atividades com eficiência.

## Tarefa 3: Coletar feedback

Nesta tarefa, você irá coletar feedback de novos colaboradores e
entrevistadores usando o Microsoft Copilot para gerar e distribuir
formulários de feedback, coletar e analisar respostas e obter insights
sobre os pontos fortes do processo de recrutamento e integração, bem
como áreas que precisam de melhoria.

1.  Na caixa de chat, insira o seguinte prompt e clique em **Sent**:

> Crie um formulário de feedback para os entrevistadores avaliarem os
> candidatos a analista de dados com base em habilidades técnicas,
> capacidade de resolução de problemas e adequação cultural. Gere uma
> versão em Word ou PDF deste formulário de feedback para download.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

2.  Em seguida, utilize o prompt abaixo e clique em **Sent**:

> Envie uma pesquisa aos novos contratados para coletar feedback sobre
> sua experiência de integração e identificar áreas que precisam ser
> melhoradas. Gere uma versão da pesquisa em Word ou PDF para download.
>
> ![A screenshot of a survey AI-generated content may be
> incorrect.](./media/image23.png)
>
> Resultado: o departamento de RH obtém feedback valioso, permitindo-lhe
> aperfeiçoar as suas práticas de recrutamento e integração, garantindo
> uma melhor experiência para futuras contratações..

## Tarefa 4: Avaliações de desempenho

Nesta tarefa, você irá conduzir avaliações de desempenho regulares para
avaliar o progresso e o desenvolvimento do novo colaborador usando o
Microsoft Copilot para criar modelos de avaliação, agendar reuniões de
revisão, acompanhar conquistas, coletar feedback de colegas e compilar
relatórios estruturados de desempenho.

1.  Na caixa de chat, insira o seguinte prompt e clique em **Sent**:

> Estabeleça um cronograma de avaliação de desempenho para o novo
> Analista de Dados, com avaliações trimestrais e sessões de definição
> de metas, e gere um calendário em formato CSV.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

2.  Em seguida, utilize o prompt abaixo e clique em **Sent**:

> Crie um modelo para relatórios de avaliação de desempenho, incluindo
> seções para realizações, áreas de melhoria e metas futuras. Crie um
> modelo de avaliação de desempenho.
>
> ![A screenshot of a report AI-generated content may be
> incorrect.](./media/image25.png)
>
> Resultado: o novo colaborador recebe feedback construtivo e suporte
> contínuo, contribuindo para seu crescimento profissional e sucesso de
> longo prazo na empresa.
>
> **Resumo**
>
> Neste laboratório, você criou com sucesso um Agente Assistente de RH
> usando o Microsoft 365 Copilot para otimizar os processos de
> recrutamento e integração de funcionários. Você aprendeu a selecionar
> rapidamente candidatos a Analista de Dados, analisando currículos e
> filtrando com base em habilidades técnicas como SQL, Python e
> visualização de dados, e então criou planos de treinamento de
> integração abrangentes e apresentações interativas para novos
> contratados. Você gerou formulários de feedback para entrevistadores e
> pesquisas para novos funcionários para avaliar e melhorar o processo
> de recrutamento e configurou cronogramas trimestrais de avaliação de
> desempenho com modelos estruturados para acompanhar conquistas e
> metas. Ao aproveitar as ferramentas baseadas em AI, você demonstrou
> como as organizações podem automatizar fluxos de trabalho de RH,
> aumentar a eficiência e garantir um processo de transição tranquilo
> para novos funcionários.
>
> Você concluiu este laboratório com sucesso. Clique em Next \>\> para
> prosseguir.
