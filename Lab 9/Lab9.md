# Laboratório 9: Implementação de Fluxos de Trabalho com Agentes Únicos e Múltiplos usando o Azure AI Framework

**Duração Estimada**: 45 minutos

**Visão Geral**

Você é um Engenheiro de AI na Contoso Ltd., responsável por implementar
fluxos de trabalho inteligentes com agentes usando o Azure AI Framework.
Neste laboratório, você criará um sistema de agente único que se integra
a ferramentas externas usando MCP e, em seguida, implementará fluxos de
trabalho multiagente, nos quais vários agentes especializados colaboram
ou realizam transferência de responsabilidade de tarefas dinamicamente
com base na intenção do usuário.

Objetivos do Laboratório

Você executará as seguintes tarefas neste laboratório:

- Tarefa 1: Criar e testar um agente de chat do Azure OpenAI

- Tarefa 2: Criar um fluxo de trabalho de agente único com integração de
  ferramentas

- Tarefa 3: Projetar um fluxo de trabalho multiagente

  - Tarefa 3.1: Orquestrar fluxos de trabalho multiagentes

  - Tarefa 3.2: Padrão de transferência do sistema multiagentes

## Tarefa 0: Configuração do ambiente do laboratório

1.  No diretório C:\Labfiles\Day 2, extraia o arquivo
    **OpenAIWorkshop-Framework**.

2.  Clique no **Visual Studio Code** a partir da área de trabalho da
    LabVM.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

3.  Selecione **File** (1) e clique em **Open Folder** (2) para abrir a
    pasta **OpenAIWorkshop-Framework**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

4.  Navegue até o caminho C:\Labfiles\Day
    2\\**OpenAIWorkshop-Framework**,
    selecione **OpenAIWorkshop-Framework** e, em seguida, clique em
    **Select Folder**.

5.  Selecione **Yes, I trust the authors**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

6.  Clique nas **reticências (...) (1)**, selecione **Terminal** **(2)**
    e, em seguida, clique em **New Terminal (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

7.  Digite o comando abaixo para navegar até o diretório
    **applications** e instalar todas as dependências necessárias a
    partir do arquivo **requirements.txt**.

> cd agentic_ai/applications
>
> pip install -r requirements.txt

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image5.png)

8.  O comando pode levar de 5 a 10 minutos para ser concluído.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image6.png)

## Tarefa 1: Criar e testar um agente de chat do Azure OpenAI

Nesta tarefa, você irá criar e testar um agente de chat simples do Azure
OpenAI no Visual Studio Code. Você configurará variáveis de ambiente,
conectará o agente ao modelo implementado e observará como ele gera
respostas dinâmicas com base em diferentes prompts.

1.  Navegue de volta ao **Visual Studio Code**.

2.  Certifique-se de que o comando pip install -r requirements.txt foi
    concluído com sucesso. Caso ainda esteja em execução, aguarde até
    que seja finalizado.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image6.png)

3.  No **Explorer**, expanda **agentic_ai** **(1)** \> **applications**
    **(2)**. Clique com o botão direito em .env.sample **(3)** e
    selecione **Rename (4)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

4.  Renomeie o arquivo para .env e clique nele para abrir o arquivo.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

5.  Substitua os valores de
    AZURE_OPENAI_API_KEY **(1)** e AZURE_OPENAI_ENDPOINT **(2)** pelos
    valores reais. Obtenha essas informações na página **Overview** do
    Microsoft Foundry.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image9.png)

6.  Adicione a variável AZURE_OPENAI_CHAT_DEPLOYMENT como
    valor **gpt-4o-mini (3)**

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image10.png)

7.  Selecione **File (1)** e, em seguida, clique em **Save (2)**.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

8.  Clique com o botão direito na pasta **application** **(1)** e
    selecione **New File** **(2)** para criar um novo arquivo e
    configurar um agente simples.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.png)

9.  Nomeie o arquivo do agente como +++simple_agent_test.py+++.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

10. Copie e cole o código a seguir no arquivo.

> import asyncio
>
> import os
>
> from dotenv import load_dotenv
>
> from agent_framework.azure import AzureOpenAIChatClient
>
> from azure.identity import AzureCliCredential
>
> \# Load .env file (same folder or specify full path)
>
> load_dotenv(dotenv_path=".env")
>
> \# Retrieve values from .env
>
> endpoint = os.getenv("AZURE_OPENAI_ENDPOINT")
>
> deployment_name = os.getenv("AZURE_OPENAI_CHAT_DEPLOYMENT")
>
> api_version = os.getenv("AZURE_OPENAI_API_VERSION")
>
> print("Using Azure OpenAI endpoint:", endpoint)
>
> print("Deployment name:", deployment_name)
>
> print("API version:", api_version)
>
> \# ✅ Correct parameter name is deployment_name (not deployment)
>
> agent = AzureOpenAIChatClient(
>
> api_key=os.getenv("AZURE_OPENAI_API_KEY"),
>
> endpoint=endpoint,
>
> deployment_name=deployment_name,
>
> api_version=api_version
>
> ).create_agent(
>
> instructions="You are a helpful and funny assistant who tells short
> jokes.",
>
> name="Joker"
>
> )
>
> async def main():
>
> result = await agent.run("Tell me a joke about the cloud.")
>
> print("\nAgent response:\n", result.text)
>
> asyncio.run(main())

![A computer screen shot of a program AI-generated content may be
incorrect.](./media/image14.png)

11. Selecione **File (1)** e, em seguida, clique em **Save** **(2).**

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

12. Clique com o botão direito em simple_agent_test.py **(1)** e
    selecione **Open in Integrated Terminal** **(2)**.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image15.png)

13. Execute o comando abaixo para executar o agente e observar a saída,
    a fim de entender como o agente funcionou.

+++python simple_agent_test.py+++

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image16.png)

14. Vamos modificar a instrução para observar como o agente responde.
    Forneça a instrução Tell me a joke about the Earth **(1)** (linha
    nº 31) e, em seguida, **salve** o arquivo. Depois, execute o comando
    abaixo **(2)** e revise a resposta do agente **(3)**.

+++python simple_agent_test.py+++

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image17.png)

15. Isso demonstra como a resposta do agente varia com base na instrução
    fornecida, destacando sua capacidade de se adaptar a diferentes
    prompts.

## Tarefa 2: Criar um Fluxo de Trabalho de Agente Único com Integração de Ferramentas

Nesta tarefa, você irá criar e testar um fluxo de trabalho de agente
único que se integra a ferramentas externas usando o MCP (Model Context
Protocol). Você configurará variáveis de ambiente, executará o servidor
MCP, o back-end e o front-end localmente, e observará como o agente
utiliza as ferramentas MCP para processar consultas do usuário e
fornecer respostas inteligentes e com reconhecimento de contexto.

1.  No Visual Studio Code, expanda **agents** **(1)
    \>** **agent_framework** **(2) \>** **single_agent** **(3)** e
    visualize o fluxo de trabalho de Agente Único com a ferramenta
    MCPStreamableHTTPTool integrada **(4)**.

    - MCPStreamableHTTPTool permite que o agente invoque serviços
      externos baseados em HTTP por meio do servidor MCP e inclua as
      saídas das ferramentas na conversa.

    - A ferramenta é passada para o ChatAgent e utilizada
      automaticamente com base nas instruções e nos prompts do usuário.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image18.png)

2.  Analise o código para entender como a integração é realizada:

    - No método \_maybe_create_tools:

> ![A screen shot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

- É criada uma ferramenta HTTP com suporte a streaming conectada ao MCP
  server.

- Isso permite que o agente realize chamadas HTTP para serviços externos
  (por meio do MCP) como parte do seu fluxo de trabalho.

&nbsp;

- A ferramenta é passada para o ChatAgent durante a inicialização:

> ![A screen shot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

- O agente pode então utilizar essa ferramenta sempre que um prompt do
  usuário acionar uma chamada de ferramenta.

- Suporte a streaming com WebSocket: quando uma ferramenta/função é
  chamada durante uma conversa em streaming, , o nome da ferramenta e o
  turno são transmitidos por meio de \_chat_async_streaming.

3.  Navegue até o arquivo .env **(1)** e adicione a seguinte variável de
    ambiente para especificar o **fluxo de trabalho do** **agente
    único** a ser executado **(2)**:

+++AGENT_MODULE=agents.agent_framework.single_agent+++

- Adicione a variável de ambiente DISABLE_AUTH=true **(3)**. Ela é
  utilizada para desabilitar a autenticação na aplicação, permitindo um
  desenvolvimento e teste local mais simples.

> +++DISABLE_AUTH=true+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

4.  Selecione **File** **(1)** e, em seguida, clique em **Save**
    **(2)**.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

5.  Agora, você iniciará o servidor **MCP**, o **backend** e o
    **frontend React** para executar todo o ambiente de agentes
    localmente, permitindo que a interface do usuário interaja com os
    agentes e as ferramentas.

6.  Na janela do Visual Studio Code, clique nas **reticências**
    **(...)** **(1)**, selecione **Terminal** (2) e, em seguida, clique
    em **New Terminal** **(3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

7.  Aguarde a conclusão da etapa anterior e prossiga para a próxima
    etapa.

8.  **Inicie o MCP Server (Terminal 1)**: (o diretório mcp está
    localizado no nível raiz do projeto)

    - Execute o comando abaixo para iniciar o **MCP server**, que
      disponibiliza APIs às quais os agentes podem acessar como
      ferramentas. (O servidor é executado
      em [http://localhost:8000](http://localhost:8000/))

> cd mcp
>
> uv run python mcp_service.py
>
> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image21.png)
>
> Observação: Se você encontrar algum erro, execute os comandos abaixo:

+++pip install uv+++

+++uv run python mcp_service.py+++

9.  Deixe o comando ser executado e abra um novo terminal.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

10. **Inicie o backend (Terminal 2):**

    - Execute o comando abaixo para iniciar o servidor backend, que
      hospeda seus fluxos de trabalho de agentes, gerenciamento de
      sessões e endpoints de API.

> cd agentic_ai/applications
>
> uv run python backend.py

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image22.png)

- Execute localmente
  em: [http://localhost:7000](http://localhost:7000/).

- Esta é a lógica principal da aplicação com a qual o frontend irá se
  comunicar. Certifique-se de que a **conexão esteja aberta**.

> ![A screen shot of a computer AI-generated content may be
> incorrect.](./media/image23.png)

11. Deixe o comando em execução e abra um novo terminal.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

12. **Inicie o Frontend React (Terminal 3):**

    - Digite o comando abaixo para navegar até o diretório
      react-frontend:

> +++cd agentic_ai/applications/react-frontend+++

- Em seguida, execute o comando abaixo para iniciar o **frontend React**
  da interface do agente. Ele fornece uma interface de usuário para
  interagir com os agentes e visualizar suas respostas em tempo real:

> +++npm start+++

- A compilação pode levar algum tempo. Ignore os avisos e aguarde até a
  conclusão. Quando o **webpack for compilado com sucesso**, a aplicação
  do agente será executada localmente
  em: [http://localhost:3000](http://localhost:3000/).

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image24.png)

13. Quando os três terminais estiverem em execução, a aplicação do
    agente será iniciada no navegador, permitindo que você interaja com
    o agente e teste suas funcionalidades.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

**Observação:** certifique-se de que os três terminais estejam em
execução. Se algum deles for interrompido, execute novamente o comando
correspondente. Caso os três não estejam ativos, você poderá encontrar
um erro de conexão.

14. Envie o prompt abaixo no chat **(1)** e visualize a resposta
    **(2)**:

+++Customer 251, what's my billing summary?+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

**Observação:** Certifique-se de que os três terminais estejam em
execução. Se algum deles for interrompido, execute novamente o comando
correspondente. Caso os três não estejam ativos, você poderá encontrar
um erro de conexão.

15. Visualize a saída. Foi o ChatAgent (self.\_agent) que interpretou o
    prompt, possivelmente acionou a **ferramenta MCP** e gerou a saída.

    - O agente interpretou sua solicitação como uma consulta de
      faturamento para o **Cliente 251**.

    - Ele utilizou a **ferramenta MCP** para buscar dados de faturamento
      estruturados.

    - O agente está funcionando conforme o esperado: ele integra
      dinamicamente as saídas das ferramentas e o raciocínio de AI para
      responder a perguntas específicas do usuário.

16. Após concluir os testes, retorne ao VS Code e encerre todas as
    sessões de terminal em execução. Isso garante que o próximo fluxo de
    trabalho multiagente seja executado sem qualquer interferência.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

## Tarefa 3: Projetar um fluxo de trabalho multiagente

Nesta tarefa, você irá projetar e implementar fluxos de trabalho
multiagente avançados que demonstram diferentes padrões de coordenação.
Você começará orquestrando vários agentes especializados por meio de um
gerenciador central para lidar de forma colaborativa com consultas
complexas e, em seguida, explorará um sistema baseado em transferência
de responsabilidade, no qual o controle é alternado dinamicamente entre
agentes específicos de domínio com base na intenção do usuário.

### Tarefa 3.1: Orquestrando fluxos de trabalho multiagentes

Nesta tarefa, você irá orquestrar um fluxo de trabalho multiagente no
qual um orquestrador central coordena vários agentes especializados para
processar colaborativamente consultas complexas dos usuários e gerar
respostas precisas baseadas em ferramentas.

1.  Navegue até **agent (1) \> agent_framework (2) \> multi_agent (3) \>
    magentic_group (4)** e visualize o código **(5)**.

    - Esse código representa uma estrutura de **orquestração
      multiagente**, pois define um sistema no qual vários agentes
      especializados colaboram sob a orientação de um orquestrador
      central.

> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image28.png)

- O método \_create_participants inicializa vários agentes especialistas
  (CRM/Faturamento, Produtos/Promoções, Segurança/Autenticação).

- Cada agente:

  - Possui um domínio específico e um conjunto próprio de ferramentas.

  - Comunica-se apenas com o orquestrador, e não diretamente com o
    usuário.

  - Fornece respostas factuais, fundamentadas em ferramentas.

- Estes são os agentes utilizados neste fluxo de trabalho multiagente:

  - **Agente de CRM e Faturamento** – Lida com contas de clientes,
    assinaturas, faturamento, faturas, pagamentos e consultas
    relacionadas, utilizando dados factuais obtidos por meio de
    ferramentas.

  - **Agente de Produtos e Promoções –** Fornece informações sobre
    disponibilidade de produtos, promoções, descontos, elegibilidade e
    termos, com base em fontes estruturadas.

  - **Agente de Segurança e Autenticação –** Gerencia autenticação,
    autorização, verificação de identidade e consultas relacionadas à
    segurança.

2.  Navegue até o arquivo .env **(1)**, comente a variável de agente
    único **(2)** e insira o comando abaixo para adicionar a variável de
    **Orquestração Multiagente** **(3)**.

+++AGENT_MODULE=agents.agent_framework.multi_agent.magentic_group+++

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image29.png)

3.  Selecione **File** (1) e, em seguida, clique em **Save** (2).

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

4.  Agora inicie o aplicativo completo de agentes, executando seus três
    componentes principais, seguindo as etapas:

5.  Na janela do Visual Studio Code, clique nas **reticências**
    **(...)** **(1)**, selecione **Terminal** (2) e, em seguida, clique
    em **New Terminal** **(3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

6.  **Inicie o MCP Server (Terminal 1)**: (o diretório mcp está
    localizado no nível raiz do projeto)

    - Execute o comando abaixo para iniciar o **MCP server**, que
      disponibiliza APIs às quais os agentes podem acessar como
      ferramentas. (O servidor é executado
      em [http://localhost:8000](http://localhost:8000/))

> cd mcp
>
> uv run python mcp_service.py

7.  Deixe o comando ser executado e abra um novo terminal.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

8.  **Inicie o Backend (Terminal 2)**:

    - Execute o comando abaixo para iniciar o servidor backend, que
      hospeda seus fluxos de trabalho de agentes, o gerenciamento de
      sessões e os endpoints de API.

> cd agentic_ai/applications
>
> uv run python backend.py

- Esta é a lógica principal do aplicativo com a qual o frontend irá se
  comunicar. Certifique-se de que a **conexão esteja aberta**.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image23.png)

9.  Deixe o comando em execução e abra um novo terminal.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

10. **Inicie o Frontend React (Terminal 3):**

    - Digite o comando abaixo para navegar até o diretório
      react-frontend:

> +++cd agentic_ai/applications/react-frontend+++

- Em seguida, execute o comando abaixo para iniciar o **frontend React**
  da interface do agente. Ele fornece uma interface de usuário para
  interagir com os agentes e visualizar suas respostas em tempo real:

> +++npm start+++

- Quando o **webpack for compilado com sucesso**, a aplicação do agente
  será executada localmente em: http://localhost:3000.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

11. Envie o prompt abaixo no chat e visualize a resposta no painel à
    esquerda:

> +++Customer 251, what's my billing summary?+++

12. O orquestrador atua como um gerente ou roteador. Ele lê a consulta
    do usuário e decide qual agente especializado deve tratá-la. Para
    isso, utiliza o contexto e palavras-chave (como “billing”,
    “promotion”, “login”) para tomar essa decisão.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.png)

13. O orquestrador atribui a tarefa a um agente de domínio. Ele envia a
    consulta para um dos seguintes agentes internos:

    - crm_billing – faturamento, faturas e pagamentos

    - product_promotions – produtos, descontos e ofertas

    - security_authentication – segurança, login e bloqueios de conta

14. Para a sua consulta (“billing summary”), o orquestrador a direciona
    para o agente **crm_billing**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

- O agente de domínio utiliza ferramentas conectadas. Cada agente tem
  acesso a ferramentas específicas (APIs) por meio do MCP server.

- Exemplo: o agente crm_billing pode chamar get_customer_detail,
  get_billing_summary, get_invoice_payment, entre outras.

- O agente aciona a ferramenta correta, obtém dados estruturados e
  constrói uma resposta factual.

15. Após concluir os testes, retorne ao VS Code e encerre todas as
    sessões de terminal em execução. Isso garante que o próximo fluxo de
    trabalho multiagente seja executado sem qualquer interferência.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

### Tarefa 3.2: Sistema Multiagente com Padrão de Transferência de Responsabilidade

Nesta tarefa, você irá explorar um sistema multiagente baseado em
transferência de responsabilidade, no qual as conversas transitam de
forma contínua entre agentes especializados (como Faturamento, Promoções
ou Segurança) com base na intenção do usuário, garantindo interações
suaves, com reconhecimento de contexto, entre diferentes domínios.

- **Como funciona**

  - O usuário interage diretamente com um agente de domínio — por
    exemplo, o Agente de CRM e Faturamento.

  - Um classificador de intenção verifica se a nova mensagem do usuário
    pertence a outro domínio (como promoções ou segurança).

  - Caso pertença, o sistema transfere automaticamente (transferência de
    responsabilidade) a conversa para o agente especialista apropriado.

  - Cada agente possui ferramentas filtradas e relevantes para o seu
    domínio (faturamento, promoções ou segurança).

  - A transferência de responsabilidade ocorre de forma transparente,
    com transferência de contexto, garantindo que o novo agente
    compreenda o histórico da conversa.

1.  Expanda **agents (1) \> agent_framework (2) \> multi_agent (3) \>
    handoff_multi_domain_agent (4)** e visualize o código **(5)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image32.png)

2.  Navegue até o arquivo .env file **(1)**, comente a variável de
    Orquestração Multiagente **(2)** e insira o comando abaixo para
    adicionar a variável **Handoff Pattern Multi-Agent System** **(3)**.

+++AGENT_MODULE=agents.agent_framework.multi_agent.handoff_multi_domain_agent+++

- Insira o comando abaixo para controlar quanto do contexto da conversa
  anterior será transferido durante a transferência de responsabilidade.
  O valor -1 indica que todos os turnos anteriores da conversa serão
  transferidos **(4)**.

> +++HANDOFF_CONTEXT_TRANSFER_TURNS=-1+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

3.  Selecione **File (1)** e, em seguida, clique em **Save (2)**.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

4.  Agora, inicie o aplicativo completo de agentes executando seus três
    componentes principais, seguindo as etapas abaixo:

5.  Na janela do Visual Studio Code, clique nas **reticências**
    **(...)** **(1)**, selecione **Terminal** (2) e, em seguida, clique
    em **New Terminal** **(3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

6.  **Inicie o MCP Server (Terminal 1)**: (o diretório mcp está
    localizado no nível raiz do projeto)

    - Execute o comando abaixo para iniciar o **MCP server**, que
      disponibiliza APIs às quais os agentes podem acessar como
      ferramentas. (O servidor é executado
      em [http://localhost:8000](http://localhost:8000/))

> cd mcp
>
> uv run python mcp_service.py

7.  Deixe o comando ser executado e abra um novo terminal.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

8.  **Inicie o MCP Server (Terminal 2)**:

    - Execute o comando abaixo para iniciar o servidor backend, que
      hospeda seus fluxos de trabalho de agentes, o gerenciamento de
      sessões e os endpoints de API.

> cd agentic_ai/applications
>
> uv run python backend.py

- Esta é a lógica principal da aplicação com a qual o frontend irá se
  comunicar. Certifique-se de que a **conexão esteja aberta**.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image23.png)

9.  Deixe o comando em execução e abra um novo terminal.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

10. **Inicie o Frontend React (Terminal 3):**

    - Digite o comando abaixo para navegar até o diretório
      react-frontend:

> +++cd agentic_ai/applications/react-frontend+++

- Em seguida, execute o comando abaixo para iniciar o **frontend React**
  da interface do agente. Ele fornece uma interface de usuário para
  interagir com os agentes e visualizar suas respostas em tempo real:

> +++npm start+++

- Quando o **webpack for compilado com sucesso**, a aplicação do agente
  será executada localmente em: http://localhost:3000.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

11. Envie o prompt abaixo no chat e visualize a resposta no painel à
    esquerda:

+++Customer 251, what's my billing summary?+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image34.png)

- Aqui, o classificador de intenção direciona a solicitação para o
  domínio crm_billing.

- A ferramenta get_billing_summary é acionada para o Cliente 251.

12. Você pode fornecer a seguinte consulta para dar continuidade ao
    fluxo relacionado ao faturamento:

+++Yes, I would like to view the invoice details+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image35.png)

**Observação**: Caso você receba a resposta “I was unable to retrieve
the invoice details because the referenced numbers are invoice IDs, not
subscription IDs. Please provide the subscription ID or let me know if
you need details for a specific invoice so I can assist you correctly.”,
forneça o prompt abaixo.

+++Yes, I would like to view the invoice details for customer 251+++

13. Agora, vamos testar uma consulta relacionada a outro domínio para
    verificar como funciona a transferência de responsabilidade.

14. Insira a consulta a seguir relacionada a Produtos e Promoções e
    visualize a resposta.

+++Are there any promotions available for my subscription plan+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image36.png)

- Como a conversa anterior foi tratada pelo Especialista de CRM e
  Faturamento, o sistema detecta uma mudança de domínio e decide
  realizar a transferência de responsabilidade da conversa para o
  Especialista de Produtos e Promoções.

- O sistema pode, opcionalmente, transferir o contexto da conversa
  anterior (como qual cliente está sendo discutido) para o novo agente,
  dependendo da configuração HANDOFF_CONTEXT_TRANSFER_TURNS.

- O Especialista de Produtos e Promoções possui acesso apenas às
  ferramentas relevantes para promoções, planos e informações de
  produtos (por exemplo, get_promotions, get_eligible_promotions).

15. Após concluir os testes, retorne ao VS Code e encerre todas as
    sessões de terminal em execução. Isso garante que o próximo fluxo de
    trabalho multiagente seja executado sem qualquer interferência.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

**Resumo**

Neste laboratório, você criou um fluxo de trabalho de agente único que
se integra a ferramentas externas usando o MCP e explorou designs
multiagente nos quais vários agentes especializados colaboram ou
realizam transferência de responsabilidade de conversas com base na
intenção do usuário. Você configurou variáveis de ambiente, iniciou o
ambiente completo de agentes e testou como os agentes respondem de forma
inteligente a consultas específicas de domínio.
