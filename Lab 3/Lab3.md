# Laboratório 3: Criar um Agente de AI para Análise de Planos de Seguro Saúde

**Visão geral**

Neste laboratório, você irá criar um Agente de AI para Análise de Planos
de Seguro Saúde, projetado especificamente para processar e analisar
dados de planos de seguro saúde. Esse agente inteligente automatiza a
comparação entre diferentes planos de benefícios de saúde ao interpretar
detalhes dos planos, analisar opções de cobertura e gerar representações
visuais para apoiar a tomada de decisão. Utilizando o Microsoft Foundry
e os serviços do Azure AI, o agente criará gráficos de barras
comparativos que exibem claramente as diferenças entre os planos de
seguro, facilitando para os usuários a compreensão de suas opções e a
escolha da cobertura de seguro saúde mais adequada.

**Objetivos do Laboratório**

Você executará a seguinte tarefa neste laboratório:

- Tarefa 1: Criar um Agente de AI Simples

## Tarefa 1: Criar um Agente de AI Simples

Nesta tarefa, você irá criar um Agente de AI simples que processa dados
e gera um gráfico de barras comparando diferentes planos de benefícios
de saúde, utilizando serviços do Azure AI para análise e visualização.

1.  Abra o arquivo **Lab 2 - Create A Simple AI Agent.ipynb**. O
    notebook **Lab 2 - Create A Simple AI Agent.ipynb** orienta você
    sobre como criar um Agente de AI simples que processa dados e gera
    um gráfico de barras comparando diferentes planos de benefícios de
    saúde.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

2.  Selecione a opção **Select kernel** disponível no canto superior
    direito. Selecione **venv (Python 3.x.x)** na lista.

![A blue and red rectangle with white text AI-generated content may be
incorrect.](./media/image2.png)

3.  Execute a célula abaixo para importar as bibliotecas necessárias e
    carregar as variáveis de ambiente para trabalhar com projetos do
    Azure AI. Essa configuração permite a autenticação segura e a
    interação com os serviços do Azure AI.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image3.png)

4.  Execute a célula abaixo para conectar-se ao seu projeto do Microsoft
    Foundry e acessar o modelo **gpt-4o** implementado. Isso estabelece
    uma conexão segura usando o project connection string e as
    credenciais do Azure.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image4.png)

5.  Execute esta célula para criar um **agente de AI simples** que
    processa dados e gera um gráfico de barras comparando diferentes
    planos de saúde usando o Microsoft Foundry. Este script inicializa o
    agente de AI, envia um prompt contendo dados do plano de saúde e
    solicita um gráfico de barras. O agente processa a solicitação, gera
    o gráfico, salva o arquivo de imagem e, em seguida, finaliza
    removendo o agente.

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image5.png)

6.  Por fim, observe o resultado.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image6.png)

**Resumo**

Neste laboratório, você criou com sucesso um agente de AI para análise
de planos de seguro saúde usando o Microsoft Foundry e os serviços do
Azure AI para automatizar a análise e a comparação de planos de
benefícios de saúde. Você aprendeu como se conectar ao seu projeto do
Microsoft Foundry, acessar o modelo GPT-4o implementado e criar um
agente inteligente que processa dados complexos de planos de saúde. O
agente interpretou os detalhes do plano, analisou as opções de cobertura
e gerou automaticamente gráficos de barras comparativos para visualizar
as diferenças entre os planos de seguro. Essa experiência prática
demonstrou como os agentes de AI podem otimizar a análise de dados,
apoiar os processos de tomada de decisão e ajudar os usuários a
compreender e comparar facilmente suas opções de seguro saúde.

Você concluiu este laboratório com sucesso. Clique em Next \>\> para
prosseguir.
