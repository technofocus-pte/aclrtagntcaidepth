# Laboratório 2: Configurar um Projeto de AI e realizar a Conclusão do Chat a partir do VS Code

**Visão geral**

Neste laboratório, você irá preparar o ambiente completo de
desenvolvimento necessário para criar agentes de AI, criando e
configurando um Projeto de AI no Microsoft Foundry, implementando um
Large Language Model (LLM) e um modelo de embedding, além de conectar o
projeto ao Visual Studio Code. Em seguida, você validará a configuração
executando uma conclusão de chat simples a partir do código, garantindo
que seu ambiente esteja corretamente configurado e pronto para
desenvolver aplicações com AI.

Objetivos do Laboratório

Você executará as seguintes tarefas neste laboratório.

- Tarefa 1: Configurar o Projeto de AI no Microsoft Foundry

- Tarefa 2: Implementar um LLM e modelos de embeddings

- Tarefa 3: Instalar dependências, criar um ambiente virtual e criar um
  arquivo de variáveis de ambiente

## Tarefa 1: Configurar o Projeto de AI no Microsoft Foundry

Nessa tarefa, você criará e configurará um Projeto de AI no Microsoft
Foundry. Isso envolve configurar os recursos necessários, definir os
parâmetros do projeto e garantir que o ambiente esteja pronto para a
implementação de modelos de AI. Ao final dessa tarefa, você terá um
Projeto de AI totalmente inicializado, servindo como base para
desenvolvimentos e experimentações futuras.

1.  Na página do Portal do Azure, na caixa Search resources no topo do
    portal, na parte superior do portal, digite **Microsoft Foundry
    (1)** e selecione **Microsoft Foundry (2)** em Services.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image1.png)

2.  No painel de navegação esquerdo de **Use with Foundry**, selecione
    **AI Hubs (1)**. Na página **AI Hubs**, clique em **Create (2)** e
    selecione **Hub (3)** no menu suspenso.

![](./media/image2.png)

3.  No painel **Create an Azure AI hub**, informe os seguintes dados em
    **Basics (1):**

    - Subscription: **mantenha a assinatura padrão (2)**

    - Resource group : **AgenticAI (3)**

    - Region : **East US2** (4)

    - Name: **<+++ai-foundry-hub@lab.LabInstance.Id>+++ (5)**

    - Connect AI Services incl. OpenAI: Clique em **Create New (6).**

    - Connect AI Services incl. OpenAI: Forneça o nome
      **<+++my-ai-service@lab.LabInstance.Id>+++ (7)**.

    - Clique em **Save (8)** e depois em **Next: Storage (9)**

> ![](./media/image3.png)

4.  Clique na aba **Review + Create** e, em seguida, em **Create.**

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image4.png)

![](./media/image5.png)

5.  Aguarde a conclusão da implementação e clique em **Go to
    resource**..

![](./media/image6.png)

6.  No painel **Overview**, clique em **Launch Azure AI Foundry**. Isso
    irá redirecioná-lo para o portal do Microsoft Foundry.

![](./media/image7.png)

7.  Role a página para baixo e clique em **+ New project** no Hub
    Overview.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image8.png)

8.  Forneça o nome do projeto como
    **<+++ai-foundry-project@lab.LabInstance.Id>+++ ,** depois clique em
    **Create (2)**.

![](./media/image9.png)

9.  Após a criação do projeto, role a página para baixo, copie o
    **Project connection string** e salve-o no Bloco de Notas ou em um
    local seguro, pois ele será necessário nas próximas tarefas.

![Uma captura de tela de um projeto O conteúdo gerado por IA pode estar
incorreto.](./media/image10.png)

## Tarefa 2: Implementar um LLM e modelos de embeddings

Nesta tarefa, você irá implantar um Large Language Model (LLM) e um
modelo de embeddings no seu projeto do Microsoft Foundry. Esses modelos
serão utilizados em aplicações baseadas em AI e em recursos de busca
vetorial nos próximos laboratórios.

1.  No seu **projeto do Microsoft Foundry**, navegue até **My assets
    (1)**, selecione **Models + endpoints (2)**, clique em **Deploy
    model (3)** e escolha **Deploy base model (4)**.

![](./media/image11.png)

2.  Na janela **Select a model**, pesquise por **gpt-4o (1)**, selecione
    **gpt-4o (2)** e clique em **Confirm (3)**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image12.png)

3.  Na janela **Deploy model gpt-4o**, selecione **Customize** e
    configure:

![](./media/image13.png)

- Deployment Name: **gpt-4o (1)**

- Deployment type: **Global Standard (2)**

- Model version: **2024-08-06 (Default) (3)**

- Tokens per Minute Rate Limit: **200K (4)**

- Clique em **Connect and Deploy (5)**

![](./media/image14.png)

4.  Clique em **Models + Endpoints (1)** para visualizar o modelo
    **gpt-4o (2)** implementado.

![](./media/image15.png)

5.  Volte ao **Azure Portal**, pesquise por **Open AI (1)** e selecione
    o recurso **Azure Open AI (2)**.

![](./media/image16.png)

6.  Na página **Microsoft Foundry | Azure OpenAI**, clique em **+ Create
    (1)** e selecione **Azure OpenAI (2)**.

![](./media/image17.png)

7.  Na página **Create Azure OpenAI**, informe as seguintes
    configurações e clique em **Next (6)**:

[TABLE]

> ![](./media/image18.png)

8.  Clique em **Next** até aparecer a aba **Review + submit**.

9.  Na página **Review + submit**, clique em **Create.**

![](./media/image19.png)

10. Aguarde a conclusão da implementação e clique em **Go to resource**.

![](./media/image20.png)

11. Na **página de recursos** my-openai-service, selecione **Go to
    Foundry portal**.

![](./media/image21.png)

12. No seu projeto do AI Foundry, navegue até a seção **Shared
    resources, selecione Deployments (1), clique em Deploy model (2) e
    escolha Deploy base model (3)** para prosseguir.

![](./media/image22.png)

**Observação:** O assistente de importação e vetorização do Azure AI
Search, utilizado em laboratórios posteriores, ainda não oferece suporte
a modelos de embeddings de texto diretamente no AI Foundry. Por isso, é
necessário criar um serviço Azure OpenAI e implantar o modelo de
embeddings nele. Usaremos esse modelo de embedding de texto mais tarde,
quando criarmos nosso índice vetorial.

13. Na janela **Select a model**, pesquise por **text-embedding-3-large
    (1)**, selecione **text-embedding-3-large (2)** e clique em
    **Confirm (3)**.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image23.png)

14. Na janela Deploy model text-embedding-3-large, configure:

    - Deployment type: **Standard (1)**

    - Tokens per Minutes Rate Limit: **120K (2)**

    - Clique em **Deploy (3)** para implementar o modelo.

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image24.png)

15. Clique em **Deployment (1)** para visualizar o **modelo**
    text-embedding-3-large (2) implementado.

![](./media/image25.png)

## Tarefa 3: Instalar dependências, criar um ambiente virtual e criar um arquivo de variáveis de ambiente

Nesta tarefa, você irá instalar as dependências necessárias, configurar
um ambiente virtual e criar um arquivo de variáveis de ambiente para
garantir um ambiente de desenvolvimento controlado e o gerenciamento
seguro das configurações do projeto de AI.

1.  Na sua **VM do Laboratório**, abra o **Visual Studio Code**.

2.  Clique em **File (1**) e depois em **Open Folder**.

![](./media/image26.png)

3.  Navegue até C:\LabFiles\Day-1 \\ **(1)**, selecione a pasta
    **azure-ai-agents-labs (2)** e clique em **Select folder (3).**

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image27.png)

4.  Clique em **Yes, I Trust the authors**,

![](./media/image28.png)

5.  Clique nos **três pontos (…) (1),** depois em **Terminal (2)** e em
    **New Terminal (3).**

![Uma captura de tela de um computador O conteúdo gerado por IA pode
estar incorreto.](./media/image29.png)

6.  Certifique-se de que você está no diretório do projeto
    **azure-ai-agents-labs** e execute os comandos abaixo no PowerShell
    para criar e ativar o ambiente virtual:

7.  python -m venv venv

+++venv/Scripts/activate+++

![](./media/image30.png)

8.  Execute o comando PowerShell abaixo. Isso instala todos os pacotes
    necessários:

9.  pip install -r requirements.txt

+++pip install azure-ai-ml azure-identity+++

![Uma captura de tela de um código de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image31.png)

10. Execute o comando abaixo do PowerShell para instalar ou atualizar o
    pip para a versão mais recente.

+++python.exe -m pip install --upgrade pip+++

![](./media/image32.png)

11. Execute o comando abaixo para acessar sua conta do Azure.

+++az login+++

12. Selecione a conta de usuário para autorizar.

13. Uma vez concluída a Autorização, retorne ao Visual Studio Code.

![](./media/image33.png)

14. Abra o arquivo **Sample.env** e configure as variáveis de ambiente
    necessárias.

![](./media/image34.png)

- Navegue até o **Microsoft Foundry portal**, clique no modelo **gpt-4o
  (2)** na seção **Models + endpoints (1)** em **My assets**, copie as
  informações em **Endpoint** no painel direito e copie e cole o
  **Target URI (1)** e a **Key (2)** em um bloco de notas.

![](./media/image35.png)

![](./media/image36.png)

15. No **arquivo Sample.env**,

    - AIPROJECT_CONNECTION_STRING: Informe o valor do **Project
      connection string** que você copiou no passo 9 da Tarefa 1

    - CHAT_MODEL_ENDPOINT: Informe o **Target URI** do modelo **gpt-4o**
      copiado no passo anterior

    - CHAT_MODEL_API_KEY: Informe o valor da **Key** do modelo
      **gpt-4o** copiado no passo anterior

    - CHAT_MODEL: **GPT-4O**

![](./media/image37.png)

16. Salve as alterações no **arquivo Sample.env**.

17. Execute o comando PowerShell abaixo. Isso cria o arquivo **.env**:

+++cp sample.env .env+++

![](./media/image38.png)

18. Em seguida, abra o arquivo **Lab 1 - Project Setup.ipynb**. O
    notebook **Lab 1 - Project Setup.ipynb** orienta você na
    configuração de um projeto de AI no Microsoft Foundry, na
    implementação de um LLM e de modelos de embedding, além de
    configurar a conectividade do VS Code. Também inclui uma chamada
    simples de API de Conclusão de Chat para validar a configuração. A
    execução deste notebook garante que o ambiente esteja corretamente
    configurado para o desenvolvimento de aplicativos alimentados por
    AI.

![Uma captura de tela de um programa de computador O conteúdo gerado por
IA pode estar incorreto.](./media/image39.png)

19. Selecione a opção **Select kernel (1)** disponível no canto superior
    direito e escolha **Install/enable selected extensions
    (python+jupyter) (2)**.

![](./media/image40.png)

20. Selecione **Python Environments** para garantir que o Jupyter
    Notebook seja executado no interpretador Python correto, com as
    dependências necessárias instaladas.

![](./media/image41.png)

21. Selecione **venv (Python 3.x.x)** na lista, pois essa versão
    provavelmente é necessária para compatibilidade com o Microsoft
    Foundry SDK e outras dependências.

![](./media/image42.png)

**Observação:** Se **venv (Python 3.x.x)** não aparecer na lista, feche
e reabra o **Visual Studio Code**.

22. Execute a primeira célula para importar as bibliotecas Python
    necessárias para trabalhar com serviços do Azure AI.

![](./media/image43.png)

23. Execute a célula abaixo para recuperar o project connection string e
    o nome do modelo a partir das variáveis de ambiente. Esses valores
    são necessários para interagir com o Large Language Model (LLM) de
    forma segura, sem codificar informações sensíveis diretamente no
    código.

![](./media/image44.png)

24. Execute a célula abaixo para conectar ao seu projeto do Microsoft
    Foundry usando o connection string. Isso estabelece uma conexão
    segura com o AIProjectClient, permitindo interações com os recursos
    do projeto.

![](./media/image45.png)

25. Execute a célula abaixo para interagir com o modelo GPT-4o usando
    seu projeto do Microsoft Foundry. Esse código inicializa um cliente
    de chat, envia uma solicitação para gerar uma piada sobre um ursinho
    de pelúcia e imprime a resposta. Por fim, visualize a saída
    retornada pelo modelo de chat.

![](./media/image46.png)

> **Observação:** A saída nesta etapa é gerada dinamicamente pelo modelo
> de AI e pode variar a cada execução.

**Resumo**

Neste laboratório, você configurou com sucesso um ambiente completo de
desenvolvimento de AI ao criar e configurar um projeto de AI no
Microsoft Foundry, implantar o GPT-4o Large Language Model e o modelo de
embedding text-embedding-3-large, e estabelecer conectividade segura a
partir do Visual Studio Code. Você instalou as dependências necessárias,
criou um ambiente virtual e configurou variáveis de ambiente para
gerenciar informações sensíveis de forma segura. Por fim, validou a
configuração executando uma chamada simples da API de Conclusão de Chat,
confirmando que seu ambiente está corretamente configurado e pronto para
o desenvolvimento de aplicativos baseados em AI.

Você concluiu este laboratório com sucesso. Clique em Next \>\> para
prosseguir.
