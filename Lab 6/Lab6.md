# Laboratório 6: Desenvolvendo Sistemas Multiagente com Comunicação Agent-to-Agent (A2A)

**Duração estimada:** 30 minutos

**Visão geral**

Neste laboratório, você irá implementar um sistema multiagente usando o
Microsoft Agent Framework. Você definirá funções distintas para os
agentes (Planejador, RH, Conformidade), implantará essas funções e
configurará a comunicação A2A (Agent-to-Agent) para permitir que um
agente acione outros. Você testará um cenário em que uma consulta do
usuário é delegada pela rede de agentes e, em seguida, inspecionará
rastreamentos e logs para confirmar o roteamento correto.

O Microsoft Agent Framework SDK é o novo kit de desenvolvimento oficial
para criar agentes inteligentes e modulares que podem raciocinar,
executar ações e colaborar com outros agentes. Ele oferece:

- Arquitetura de agente unificada – Substitui o AutoGen, o Semantic
  Kernel e os orquestradores fragmentados

- Suporte integrado para o Microsoft Foundry – Implementa agentes
  diretamente no serviço de agentes do Foundry

- Ferramentas via MCP (Model Context Protocol) – Integração padronizada
  com dados, APIs e sistemas

- Comunicação A2A nativa – Os agentes podem acionar outros agentes como
  colaboradores autônomos

Este SDK foi projetado para oferecer suporte a sistemas de agentes
corporativos, prontos para produção, com confiabilidade, observabilidade
e governança incorporadas desde o início.

Objetivos do laboratório

Neste laboratório, você irá realizar as seguintes tarefas:

- Tarefa 1: abrir o projeto pré-configurado no VS Code

- Tarefa 2: criar o Planner Agent

- Tarefa 3: criar agentes de RH e conformidade

- Tarefa 4: definir a lógica de roteamento A2A (gráfico do agente/fluxo
  de trabalho)

- Tarefa 5: testar a conversa multiagente e inspecionar os logs

## Tarefa 1: Abrir o projeto pré-configurado no VS Code

Nesta tarefa, você irá revisar a estrutura de pastas pré-configurada
para entender onde as definições de agentes, fluxos de trabalho e
ferramentas estão organizadas. Isso prepara você para implementar
extensões no sistema usando o Microsoft Agent Framework SDK.

1.  No desktop da LabVM, abra o **Visual Studio Code**.

2.  Com o Visual Studio Code aberto, clique em **File (1)** e selecione
    a opção **Open Folder (2)** para abrir a pasta do código.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

3.  Na janela de seleção de pasta, navegue até C:\Labfiles\Day
    2\Enterprise-Agent-Code-Files e selecione **Select Folder**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

4.  Quando a janela pop-up for exibida, clique em **Yes, I trust the
    authors**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

5.  Revise a estrutura de pastas do enterprise agent.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

6.  Clique com o botão direito no arquivo **.env.example (1)** e
    selecione **Rename (2)**.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image5.png)

7.  Renomeie o arquivo de **.env.example** para **.env**, ativando esse
    arquivo de ambiente para o agente.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image6.png)

8.  Substitua o conteúdo do arquivo .env pelo conteúdo abaixo:

> AZURE_OPENAI_ENDPOINT=https://agentic-
> @lab.LabInstance.Id.cognitiveservices.azure.com/
>
> AZURE_OPENAI_API_KEY=**\<Replace with Azure OpenAI key\>**
>
> AZURE_OPENAI_RESPONSES_DEPLOYMENT_NAME=gpt-4o-mini
>
> AZURE_OPENAI_API_VERSION=2025-03-01-preview

Na página Microsoft Foundry – Overview, copie a API Key e substitua o
valor **\<Replace with Azure OpenAI key\>** no arquivo .env.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

9.  Após concluir, selecione **File (1)** e clique em **Save (2)** para
    salvar o arquivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image9.png)

## Tarefa 2: Criar o Planner Agent

Nesta tarefa, você irá implementar o Planner Agent, responsável por
interpretar consultas do usuário e decidir para qual agente especialista
delegar a solicitação. O agente será configurado usando o Agent
Framework SDK com instruções específicas de função.

1.  Na lista de arquivos, selecione **planner_agent.py** dentro da pasta
    agents.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.png)

2.  Adicione o código Python abaixo para configurar o Planner Agent:

> import os
>
> import asyncio
>
> from agent_framework.azure import AzureOpenAIResponsesClient \# type:
> ignore
>
> async def build_planner_agent():
>
> client = AzureOpenAIResponsesClient(
>
> api_key=os.getenv("AZURE_OPENAI_API_KEY"),
>
> endpoint=os.getenv("AZURE_OPENAI_ENDPOINT"),
>
> deployment_name=os.getenv("AZURE_OPENAI_RESPONSES_DEPLOYMENT_NAME"),
>
> api_version=os.getenv("AZURE_OPENAI_API_VERSION"),
>
> )
>
> return client.create_agent(
>
> name="PlannerAgent",
>
> instructions=(
>
> "You are an intelligent routing agent. Analyze user queries and route
> them to the appropriate specialist. "
>
> "Available specialists:\n"
>
> "- HR: Employee policies, leave, benefits, working hours, performance,
> hiring\n"
>
> "- FINANCE: Reimbursements, expenses, budgets, travel costs, meal
> allowances, equipment purchases\n"
>
> "- COMPLIANCE: GDPR, data privacy, regulatory requirements, legal
> compliance, audits\n\n"
>
> "Return exactly one word: HR, FINANCE, or COMPLIANCE. "
>
> "Consider keywords like: money, cost, budget, reimburse, expense,
> payment, allowance → FINANCE\n"
>
> "Keywords like: leave, sick, vacation, policy, employee, benefits →
> HR\n"
>
> "Keywords like: GDPR, privacy, compliance, legal, audit, regulation →
> COMPLIANCE"
>
> ),
>
> )
>
> async def classify_target(planner_agent, user_query: str) -\> str:
>
> result = await planner_agent.run(
>
> "Analyze and route this query:\n\n"
>
> f"User query: {user_query}\n\n"
>
> "Return exactly one word: HR, FINANCE, or COMPLIANCE."
>
> )
>
> \# Extract the text content from the AgentRunResponse object
>
> text = str(result).strip().lower()
>
> \# Advanced classification with fallback logic
>
> if "finance" in text or "financial" in text:
>
> return "FINANCE"
>
> elif "hr" in text or "human" in text:
>
> return "HR"
>
> elif "compliance" in text or "legal" in text:
>
> return "COMPLIANCE"
>
> else:
>
> \# Fallback keyword analysis if agent response is unclear
>
> query_lower = user_query.lower()
>
> finance_keywords = \["reimburs", "expense", "cost", "budget", "money",
> "payment", "allowance", "travel", "meal", "flight", "hotel"\]
>
> hr_keywords = \["leave", "sick", "vacation", "employee", "benefit",
> "policy", "hire", "performance", "work"\]
>
> compliance_keywords = \["gdpr", "privacy", "compliance", "legal",
> "audit", "regulation", "data protection"\]
>
> finance_score = sum(1 for keyword in finance_keywords if keyword in
> query_lower)
>
> hr_score = sum(1 for keyword in hr_keywords if keyword in query_lower)
>
> compliance_score = sum(1 for keyword in compliance_keywords if keyword
> in query_lower)
>
> if finance_score \> hr_score and finance_score \> compliance_score:
>
> return "FINANCE"
>
> elif hr_score \> compliance_score:
>
> return "HR"
>
> else:
>
> return "COMPLIANCE"

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image11.png)

> **Propósito do Planner Agent:**

- Este agente foi projetado para analisar as consultas dos usuários e
  decidir qual agente especializado (RH, Finanças ou Conformidade) deve
  lidar com a resposta.

> **Criação do agente usando AzureOpenAIResponsesClient:**

- A função build_planner_agent() implementa a inicialização do Planner
  usando o Agent Framework SDK, com credenciais baseadas em API
  carregadas a partir de variáveis de ambiente.

> **Roteamento guiado por LLM (lógica principal):**

- O Planner Agent é instruído a retornar exatamente uma palavra — RH,
  FINANÇAS ou CONFORMIDADE — com base nas palavras-chave e no contexto
  da consulta.

> **classify_target() para tomada de decisão:**

- Esta função primeiro executa uma chamada await agent.run() para
  perguntar ao Planner qual especialista selecionar. Se a resposta não
  for clara, a função implementa uma lógica de fallback baseada em
  análise de palavras-chave.

> **Estratégia híbrida de AI + heurística:**

- O design implementa um roteamento confiável ao combinar raciocínio do
  modelo com pontuação manual por palavras-chave, tornando o Planner
  robusto mesmo quando a saída da AI é vaga.

3.  Após concluir, selecione **File (1)** e clique em **Save (2)** para
    salvar o arquivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

## Tarefa 3: Criar os Worker Agents

Nesta tarefa, você irá implementar agentes específicos por domínio,
responsáveis pelos conhecimentos de RH, Finanças e Conformidade. Cada
agente será registrado no Registro de Agentes permitindo descoberta e
delegação por meio de comunicação A2A.

1.  Na lista de arquivos, selecione **hr_agent.py** dentro da pasta
    agents e adicione o seguinte código Python para implementar a
    configuração do agente de RH.

> import os
>
> import asyncio
>
> from agent_framework.azure import AzureOpenAIResponsesClient \# type:
> ignore
>
> async def build_hr_agent():
>
> client = AzureOpenAIResponsesClient(
>
> api_key=os.getenv("AZURE_OPENAI_API_KEY"),
>
> endpoint=os.getenv("AZURE_OPENAI_ENDPOINT"),
>
> deployment_name=os.getenv("AZURE_OPENAI_RESPONSES_DEPLOYMENT_NAME"),
>
> api_version=os.getenv("AZURE_OPENAI_API_VERSION"),
>
> )
>
> return client.create_agent(
>
> name="HRAgent",
>
> instructions=(
>
> "You are an expert HR policy specialist with deep knowledge of
> employment law and best practices. "
>
> "Answer questions about:\n"
>
> "- Leave policies (sick, vacation, parental, bereavement)\n"
>
> "- Employee benefits (health insurance, retirement, wellness
> programs)\n"
>
> "- Performance management and reviews\n"
>
> "- Hiring, onboarding, and termination procedures\n"
>
> "- Working hours, overtime, and flexible work arrangements\n"
>
> "- Employee relations and conflict resolution\n"
>
> "- Training and development programs\n\n"
>
> "Provide specific, actionable guidance with policy references where
> applicable. "
>
> "Be empathetic and professional in your responses."
>
> ),
>
> )

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image12.png)

> **Propósito do agente de RH:**

- Este agente atua como um especialista dedicado em políticas de RH,
  treinado para responder a perguntas relacionadas ao bem-estar dos
  colaboradores, estruturas de licença, benefícios e procedimentos do
  local de trabalho.

> **Inicialização do agente com Azure Responses Client:**

- A função build_hr_agent() inicializa o agente usando
  AzureOpenAIResponsesClient, autenticado por meio de chaves de API e
  valores de endpoint armazenados em variáveis de ambiente.

> **Especialização específica por domínio:**

- A seção de instruções define claramente o escopo do agente de RH —
  incluindo tipos de licença, benefícios, integração, relações com
  colaboradores e gestão de desempenho — garantindo que ele responda
  apenas a consultas relacionadas a RH.

> **Tom profissional e empático:**

- O agente é projetado para reproduzir padrões reais de comunicação de
  RH, fornecendo orientações precisas, profissionais e empáticas, ideais
  para assistentes organizacionais internos.

> **Base para colaboração multiagente:**

- Depois de implementado, este agente de RH será invocado pelo Planner
  Agent, permitindo delegação automatizada em fluxos de trabalho
  multiagente quando consultas relacionadas a RH forem detectadas.

2.  Após concluir, selecione **File (1)** e clique em **Save (2)** para
    salvar o arquivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

3.  Na lista de arquivos, selecione **finance_agent.py** dentro da pasta
    agents e adicione o seguinte código Python para implementar a
    configuração do agente de finança.

> import os
>
> import asyncio
>
> from agent_framework.azure import AzureOpenAIResponsesClient \# type:
> ignore
>
> async def build_finance_agent():
>
> client = AzureOpenAIResponsesClient(
>
> api_key=os.getenv("AZURE_OPENAI_API_KEY"),
>
> endpoint=os.getenv("AZURE_OPENAI_ENDPOINT"),
>
> deployment_name=os.getenv("AZURE_OPENAI_RESPONSES_DEPLOYMENT_NAME"),
>
> api_version=os.getenv("AZURE_OPENAI_API_VERSION"),
>
> )
>
> return client.create_agent(
>
> name="FinanceAgent",
>
> instructions=(
>
> "You are a finance and reimbursement specialist. Answer questions
> about "
>
> "expense policies, reimbursement limits, budget approvals, travel
> expenses, "
>
> "meal allowances, equipment purchases, and financial procedures.
> Provide "
>
> "specific amounts, policies, and actionable guidance."
>
> ),
>
> )

![A computer screen shot of a program AI-generated content may be
incorrect.](./media/image13.png)

> Função especializada de Finanças:

- Este agente é projetado para lidar com todos os tópicos relacionados a
  Finanças, incluindo políticas de reembolso, orçamentos de viagem,
  auxílios e aprovações de compras.

> **Inicialização via Agent Framework SDK:**

- A função build_finance_agent() cria um agente usando
  AzureOpenAIResponsesClient, utilizando autenticação por chave de API a
  partir de variáveis de ambiente seguras.

> **Instruções focadas em políticas:**

- As instruções do agente limitam claramente sua responsabilidade a
  procedimentos financeiros, garantindo respostas precisas sobre custos,
  pagamentos, orçamentos e regras corporativas de despesas.

> **Precisão e resultados acionáveis:**

- Diferentemente de agentes de uso geral, este assistente de Finanças é
  instruído a fornecer valores específicos de políticas, como limites,
  elegibilidade ou fluxos de aprovação, tornando-o prático para os
  colaboradores.

> **Suporte à delegação do Planner (A2A):**

- Este agente será invocado automaticamente quando o Planner detectar
  palavras-chave ou consultas relacionadas a Finanças, possibilitando
  colaboração multiagente contínua no sistema.

4.  Após concluir, selecione **File (1)** e clique em **Save (2)** para
    salvar o arquivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

5.  Na lista de arquivos, selecione **compliance_agent.py** dentro da
    pasta **agents** e adicione o seguinte código Python para
    **implementar** a configuração do agente de Conformidade.

> import os
>
> import asyncio
>
> from agent_framework.azure import AzureOpenAIResponsesClient \# type:
> ignore
>
> async def build_compliance_agent():
>
> client = AzureOpenAIResponsesClient(
>
> api_key=os.getenv("AZURE_OPENAI_API_KEY"),
>
> endpoint=os.getenv("AZURE_OPENAI_ENDPOINT"),
>
> deployment_name=os.getenv("AZURE_OPENAI_RESPONSES_DEPLOYMENT_NAME"),
>
> api_version=os.getenv("AZURE_OPENAI_API_VERSION"),
>
> )
>
> return client.create_agent(
>
> name="ComplianceAgent",
>
> instructions=(
>
> "You are a senior compliance and legal specialist with expertise in
> multiple jurisdictions. "
>
> "Provide authoritative guidance on:\n"
>
> "- GDPR and data protection regulations (EU, UK, US state laws)\n"
>
> "- Privacy policies and data processing agreements\n"
>
> "- Regulatory compliance (SOX, HIPAA, PCI-DSS, ISO standards)\n"
>
> "- Risk assessment and audit requirements\n"
>
> "- Contract law and vendor agreements\n"
>
> "- Information security policies\n"
>
> "- Cross-border data transfers and adequacy decisions\n"
>
> "- Breach notification requirements\n\n"
>
> "Always provide factual, well-researched answers with relevant legal
> citations. "
>
> "Include practical implementation steps and potential risks. Use
> formal, professional tone."
>
> ),
>
> )

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image14.png)

> **Propósito do agente:**

- Este agente atua como uma autoridade dedicada em jurídico e
  conformidade, responsável por lidar com consultas relacionadas a GDPR,
  estruturas regulatórias, direito contratual, avaliações de risco e
  padrões de segurança.

> **Inicialização do agente:**

- A função build_compliance_agent() utiliza AzureOpenAIResponsesClient
  com autenticação por chave de API para registrar o agente de
  conformidade por meio do Microsoft Agent Framework SDK.

> **Especialização regulatória definida nas instruções:**

- As instruções fornecem um escopo claro de conformidade — incluindo
  regulações globais de privacidade (GDPR, HIPAA, SOX), prontidão para
  auditorias, acordos legais e protocolos de violação — garantindo
  respostas de alta confiabilidade.

> **Tom e expectativas de saída:**

- Este agente é configurado para fornecer respostas em tom formal e
  autoritativo, incluindo citações legais ou recomendações de
  implementação quando aplicável.

> **Função no sistema multiagente:**

- Durante a delegação A2A, o Planner Agent encaminhará consultas
  jurídicas ou relacionadas à conformidade para este especialista,
  mantendo precisão e governança nos fluxos de decisão corporativos.

6.  Após concluir, selecione **File (1)** e clique em **Save (2)** para
    salvar o arquivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

## Tarefa 4: Definir a lógica de roteamento A2A (gráfico do agente/fluxo de trabalho)

Agent-to-Agent (A2A) é uma capacidade central do Microsoft Agent
Framework que permite que um agente delegue tarefas de forma autônoma
para outro agente.

Nesta tarefa, você implementará a lógica de roteamento usando um fluxo
de trabalho do agente para que o Planner possa acionar de forma autônoma
os agentes de RH ou conformidade com base na intenção da consulta. Isso
estabelece uma verdadeira colaboração entre vários agentes.

1.  Na lista de arquivos, selecione **main.py** dentro da pasta agents e
    adicione o seguinte código Python para implementar a configuração do
    fluxo de comunicação A2A. Adicione o código Python a seguir para
    configurar a lógica de roteamento dos agentes.

> import asyncio
>
> import time
>
> import logging
>
> from typing import Dict, Any
>
> from utils.env import load_env
>
> from agents.planner_agent import build_planner_agent, classify_target
>
> from agents.hr_agent import build_hr_agent
>
> from agents.compliance_agent import build_compliance_agent
>
> from agents.finance_agent import build_finance_agent
>
> \# Configure logging
>
> logging.basicConfig(level=logging.INFO, format='%(asctime)s -
> %(levelname)s - %(message)s')
>
> async def run_multi_agent(query: str, agents: Dict\[str, Any\]) -\>
> Dict\[str, Any\]:
>
> """
>
> Advanced multi-agent system with routing, timing, and comprehensive
> response handling.
>
> """
>
> start_time = time.time()
>
> try:
>
> \# Step 1: Route the query
>
> logging.info(f"Routing query: {query\[:50\]}...")
>
> target = await classify_target(agents\["planner"\], query)
>
> logging.info(f"Query routed to: {target}")
>
> \# Step 2: Get response from appropriate agent
>
> agent_mapping = {
>
> "HR": ("hr", "HRAgent"),
>
> "FINANCE": ("finance", "FinanceAgent"),
>
> "COMPLIANCE": ("compliance", "ComplianceAgent")
>
> }
>
> if target in agent_mapping:
>
> agent_key, agent_name = agent_mapping\[target\]
>
> answer = await agents\[agent_key\].run(query)
>
> else:
>
> \# Fallback to HR if routing unclear
>
> logging.warning(f"Unknown target '{target}', falling back to HR")
>
> answer = await agents\["hr"\].run(query)
>
> target = "HR"
>
> agent_name = "HRAgent"
>
> \# Step 3: Process response
>
> response_time = time.time() - start_time
>
> return {
>
> "query": query,
>
> "routed_to": target,
>
> "agent_name": agent_name,
>
> "answer": str(answer),
>
> "response_time": round(response_time, 2),
>
> "timestamp": time.strftime("%Y-%m-%d %H:%M:%S"),
>
> "success": True
>
> }
>
> except Exception as e:
>
> logging.error(f"Error processing query: {e}")
>
> return {
>
> "query": query,
>
> "routed_to": "ERROR",
>
> "agent_name": "ErrorHandler",
>
> "answer": f"I apologize, but I encountered an error processing your
> request: {str(e)}",
>
> "response_time": round(time.time() - start_time, 2),
>
> "timestamp": time.strftime("%Y-%m-%d %H:%M:%S"),
>
> "success": False
>
> }
>
> def format_response(result: Dict\[str, Any\]) -\> str:
>
> """Format the agent response for display."""
>
> status_icon = "✅" if result\["success"\] else "❌"
>
> formatted = f"""
>
> {status_icon} Agent Response Summary:
>
> ┌─ Routed to: {result\['routed_to'\]} ({result\['agent_name'\]})
>
> ├─ Response time: {result\['response_time'\]}s
>
> ├─ Timestamp: {result\['timestamp'\]}
>
> └─ Status: {'Success' if result\['success'\] else 'Error'}
>
> 💬 Answer:
>
> {result\['answer'\]}
>
> """
>
> return formatted
>
> async def run_interactive_mode(agents: Dict\[str, Any\]):
>
> """Interactive mode for real-time queries."""
>
> print("\n🤖 Enterprise Agent System - Interactive Mode")
>
> print("Available agents: HR, Finance, Compliance")
>
> print("Type 'quit' to exit, 'help' for commands\n")
>
> while True:
>
> try:
>
> query = input("Enter your question: ").strip()
>
> if query.lower() in \['quit', 'exit', 'q'\]:
>
> print("👋 Goodbye!")
>
> break
>
> elif query.lower() == 'help':
>
> print("""
>
> 📋 Available Commands:
>
> \- Ask any question about HR, Finance, or Compliance
>
> \- 'quit' or 'exit' - Exit the system
>
> \- 'help' - Show this help message
>
> 🎯 Example questions:
>
> \- "What's the travel reimbursement limit for meals?"
>
> \- "How many vacation days do employees get?"
>
> \- "Do we need GDPR compliance for EU customers?"
>
> """)
>
> continue
>
> elif not query:
>
> continue
>
> result = await run_multi_agent(query, agents)
>
> print(format_response(result))
>
> except KeyboardInterrupt:
>
> print("\n👋 Goodbye!")
>
> break
>
> except Exception as e:
>
> logging.error(f"Interactive mode error: {e}")
>
> print(f"❌ Error: {e}")
>
> async def run_batch_tests(agents: Dict\[str, Any\]):
>
> """Run predefined test queries."""
>
> test_queries = \[
>
> "How much reimbursement is allowed for international flights?",
>
> "Is employee data protected under GDPR?",
>
> "How many sick leave days do employees get?"
>
> \]
>
> print("🧪 Running batch tests...\n")
>
> for i, query in enumerate(test_queries, 1):
>
> print(f"{'='\*80}")
>
> print(f"TEST {i}/{len(test_queries)}: {query}")
>
> print(f"{'='\*80}")
>
> result = await run_multi_agent(query, agents)
>
> print(format_response(result))
>
> \# Small delay between queries for better readability
>
> if i \< len(test_queries):
>
> await asyncio.sleep(0.5)
>
> async def main():
>
> """Main application entry point with enhanced features."""
>
> print("🚀 Initializing Enterprise Agent System...")
>
> try:
>
> \# Load environment and build agents
>
> load_env()
>
> logging.info("Building agent network...")
>
> agents = {
>
> "planner": await build_planner_agent(),
>
> "hr": await build_hr_agent(),
>
> "compliance": await build_compliance_agent(),
>
> "finance": await build_finance_agent()
>
> }
>
> logging.info("✅ All agents initialized successfully")
>
> \# Check if running interactively or in batch mode
>
> import sys
>
> if len(sys.argv) \> 1 and sys.argv\[1\] == "--interactive":
>
> await run_interactive_mode(agents)
>
> else:
>
> await run_batch_tests(agents)
>
> except Exception as e:
>
> logging.error(f"System initialization failed: {e}")
>
> print(f"❌ Failed to start system: {e}")
>
> if \_\_name\_\_ == "\_\_main\_\_":
>
> asyncio.run(main())

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image15.png)

> **Mecanismo central de execução:**

- Este script atua como o orquestrador central, coordenando todos os
  agentes (Planner, RH, Finanças e Conformidade) e gerenciando o
  roteamento multiagente usando o Microsoft Agent Framework.

> **Inicialização da rede de agentes:**

- Ele carrega as configurações de ambiente, cria cada agente com await
  build\_\*\_agent() e os registra em um dicionário compartilhado para
  facilitar a delegação.

> **Roteamento A2A avançado:**

- A função run_multi_agent() roteia as consultas do usuário para o
  especialista correto por meio do Planner e, em seguida, aguarda a
  resposta do agente especialista. Ela captura o roteamento, o tempo, o
  status de sucesso e a resposta final.

> **Múltiplos modos de execução:**

- Batch Mode: Executa consultas de teste predefinidas.

- Interactive Mode (--interactive): Habilita chat em tempo real para
  testes e exploração ao vivo.

> **Resiliência pronta para produção:**

- Inclui formatação de respostas, carimbos de data e hora, mecanismos de
  fallback para erros e logging — estabelecendo uma base sólida para
  observabilidade, telemetria e AgentOps em exercícios posteriores.

2.  Após concluir, selecione **File (1)** e clique em **Save (2)** para
    salvar o arquivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

## Tarefa 5: Testar a conversa multiagente e inspecionar os logs

Nesta tarefa, você irá executar testes ponta a ponta por meio do sistema
multiagente e observar a colaboração entre os agentes usando logs e
telemetria no Microsoft Foundry.

1.  Você configurou com sucesso o sistema multiagente com um agente
    Planner e um agente Worker. Agora, você irá testar o funcionamento
    desse sistema multiagente.

> **Observação:** embora o sistema multiagente já esteja configurado com
> capacidades de LLM, ele ainda não possui integração com MCP nem acesso
> a fontes de conhecimento externas, como conjuntos de dados ou índices
> do Azure AI Search. Nesta etapa, os agentes dependerão exclusivamente
> da inteligência geral do modelo para responder às perguntas.

2.  Selecione a opção **... (1)** no menu superior para expandir o menu.
    Selecione **Terminal (2)** e clique em **New Terminal (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image16.png)

3.  Após o terminal ser aberto, execute o comando abaixo para instalar
    todos os pacotes necessários:

+++pip install -r requirements.txt+++.

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image17.png)

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image18.png)

4.  Depois que a instalação for concluída com sucesso, execute o comando
    a seguir para iniciar o agente e revisar as respostas para os
    prompts de teste fornecidos no arquivo de código.

+++python main.py+++

> ![A computer screen shot of a program AI-generated content may be
> incorrect.](./media/image19.png)

Verifique o parâmetro **Routed to** e analise como o agente está
determinando e roteando as solicitações para os respectivos agentes de
trabalho.

5.  Em seguida, execute o agente novamente em modo interativo
    adicionando o parâmetro --interactive. Isso permite inserir a
    pergunta manualmente e receber a resposta. Quando solicitado,
    utilize o prompt abaixo.

    - Comando:

> +++python main.py –interactive+++

- Prompt:

> +++How much reimbursement is allowed for international flights?+++

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image20.png)

6.  Após receber a resposta, no próximo prompt, digite q para sair ou
    interromper a execução do agente.

![A black screen with white text AI-generated content may be
incorrect.](./media/image21.png)

**Resumo**

Neste laboratório, você definiu três agentes (Planejamento, RH e
Conformidade) usando o Microsoft Agent Framework SDK e os registrou.
Você construiu um fluxo de roteamento para delegar consultas de usuários
por meio de chamadas Agent-to-Agent (A2A). Além disso, você testou um
cenário multiagente e inspecionou os logs para confirmar o roteamento
correto das mensagens e o fluxo de execução.
