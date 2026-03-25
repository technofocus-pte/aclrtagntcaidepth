# Laboratório 13: Implementação de Detecção de Fraude Corporativa com AI Human-in-the-Loop

**Duração Estimada:** 60 minutos

**Visão Geral**

Você é um Engenheiro de AI na Contoso Ltd., responsável por implementar
fluxos de trabalho de human-in-the-loop (HITL) com AI. Neste
laboratório, você irá explorar o Fluxo de Trabalho de Detecção e
Resposta a Fraudes da Contoso, no qual agentes de AI analisam atividades
suspeitas e direcionam ações de alto risco para analistas humanos para
revisão, utilizando um painel em tempo real baseado em React + FastAPI
para monitoramento e interação.

Objetivo do Laboratório

Você executará as seguintes tarefas neste laboratório:

- Tarefa 1: Implementação de Fluxos de Trabalho de Human-in-the-Loop AI
  com o Azure Agent Framework

## Tarefa 0: Configurar o código 

1.  No diretório C:\Labfiles\Day 3, extraia o arquivo
    **OpenAIWorkshop-Framework**.

2.  Clique no **Visual Studio Code** a partir da área de trabalho da
    LabVM.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

3.  Selecione **File (1)** e clique em **Open Folder (2)** para abrir a
    pasta **OpenAIWorkshop-Framework**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

4.  Navegue até o caminho C:\Labfiles\Day
    3\\**OpenAIWorkshop-Framework**, selecione a pasta
    **OpenAIWorkshop-Framework** e, em seguida, clique em **Select
    Folder**.

5.  Selecione **Yes, I trust the authors**.

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image3.png)

6.  Clique nas reticências **(...) (1)**, em seguida selecione
    **Terminal (2)** e depois **New Terminal (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

7.  Insira o comando abaixo para navegar até o diretório
    **applications** e instalar todas as dependências necessárias a
    partir dos arquivos **pyproject.toml / uv.lock**.

> cd agentic_ai/applications
>
> uv sync

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

**Observação:** Se você encontrar algum erro, execute os comandos
informados abaixo.

> +++pip install uv+++

+++uv sync+++

8.  O comando pode levar de 5 a 10 minutos para ser concluído.
    **Enquanto isso, você pode prosseguir com a Tarefa 1**.

## Tarefa 1: Implementando fluxos de trabalho de AI com Human-in-the-Loop usando o Azure Agent Framework

Neste laboratório, você implementará um fluxo de trabalho
Human-in-the-Loop (HITL) para o sistema de detecção de fraudes da
Contoso. Você executará a detecção de fraudes com vários agentes,
analisará alertas de alto risco, tomará decisões humanas e visualizará o
fluxo de trabalho em tempo real usando o painel React + FastAPI.

1.  No Visual Studio Code, expanda **agentic_ai (1)** \> **workflow
    (2)** \> **fraud_detection (3)**, selecione
    **fraud_detection_workflow.py (4)** e visualize o código **(5)**.

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image6.png)

2.  Em **fraud_detection (1)**, clique com o botão direito em
    **.env.sample (2)** e selecione **Rename (3)**.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image7.png)

3.  Renomeie o arquivo para .env e clique nele para abrir o arquivo.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image8.png)

4.  Substitua os valores de AZURE_OPENAI_API_KEY **(1)** e
    AZURE_OPENAI_ENDPOINT **(2)** pelos valores reais que você copiou no
    laboratório anterior.

5.  Adicione AZURE_OPENAI_CHAT_DEPLOYMENT com o valor **gpt-4o-mini
    (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.png)

- Navegue até o **Microsoft Foundry portal**, selecione **Overview
  (1)**, selecione **Azure OpenAI (2)**. Copie a **Azure OpenAI key
  (3)** e o **Azure OpenAI endpoint (4)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.png)

6.  Selecione **File (1)** e, em seguida, **Save (2)**.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

7.  Na janela do Visual Studio Code, clique nas reticências **(...)
    (1)**, selecione **Terminal (2)** e, em seguida, **New Terminal
    (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

8.  Execute o comando abaixo.

> cd mcp
>
> uv run python mcp_service.py

9.  Deixe o comando em execução e abra um novo terminal.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

10. Insira o comando abaixo para executar o fluxo de trabalho com a
    linha de comando.

> cd agentic_ai/workflow/fraud_detection
>
> uv run python fraud_detection_workflow.py
>
> ![A black screen with white text AI-generated content may be
> incorrect.](./media/image12.png)

**Observação:** O comando pode levar de 5 a 10 minutos para ser
concluído. Aguarde até que o processo seja finalizado.

11. O exemplo inclui três alertas de amostra:

    - **Alerta 1: Login em múltiplos países** (gravidade alta)

    - alert_id: "ALERT-001"

    - customer_id: **1**

    - alert_type: "multi_country_login"

    - descrição: "Tentativas de login a partir dos EUA e da Rússia
      dentro de 2 horas."

gravidade: "alta"

- **Alerta 2: Pico de dados** (Gravidade média)

- alert_id: "ALERT-002"

- customer_id: 2

- alert_type: "data_spike"

- descrição: "O uso de dados aumentou em 500% nas últimas 24 horas."

gravidade: "média"

- **Alerta 3: Cobranças incomuns** (gravidade alta)

- alert_id: "ALERT-003"

- customer_id: 3

- alert_type: "unusual_charges"

- descrição: “Três compras grandes, totalizando US$ 5.000 em 10
  minutos.”

gravidade: “alta”

12. Após a execução bem-sucedida, você verá o terminal conforme abaixo.
    Selecione a ação com base na gravidade do risco. Se a gravidade do
    risco for ≥0,6, será necessária uma revisão humana.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

13. Como a gravidade do risco é alta, você pode digitar 2 para bloquear
    a conta do cliente **(1).**

    - Insira as notas do analista: Alto risco confirmado pelas três
      análises. Ação imediata: bloquear a conta para impedir o acesso
      não autorizado. **(2)**

    - Digite a identificação do analista (padrão: analyst_cli):
      Pressione **Enter** **(3)**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

14. Quando o fluxo de trabalho estiver concluído, você receberá um
    resultado como este.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.png)

15. Quando o comando for executado com sucesso, **exclua todas as
    sessões de terminal em execução existentes.**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image16.png)

## UI de visualização de fluxo de trabalho em tempo real para o fluxo de trabalho de detecção e resposta a fraudes da Contoso

Você utilizará a UI de visualização de fluxo de trabalho em tempo real
para o fluxo de trabalho de detecção e resposta a fraudes da Contoso.
Você iniciará todos os serviços (MCP Server, backend, frontend),
selecionará alertas de exemplo, observará a execução do fluxo de
trabalho em tempo real, revisará alertas de fraude de alto risco,
enviará decisões do analista e monitorará os fluxos de eventos em tempo
real.

1.  Abra um novo terminal.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

2.  Inicie todos os serviços (3 terminais):

    - Terminal 1 - MCP Server:

> cd mcp
>
> uv run mcp_service.py

- Terminal 2 - FastAPI Backend:

> cd agentic_ai/workflow/fraud_detection
>
> uv run --prerelease allow backend.py
>
> ![A screen shot of a computer program AI-generated content may be
> incorrect.](./media/image17.png)

- Terminal 3 - React Frontend:

> cd agentic_ai/workflow/fraud_detection/ui
>
> npm run dev
>
> **Observação:** se ocorrer algum erro, execute o comando +++npm
> install+++ e, em seguida, execute novamente o comando +++npm run
> dev+++..
>
> ![A computer screen with white text AI-generated content may be
> incorrect.](./media/image18.png)

- Pressione **ctrl + click** em http://localhost:3000 para abrir o
  aplicativo em um navegador

> ![A screen shot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

3.  Visualizar a interface do usuário do Visualizador de fluxo de
    trabalho em tempo real.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

4.  Você pode ver exemplos de alertas no menu suspenso **Select
    Alerts**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image21.png)

**Observação:** você poderá ver os alertas no menu suspenso somente após
a conexão ser aberta no segundo terminal (backend.py). Certifique-se de
que a conexão esteja aberta**.**

5.  **Selecione o alerta:** escolha entre três alertas de amostra
    (ALERT-001, ALERT-002, ALERT-003) **(1)**

    - Clique em **Start Workflow** **(2)** para iniciar o processamento.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

6.  **Assista às atualizações em temporeal:** os nós mudam de cor à
    medida que os executores são executados

    - 🔵 Azul = Em execução

    - 🟢 Verde = Concluído

    - ⚪ Cinza = Inativo

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

7.  **Revisão do Analista:** quando uma fraude de alto risco é
    detectada, um painel de revisão é exibido.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

8.  **Submit Decision**: escolha a ação e adicione observações

    - Your Decision: se a gravidade for alta, selecione **Lock Account**
      **(1)**

    - Analyst notes: insira High risk, quando confirmado a partir das
      três análises. Ação imediata: bloqueio da conta para evitar acesso
      não autorizado. **(2)**

    - Selecione **SUBMIT WORKFLOW (3)**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

9.  **Monitorar Eventos:** o painel à direita exibe o fluxo completo de
    eventos.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

**Resumo**

Neste laboratório, você implementou um fluxo de trabalho
human-in-the-loop (HITL) para detecção de fraude usando o Azure Agent
Framework. Você explorou como agentes de AI analisam atividades
suspeitas, direcionam casos de alto risco para analistas humanos e
interagem com um painel em tempo real baseado em React + FastAPI para
monitorar a execução do fluxo de trabalho e enviar decisões.
