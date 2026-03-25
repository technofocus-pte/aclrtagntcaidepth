# Laboratório 8: Implementação de Agentes e Gerenciamento do Ambiente de Execução no Microsoft Foundry

**Duração Estimada:** 15 minutos

**Visão Geral**

Neste laboratório, você irá implementar seu sistema multiagente
(desenvolvido usando o Microsoft Agent Framework SDK) no Microsoft
Foundry Agent Service. Você publicará o agente configurado em um
ambiente de execução gerenciado.

Você criou um Agente de Resposta de Chat até agora, o que significa que:

- Ele lida com interações únicas e sem estado, respondendo imediatamente
  às entradas do usuário.

- Ele é executado localmente dentro do seu aplicativo ou SDK, sem um
  backend persistente.

- Cada solicitação é processada de forma independente, sem retenção de
  memória ou contexto de longo prazo.

- É ideal para experiências rápidas de chat ou para testar a lógica
  central antes da implementação completa.

Agora, você atualizará isso para ser um Agente Persistente no Microsoft
Foundry, o que significa que:

- Ele é executado como um serviço gerenciado e de longa duração dentro
  do ambiente de execução do Foundry.

- Ele pode manter estado e contexto entre sessões, permitindo
  continuidade e aprendizado.

- Ele oferece suporte à integração com ferramentas externas e outros
  agentes usando os protocolos MCP e A2A.

- Ele é otimizado para confiabilidade em escala corporativa,
  monitoramento e conformidade.

**Objetivos do laboratório**

Você executará a seguinte tarefa neste laboratório:

- Tarefa 1: Implementar agentes no Microsoft Foundry Agent Service

## Tarefa 1: Implementar agentes no Microsoft Foundry Agent Service

Nesta tarefa, você atualizará o agente existente para ser um agente
persistente e publicará cada agente no Microsoft Foundry Agent Service
como um modelo independente.

1.  Agora, você precisa atualizar os arquivos de código para dar suporte
    ao sistema de agente persistente, que implementa o registro do
    agente no Microsoft Foundry Agent Service.

2.  No painel do Visual Studio Code, no menu à esquerda, selecione o
    arquivo .env para atualizar as chaves do projeto AI Foundry.

3.  Adicione as seguintes variáveis ao arquivo:

> \# Azure AI Project Configuration
>
> AZURE_AI_PROJECT_ENDPOINT=**\<Microsoft Foundry endpoint\>**
>
> AZURE_AI_MODEL_DEPLOYMENT_NAME=gpt-4o-mini
>
> Localize o endpoint do projeto do Microsoft Foundry na página Overview
> e substitua **\<Microsoft Foundry endpoint\>** por esse valor.
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image1.png)

![](./media/image2.png)

4.  Após a atualização, o arquivo ficará semelhante a este.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image3.png)

5.  Agora, você deve atualizar os agentes um a um. Selecione
    **compliance_agent.py** no menu do explorer, dentro da pasta
    **agents**, e substitua o conteúdo pelo trecho de código fornecido.

> import os
>
> import asyncio
>
> from azure.ai.projects.aio import AIProjectClient
>
> from agent_framework import ChatAgent
>
> from agent_framework.azure import AzureAIAgentClient
>
> from azure.identity.aio import AzureCliCredential
>
> async def build_compliance_agent():
>
> """Build or reuse persistent compliance agent"""
>
> credential = AzureCliCredential()
>
> async with AIProjectClient(
>
> endpoint=os.getenv("AZURE_AI_PROJECT_ENDPOINT"),
>
> credential=credential
>
> ) as project_client:
>
> \# Try to find existing agent first
>
> agent_name = "Enterprise-ComplianceAgent"
>
> try:
>
> agents = project_client.agents.list_agents()
>
> async for agent in agents:
>
> if agent.name == agent_name:
>
> \# Return existing persistent agent
>
> return ChatAgent(
>
> chat_client=AzureAIAgentClient(
>
> async_credential=credential,
>
> agent_id=agent.id,
>
> project_endpoint=os.getenv("AZURE_AI_PROJECT_ENDPOINT"),
>
> model_deployment_name=os.getenv("AZURE_AI_MODEL_DEPLOYMENT_NAME")
>
> ),
>
> instructions="You are a senior compliance and legal specialist."
>
> )
>
> except Exception:
>
> pass \# Continue to create new agent if listing fails
>
> \# Create new persistent agent if not found
>
> created_agent = await project_client.agents.create_agent(
>
> model=os.getenv("AZURE_AI_MODEL_DEPLOYMENT_NAME"),
>
> name=agent_name,
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
> "When provided CONTEXT, prefer it as the primary source. "
>
> "If the user asks to create a ticket (phrases like \\create a
> ticket\\, \\submit a compliance request\\, \\open a support ticket\\),
> output a structured block starting with:\n"
>
> "CREATE_TICKET\n"
>
> "Subject: \<one-line subject\>\n"
>
> "Body: \<detailed description\>\n"
>
> "Tags: tag1,tag2 (optional)\n"
>
> "Email: user@example.com (optional)\n"
>
> "Name: John Doe (optional)\n"
>
> "Return only the CREATE_TICKET block when requesting a ticket; do not
> call any APIs yourself.\n\n"
>
> "Always provide factual, well-researched answers with relevant legal
> citations. "
>
> "Include practical implementation steps and potential risks. Use
> formal, professional tone."
>
> )
>
> )
>
> \# Return persistent agent wrapped in ChatAgent
>
> return ChatAgent(
>
> chat_client=AzureAIAgentClient(
>
> async_credential=credential,
>
> agent_id=created_agent.id,
>
> project_endpoint=os.getenv("AZURE_AI_PROJECT_ENDPOINT"),
>
> model_deployment_name=os.getenv("AZURE_AI_MODEL_DEPLOYMENT_NAME")
>
> ),
>
> instructions="You are a senior compliance and legal specialist."
>
> )

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image4.png)

> **Integração com o Azure AI Project Client:**

- O AIProjectClient se conecta diretamente ao endpoint do seu projeto no
  Microsoft Foundry, permitindo que o script liste, recupere ou crie
  agentes que são hospedados de forma persistente dentro do Foundry.

> **Lógica de reutilização do agente:**

- Antes de criar um novo agente, o código primeiro verifica se já existe
  um agente chamado "Enterprise-ComplianceAgent".

- Se encontrado, esse agente existente é reutilizado por meio do vínculo
  com seu agent_id exclusivo, gerenciado pelo Foundry.

> **Criação de Agente Persistente:**

- Se o agente não existir, ele será criado por meio de
  project_client.agents.create_agent().

- O agente é registrado no Foundry com seu modelo, nome e conjunto
  detalhado de instruções, tornando-o permanentemente acessível entre
  sessões.

> **Integração com ChatAgent:**

- Após ser criado ou recuperado, o agente persistente do Foundry é
  integrado a uma instância de ChatAgent usando AzureAIAgentClient.

- Isso permite a comunicação programática com o agente hospedado,
  mantendo seu estado, políticas e recursos de monitoramento dentro do
  Microsoft Foundry.

6.  Após concluir, selecione **File** (1) e, em seguida, clique em
    **Save** (2) para salvar o arquivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image5.png)

7.  Selecione o arquivo **finance_agent.py** e substitua o conteúdo pelo
    trecho de código fornecido abaixo para configurar o agente
    financeiro persistente.

> import os
>
> import asyncio
>
> from azure.ai.projects.aio import AIProjectClient
>
> from agent_framework import ChatAgent
>
> from agent_framework.azure import AzureAIAgentClient
>
> from azure.identity.aio import AzureCliCredential
>
> async def build_finance_agent():
>
> """Build or reuse persistent finance agent"""
>
> credential = AzureCliCredential()
>
> async with AIProjectClient(
>
> endpoint=os.getenv("AZURE_AI_PROJECT_ENDPOINT"),
>
> credential=credential
>
> ) as project_client:
>
> \# Try to find existing agent first
>
> agent_name = "Enterprise-FinanceAgent"
>
> try:
>
> agents = project_client.agents.list_agents()
>
> async for agent in agents:
>
> if agent.name == agent_name:
>
> \# Return existing persistent agent
>
> return ChatAgent(
>
> chat_client=AzureAIAgentClient(
>
> async_credential=credential,
>
> agent_id=agent.id,
>
> project_endpoint=os.getenv("AZURE_AI_PROJECT_ENDPOINT"),
>
> model_deployment_name=os.getenv("AZURE_AI_MODEL_DEPLOYMENT_NAME")
>
> ),
>
> instructions="You are a finance and reimbursement specialist."
>
> )
>
> except Exception:
>
> pass \# Continue to create new agent if listing fails
>
> \# Create new persistent agent if not found
>
> created_agent = await project_client.agents.create_agent(
>
> model=os.getenv("AZURE_AI_MODEL_DEPLOYMENT_NAME"),
>
> name=agent_name,
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
> "specific amounts, policies, and actionable guidance.\n\n"
>
> "When provided CONTEXT, prefer it as the primary source. "
>
> "If the user asks to create a ticket (phrases like \\create a
> ticket\\, \\submit a reimbursement request\\, \\open a support
> ticket\\), output a structured block starting with:\n"
>
> "CREATE_TICKET\n"
>
> "Subject: \<one-line subject\>\n"
>
> "Body: \<detailed description\>\n"
>
> "Tags: tag1,tag2 (optional)\n"
>
> "Email: user@example.com (optional)\n"
>
> "Name: John Doe (optional)\n"
>
> "Return only the CREATE_TICKET block when requesting a ticket; do not
> call any APIs yourself."
>
> )
>
> )
>
> \# Return persistent agent wrapped in ChatAgent
>
> return ChatAgent(
>
> chat_client=AzureAIAgentClient(
>
> async_credential=credential,
>
> agent_id=created_agent.id,
>
> project_endpoint=os.getenv("AZURE_AI_PROJECT_ENDPOINT"),
>
> model_deployment_name=os.getenv("AZURE_AI_MODEL_DEPLOYMENT_NAME")
>
> ),
>
> instructions="You are a finance and reimbursement specialist."
>
> )

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image6.png)

> **Gerenciamento de Agentes Persistentes via Microsoft Foundry:**

- O AIProjectClient se conecta ao seu projeto do Microsoft Foundry,
  permitindo que o script liste, localize ou crie agentes persistentes
  que residem no ambiente de execução do Foundry, em vez de serem
  executados localmente.

> **Reutilização de Agentes Existentes:**

- Antes de criar um novo agente, a função verifica se já existe um
  agente chamado "Enterprise-FinanceAgent".

- Se encontrado, esse agente implementado é reutilizado ao inicializar
  um ChatAgent por meio do seu ID gerenciado pelo Foundry, evitando
  implementações duplicadas.

> **Criação Automática de Agente (se não existir):**

- Se o agente não for encontrado, ele cria um novo agente persistente no
  Foundry usando project_client.agents.create_agent(),

- registrando-o com um nome de implementação de modelo, nome de agente
  exclusivo e instruções específicas do domínio com foco em finanças e
  reembolsos.

> **Integração com AzureAIAgentClient para Comunicação:**

- O agente criado ou reutilizado é então integrado a um ChatAgent usando
  AzureAIAgentClient,

- que gerencia autenticação, roteamento de modelo e comunicação
  persistente com o agente implementado no Foundry.

8.  Após concluir, selecione **File (1)** e, em seguida, clique em
    **Save (2)** para salvar o arquivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image5.png)

9.  Agora, selecione o arquivo **hr_agent.py** e substitua o código pelo
    conteúdo a seguir, que converte um agente de chat sem estado em um
    agente persistente.

> import os
>
> import asyncio
>
> from azure.ai.projects.aio import AIProjectClient
>
> from agent_framework import ChatAgent
>
> from agent_framework.azure import AzureAIAgentClient
>
> from azure.identity.aio import AzureCliCredential
>
> async def build_hr_agent():
>
> """Build or reuse persistent HR agent"""
>
> credential = AzureCliCredential()
>
> async with AIProjectClient(
>
> endpoint=os.getenv("AZURE_AI_PROJECT_ENDPOINT"),
>
> credential=credential
>
> ) as project_client:
>
> \# Try to find existing agent first
>
> agent_name = "Enterprise-HRAgent"
>
> try:
>
> agents = project_client.agents.list_agents()
>
> async for agent in agents:
>
> if agent.name == agent_name:
>
> \# Return existing persistent agent
>
> return ChatAgent(
>
> chat_client=AzureAIAgentClient(
>
> async_credential=credential,
>
> agent_id=agent.id,
>
> project_endpoint=os.getenv("AZURE_AI_PROJECT_ENDPOINT"),
>
> model_deployment_name=os.getenv("AZURE_AI_MODEL_DEPLOYMENT_NAME")
>
> ),
>
> instructions="You are an expert HR policy specialist."
>
> )
>
> except Exception:
>
> pass \# Continue to create new agent if listing fails
>
> \# Create new persistent agent if not found
>
> created_agent = await project_client.agents.create_agent(
>
> model=os.getenv("AZURE_AI_MODEL_DEPLOYMENT_NAME"),
>
> name=agent_name,
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
> "When provided CONTEXT, prefer it as the primary source. "
>
> "If the user asks to create a ticket (phrases like \\create a
> ticket\\, \\submit a leave request\\, \\open a support ticket\\),
> output a structured block starting with:\n"
>
> "CREATE_TICKET\n"
>
> "Subject: \<one-line subject\>\n"
>
> "Body: \<detailed description\>\n"
>
> "Tags: tag1,tag2 (optional)\n"
>
> "Email: user@example.com (optional)\n"
>
> "Name: John Doe (optional)\n"
>
> "Return only the CREATE_TICKET block when requesting a ticket; do not
> call any APIs yourself.\n\n"
>
> "Provide specific, actionable guidance with policy references where
> applicable. "
>
> "Be empathetic and professional in your responses."
>
> )
>
> )
>
> \# Return persistent agent wrapped in ChatAgent
>
> return ChatAgent(
>
> chat_client=AzureAIAgentClient(
>
> async_credential=credential,
>
> agent_id=created_agent.id,
>
> project_endpoint=os.getenv("AZURE_AI_PROJECT_ENDPOINT"),
>
> model_deployment_name=os.getenv("AZURE_AI_MODEL_DEPLOYMENT_NAME")
>
> ),
>
> instructions="You are an expert HR policy specialist."
>
> )

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image7.png)

> Esta atualização converte o Agente de RH em um agente persistente
> hospedado na nuvem dentro do Microsoft Foundry. Ele se conecta ao
> projeto Foundry usando o AIProjectClient, reutiliza o
> “Enterprise-HRAgent” existente, se implementado, ou cria um novo com
> instruções especializadas do domínio de RH. Uma vez implementado, ele
> é integrado a um ChatAgent vinculado via AzureAIAgentClient,
> permitindo a automação de RH estável, reutilizável e gerenciada
> centralmente dentro do ambiente Foundry.

10. Após concluir, selecione **File (1)** e, em seguida, clique em
    **Save (2)** para salvar o arquivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image5.png)

11. Selecione o arquivo **planner_agent.py** e substitua o conteúdo pelo
    trecho de código fornecido abaixo para configurar o orquestrador
    persistente.

> import os
>
> import asyncio
>
> from azure.ai.projects.aio import AIProjectClient
>
> from agent_framework import ChatAgent
>
> from agent_framework.azure import AzureAIAgentClient
>
> from azure.identity.aio import AzureCliCredential
>
> async def build_planner_agent():
>
> """Build or reuse persistent planner agent"""
>
> credential = AzureCliCredential()
>
> async with AIProjectClient(
>
> endpoint=os.getenv("AZURE_AI_PROJECT_ENDPOINT"),
>
> credential=credential
>
> ) as project_client:
>
> \# Try to find existing agent first
>
> agent_name = "Enterprise-PlannerAgent"
>
> try:
>
> agents = project_client.agents.list_agents()
>
> async for agent in agents:
>
> if agent.name == agent_name:
>
> \# Return existing persistent agent
>
> return ChatAgent(
>
> chat_client=AzureAIAgentClient(
>
> async_credential=credential,
>
> agent_id=agent.id,
>
> project_endpoint=os.getenv("AZURE_AI_PROJECT_ENDPOINT"),
>
> model_deployment_name=os.getenv("AZURE_AI_MODEL_DEPLOYMENT_NAME")
>
> ),
>
> instructions="You are an intelligent routing agent."
>
> )
>
> except Exception:
>
> pass \# Continue to create new agent if listing fails
>
> \# Create new persistent agent if not found
>
> created_agent = await project_client.agents.create_agent(
>
> model=os.getenv("AZURE_AI_MODEL_DEPLOYMENT_NAME"),
>
> name=agent_name,
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
> )
>
> )
>
> \# Return persistent agent wrapped in ChatAgent
>
> return ChatAgent(
>
> chat_client=AzureAIAgentClient(
>
> async_credential=credential,
>
> agent_id=created_agent.id,
>
> project_endpoint=os.getenv("AZURE_AI_PROJECT_ENDPOINT"),
>
> model_deployment_name=os.getenv("AZURE_AI_MODEL_DEPLOYMENT_NAME")
>
> ),
>
> instructions="You are an intelligent routing agent."
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
>
> ![A screen shot of a computer program AI-generated content may be
> incorrect.](./media/image8.png)

Ele se conecta por meio do AIProjectClient, reutiliza o
“Enterprise-PlannerAgent” existente, se já estiver implementado, ou cria
um novo com lógica de roteamento que classifica as consultas nas
categorias RH, Finanças ou Conformidade.

12. Após concluir, selecione **File (1)** e, em seguida, clique em
    **Save (2)** para salvar o arquivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image5.png)

13. Agora, selecione o arquivo **main.py** a partir do diretório raiz e
    substitua o código pelo trecho fornecido. Para todos os arquivos de
    código, substitua o conteúdo com atenção, pois o Python é sensível à
    indentação.

> import asyncio
>
> import time
>
> import logging
>
> import re
>
> from typing import Dict, Any
>
> from utils.env import load_env
>
> from azure.identity.aio import AzureCliCredential
>
> from agents.planner_agent import build_planner_agent, classify_target
>
> from agents.hr_agent import build_hr_agent
>
> from agents.compliance_agent import build_compliance_agent
>
> from agents.finance_agent import build_finance_agent
>
> from tools.azure_search_tool import AzureSearchTool
>
> from tools.freshdesk_tool import FreshdeskTool
>
> \# Configure logging
>
> logging.basicConfig(level=logging.INFO, format='%(asctime)s -
> %(levelname)s - %(message)s')
>
> def parse_create_ticket_block(response_text: str) -\> Dict\[str,
> Any\]:
>
> """
>
> Parse CREATE_TICKET block from agent response.
>
> """
>
> if "CREATE_TICKET" not in response_text:
>
> return None
>
> \# Extract the CREATE_TICKET block
>
> lines = response_text.split('\n')
>
> ticket_start = -1
>
> for i, line in enumerate(lines):
>
> if line.strip() == "CREATE_TICKET":
>
> ticket_start = i
>
> break
>
> if ticket_start == -1:
>
> return None
>
> \# Parse ticket details
>
> ticket_data = {
>
> "subject": "",
>
> "body": "",
>
> "tags": \[\],
>
> "email": "system@enterprise.com",
>
> "name": "Enterprise System User"
>
> }
>
> \# Process lines after CREATE_TICKET
>
> for line in lines\[ticket_start + 1:\]:
>
> line = line.strip()
>
> if not line:
>
> continue
>
> if line.startswith("Subject:"):
>
> ticket_data\["subject"\] = line\[8:\].strip()
>
> elif line.startswith("Body:"):
>
> ticket_data\["body"\] = line\[5:\].strip()
>
> elif line.startswith("Tags:"):
>
> tags_str = line\[5:\].strip()
>
> if tags_str:
>
> ticket_data\["tags"\] = \[tag.strip() for tag in tags_str.split(',')\]
>
> elif line.startswith("Email:"):
>
> ticket_data\["email"\] = line\[6:\].strip()
>
> elif line.startswith("Name:"):
>
> ticket_data\["name"\] = line\[5:\].strip()
>
> return ticket_data
>
> async def run_multi_agent_with_user_info(query: str, agents:
> Dict\[str, Any\], user_name: str = None) -\> Dict\[str, Any\]:
>
> """
>
> Enhanced multi-agent system with CREATE_TICKET pattern support and
> user name handling.
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
> \# Step 2: Retrieve relevant context using Azure Search
>
> logging.info("Retrieving context from knowledge base...")
>
> context = await agents\["search_tool"\].search(query, top=3)
>
> \# Step 3: Create enriched prompt with context
>
> enriched_prompt = f"""
>
> Context from Knowledge Base:
>
> {context}
>
> ---
>
> User Question: {query}
>
> Please provide a comprehensive answer based on the context above. If
> no relevant context is found, provide your best guidance based on your
> training.
>
> """
>
> \# Step 4: Get agent response
>
> agent_key = target.lower()
>
> agent_name = f"{target}Agent"
>
> if agent_key in agents:
>
> logging.info(f"Processing with {agent_name}...")
>
> answer = await agents\[agent_key\].run(enriched_prompt)
>
> else:
>
> \# Fallback to HR if routing unclear
>
> logging.warning(f"Unknown target '{target}', falling back to HR")
>
> answer = await agents\["hr"\].run(enriched_prompt)
>
> target = "HR"
>
> agent_name = "HRAgent"
>
> answer_text = str(answer)
>
> \# Step 5: Check for CREATE_TICKET pattern in response
>
> ticket_info = None
>
> ticket_created = False
>
> ticket_data = parse_create_ticket_block(answer_text)
>
> if ticket_data and "freshdesk_tool" in agents:
>
> logging.info("CREATE_TICKET pattern detected - creating Freshdesk
> ticket")
>
> \# Use provided user name if available
>
> if user_name:
>
> ticket_data\["name"\] = user_name
>
> logging.info(f"Using provided user name: {user_name}")
>
> try:
>
> \# Create ticket using parsed data
>
> ticket_result = await agents\["freshdesk_tool"\].create_ticket(
>
> subject=ticket_data\["subject"\] or f"{target} Request:
> {query\[:60\]}...",
>
> description=ticket_data\["body"\] or f"Request: {query}\n\nAgent
> Response:\n{answer_text}",
>
> tags=ticket_data\["tags"\] or \[target.lower(), "agent-system"\],
>
> requester={
>
> "name": ticket_data\["name"\],
>
> "email": ticket_data\["email"\]
>
> }
>
> )
>
> if ticket_result.get("success"):
>
> ticket_info = ticket_result
>
> ticket_created = True
>
> ticket_id = ticket_result.get("ticket", {}).get("id")
>
> ticket_url = ticket_result.get("ticket", {}).get("url")
>
> \# Replace CREATE_TICKET block with success message
>
> if "CREATE_TICKET" in answer_text:
>
> \# Remove the CREATE_TICKET block and replace with success message
>
> lines = answer_text.split('\n')
>
> filtered_lines = \[\]
>
> skip_ticket_block = False
>
> for line in lines:
>
> if line.strip() == "CREATE_TICKET":
>
> skip_ticket_block = True
>
> \# Add success message with user name
>
> success_msg = f"""
>
> 🎫 \*\*Support Ticket Created Successfully\*\*
>
> - Ticket ID: \#{ticket_id}
>
> - Subject: {ticket_data\["subject"\]}
>
> - Requester: {ticket_data\["name"\]}
>
> - Status: Open
>
> - URL: {ticket_url}
>
> Your request has been submitted to our {target} team. You will receive
> updates via email.
>
> """
>
> filtered_lines.append(success_msg)
>
> continue
>
> elif skip_ticket_block and (line.startswith("Subject:") or
> line.startswith("Body:") or
>
> line.startswith("Tags:") or line.startswith("Email:") or
>
> line.startswith("Name:")):
>
> continue
>
> else:
>
> skip_ticket_block = False
>
> filtered_lines.append(line)
>
> answer_text = '\n'.join(filtered_lines)
>
> else:
>
> answer_text += f"\n\n⚠️ \*\*Note\*\*: Could not create support ticket:
> {ticket_result.get('error', 'Unknown error')}"
>
> except Exception as e:
>
> logging.error(f"Failed to create Freshdesk ticket: {e}")
>
> answer_text += f"\n\n⚠️ \*\*Note\*\*: Ticket creation failed:
> {str(e)}"
>
> \# Step 6: Process response
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
> "answer": answer_text,
>
> "context_retrieved": len(context) \> 100, \# Simple check if context
> was found
>
> "ticket_created": ticket_created,
>
> "ticket_info": ticket_info,
>
> "response_time": round(response_time, 2),
>
> "timestamp": time.strftime("%Y-%m-%d %H:%M:%S"),
>
> "success": True,
>
> "user_name": user_name
>
> }
>
> except Exception as e:
>
> logging.error(f"Multi-agent processing error: {e}")
>
> return {
>
> "query": query,
>
> "routed_to": "Error",
>
> "agent_name": "ErrorHandler",
>
> "answer": f"I encountered an error processing your request: {str(e)}.
> Please try again.",
>
> "context_retrieved": False,
>
> "ticket_created": False,
>
> "ticket_info": None,
>
> "response_time": 0,
>
> "timestamp": time.strftime("%Y-%m-%d %H:%M:%S"),
>
> "success": False,
>
> "user_name": user_name
>
> }
>
> async def run_multi_agent(query: str, agents: Dict\[str, Any\]) -\>
> Dict\[str, Any\]:
>
> """
>
> Wrapper for multi-agent system with no user name.
>
> """
>
> return await run_multi_agent_with_user_info(query, agents, None)
>
> def format_response(result: Dict\[str, Any\]) -\> str:
>
> """Format the agent response for display."""
>
> status_icon = "✅" if result\["success"\] else "❌"
>
> context_icon = "📚" if result.get("context_retrieved") else "📭"
>
> ticket_icon = "🎫" if result.get("ticket_created") else ""
>
> formatted = f"""
>
> {status_icon} Agent Response Summary:
>
> ┌─ Routed to: {result\['routed_to'\]} ({result\['agent_name'\]})
>
> ├─ Response time: {result\['response_time'\]}s
>
> ├─ Context retrieved: {context_icon} {'Yes' if
> result.get('context_retrieved') else 'No'}
>
> ├─ Ticket created: {ticket_icon} {'Yes' if
> result.get('ticket_created') else 'No'}
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
> \# Add ticket details if available
>
> if result.get("ticket_info") and
> result\["ticket_info"\].get("success"):
>
> ticket = result\["ticket_info"\]\["ticket"\]
>
> formatted += f"""
>
> 🎫 Ticket Details:
>
> ├─ ID: \#{ticket\['id'\]}
>
> ├─ Status: {ticket\['status'\]}
>
> ├─ Priority: {ticket\['priority'\]}
>
> └─ URL: {ticket\['url'\]}
>
> """
>
> return formatted
>
> async def interactive_ticket_creation(agents: Dict\[str, Any\],
> base_query: str) -\> Dict\[str, Any\]:
>
> """
>
> Simple interactive ticket creation.
>
> """
>
> print("\n🎫 \*\*Manual Ticket Creation\*\*")
>
> print("I'll help you create a support ticket manually.\n")
>
> try:
>
> \# Get basic ticket details
>
> subject = input(f"📝 Ticket Subject: ").strip() or f"Manual Request:
> {base_query\[:60\]}..."
>
> print("\n📄 \*\*Ticket Description\*\* (press Enter twice when
> done):")
>
> description_lines = \[f"Original Request: {base_query}", ""\]
>
> while True:
>
> line = input(" ").strip()
>
> if not line:
>
> break
>
> description_lines.append(line)
>
> description = "\n".join(description_lines)
>
> \# Create the ticket directly
>
> print(f"\n🚀 Creating ticket: '{subject}'...")
>
> ticket_result = await agents\["freshdesk_tool"\].create_ticket(
>
> subject=subject,
>
> description=description,
>
> tags=\["manual", "interactive"\],
>
> requester={
>
> "name": "Enterprise System User",
>
> "email": "system@enterprise.com"
>
> }
>
> )
>
> if ticket_result.get("success"):
>
> ticket_info = ticket_result.get("ticket", {})
>
> print(f"""
>
> ✅ \*\*Ticket Created Successfully!\*\*
>
> 🎫 Ticket Details:
>
> • ID: \#{ticket_info.get('id')}
>
> • Subject: {subject}
>
> • Status: Open
>
> • URL: {ticket_info.get('url')}
>
> 📧 You will receive email updates about your ticket status.
>
> """)
>
> return {
>
> "success": True,
>
> "ticket_created": True,
>
> "ticket_info": ticket_result
>
> }
>
> else:
>
> print(f"❌ \*\*Failed to create ticket\*\*:
> {ticket_result.get('error', 'Unknown error')}")
>
> return {"success": False, "ticket_created": False}
>
> except KeyboardInterrupt:
>
> print("\n🚫 Ticket creation cancelled.")
>
> return {"success": False, "ticket_created": False}
>
> except Exception as e:
>
> print(f"❌ \*\*Error during ticket creation\*\*: {str(e)}")
>
> return {"success": False, "ticket_created": False}
>
> async def run_interactive_mode(agents: Dict\[str, Any\]):
>
> """Interactive mode for real-time queries with enhanced ticket
> creation."""
>
> print("\n🤖 Enterprise Agent System - Interactive Mode")
>
> print("Available agents: HR, Finance, Compliance")
>
> print("Type 'quit' to exit, 'help' for commands, 'ticket' for
> interactive ticket creation\n")
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
> - Ask any question about HR, Finance, or Compliance
>
> - 'ticket' - Interactive ticket creation mode
>
> - 'quit' or 'exit' - Exit the system
>
> - 'help' - Show this help message
>
> 🎯 Example questions:
>
> - "What's the travel reimbursement limit for meals?"
>
> - "I need to create a ticket for sick leave"
>
> - "Can you help me submit a reimbursement request?"
>
> - "How many vacation days do employees get?"
>
> - "Do we need GDPR compliance for EU customers?"
>
> 🎫 Ticket Creation:
>
> - Use 'ticket' command for guided ticket creation
>
> - Or include phrases like "create ticket", "submit request" in your
> question
>
> - For LEAVE and REIMBURSEMENT requests, you'll be prompted for your
> name
>
> """)
>
> continue
>
> elif query.lower() == 'ticket':
>
> if "freshdesk_tool" not in agents:
>
> print("❌ Ticket creation is not available (Freshdesk tool not
> configured)")
>
> continue
>
> base_query = input("📝 Describe what you need help with: ").strip()
>
> if base_query:
>
> await interactive_ticket_creation(agents, base_query)
>
> continue
>
> elif not query:
>
> continue
>
> \# Check if this is a leave or reimbursement request that needs user
> name
>
> query_lower = query.lower()
>
> is_leave_request = any(word in query_lower for word in \["leave",
> "vacation", "sick", "time off", "pto", "holiday"\])
>
> is_reimbursement_request = any(word in query_lower for word in
> \["reimburse", "expense", "travel", "receipt", "reimbursement"\])
>
> wants_ticket = any(keyword in query_lower for keyword in \["create
> ticket", "submit ticket", "file ticket", "raise ticket",
>
> "create request", "submit request", "file request", "need help with",
>
> "open ticket", "new ticket", "support ticket", "help ticket"\])
>
> user_name = None
>
> if (is_leave_request or is_reimbursement_request) and wants_ticket:
>
> print("\n👤 For leave and reimbursement requests, I need to collect
> some information:")
>
> user_name = input("Please enter your name: ").strip()
>
> if not user_name:
>
> print("❌ Name is required for this type of request. Please try
> again.")
>
> continue
>
> print(f"✅ Thank you, {user_name}! Processing your request...")
>
> print("\n🤔 Processing your query...")
>
> result = await run_multi_agent_with_user_info(query, agents,
> user_name)
>
> print(format_response(result))
>
> print() \# Add spacing between queries
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
> print()
>
> async def run_batch_tests(agents: Dict\[str, Any\]):
>
> """Run focused test with ticket creation."""
>
> test_queries = \[
>
> "What is the company's policy on remote work and flexible hours?"
>
> \]
>
> print("🧪 Running focused batch tests with grounded data
> integration...\n")
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
> await asyncio.sleep(1.0) \# Longer delay for tool operations
>
> async def main():
>
> """Main application entry point with enhanced features and tool
> integration."""
>
> print("🚀 Initializing Enterprise Agent System with Persistent Azure
> AI Foundry Agents...")
>
> try:
>
> \# Load environment and build persistent agents
>
> load_env()
>
> logging.info("Building persistent Azure AI Foundry agent network...")
>
> \# Build core persistent agents using Azure AI Foundry
>
> planner_agent_client = await build_planner_agent()
>
> hr_agent_client = await build_hr_agent()
>
> compliance_agent_client = await build_compliance_agent()
>
> finance_agent_client = await build_finance_agent()
>
> async with (
>
> planner_agent_client as planner_agent,
>
> hr_agent_client as hr_agent,
>
> compliance_agent_client as compliance_agent,
>
> finance_agent_client as finance_agent
>
> ):
>
> agents = {
>
> "planner": planner_agent,
>
> "hr": hr_agent,
>
> "compliance": compliance_agent,
>
> "finance": finance_agent
>
> }
>
> \# Initialize and attach tools
>
> logging.info("Initializing tools...")
>
> try:
>
> search_tool = AzureSearchTool()
>
> agents\["search_tool"\] = search_tool
>
> \# Test search tool
>
> health = await search_tool.health_check()
>
> if health\["status"\] == "healthy":
>
> logging.info("✅ Azure Search tool initialized successfully")
>
> else:
>
> logging.warning(f"⚠️ Azure Search tool health check failed: {health}")
>
> except Exception as e:
>
> logging.error(f"Failed to initialize Azure Search tool: {e}")
>
> \# Create mock search tool for testing
>
> class MockSearchTool:
>
> async def search(self, query, top=3):
>
> return f"📭 Mock search results for: {query}\n(Azure Search tool not
> configured)"
>
> agents\["search_tool"\] = MockSearchTool()
>
> \# Initialize Freshdesk tool for ticket creation
>
> try:
>
> freshdesk_tool = FreshdeskTool()
>
> agents\["freshdesk_tool"\] = freshdesk_tool
>
> logging.info("✅ Freshdesk tool initialized successfully")
>
> except Exception as e:
>
> logging.warning(f"⚠️ Freshdesk tool initialization failed: {e}")
>
> \# System will work without Freshdesk, just won't create tickets
>
> logging.info("✅ All Azure AI Foundry agents and tools initialized")
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
> logging.error(f"Azure AI Foundry agent system initialization failed:
> {e}")
>
> print(f"❌ Failed to start Azure AI Foundry system: {e}")
>
> \# Try to run with minimal configuration
>
> logging.info("Attempting to run with minimal Azure AI Foundry
> configuration...")
>
> try:
>
> planner_agent_client = await build_planner_agent()
>
> hr_agent_client = await build_hr_agent()
>
> compliance_agent_client = await build_compliance_agent()
>
> finance_agent_client = await build_finance_agent()
>
> async with (
>
> planner_agent_client as planner_agent,
>
> hr_agent_client as hr_agent,
>
> compliance_agent_client as compliance_agent,
>
> finance_agent_client as finance_agent
>
> ):
>
> minimal_agents = {
>
> "planner": planner_agent,
>
> "hr": hr_agent,
>
> "compliance": compliance_agent,
>
> "finance": finance_agent,
>
> "search_tool": type('MockSearch', (), {'search': lambda self, q,
> top=3: f"Mock search for: {q}"})()
>
> }
>
> await run_batch_tests(minimal_agents)
>
> except Exception as minimal_error:
>
> print(f"❌ Even minimal Azure AI Foundry configuration failed:
> {minimal_error}")
>
> if \_\_name\_\_ == "\_\_main\_\_":
>
> asyncio.run(main())

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image9.png)

14. Após concluir, selecione **File (1)** e, em seguida, clique em
    **Save (2)** para salvar o arquivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image5.png)

15. Os agentes foram atualizados com sucesso com as configurações de
    persistência. Agora, execute o agente para verificar a criação dos
    agentes no Portal do Microsoft Foundry.

16. Selecione a opção **...** (1) no menu superior para expandir o menu.
    Em seguida, selecione **Terminal** (2) e clique em **New Terminal**
    (3).

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.png)

17. No **Terminal do VS Code**, execute o comando de login da Azure CLI:

+++az login+++

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image11.png)

18. Na janela **Sign in**, selecione **Work or school account** e clique
    em **Continue**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.png)

19. Na guia **Sign into Microsoft**, faça login usando as credenciais
    abaixo.

- Username - <+++@lab.CloudPortalCredential(User1).Username>+++

- TAP - +++@lab.CloudPortalCredential(User1).TAP+++

20. Quando for exibida a tela com as opções de entrada, selecione **No,
    this app only** para continuar sem vincular outros aplicativos de
    desktop.

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image13.png)

21. Quando for solicitado que você selecione uma subscription e um
    tenant, digite **1** e pressione **Enter** para continuar.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

22. Execute o comando fornecido abaixo para testar o funcionamento da
    ferramenta de pesquisa.

+++python main.py+++

![](./media/image15.png)

23. Abra o Portal do Azure, que foi aberto anteriormente, navegue até o
    seu grupo de recursos e, na lista de recursos, selecione o recurso
    **agente-AI** foundry.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image16.png)

24. No painel seguinte, clique em **Go to Foundry portal**. Você será
    redirecionado para o Microsoft Foundry Portal.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.png)

25. Após acessar o Microsoft Foundry Portal, selecione **Agents** (1) no
    menu à esquerda. Você verá que todos os agentes foram registrados
    com sucesso no portal do Microsoft Foundry.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.png)

> Como parte do uso do Microsoft Agent Framework, os agentes são
> projetados para operar tanto em ambientes locais quanto em ambientes
> hospedados na nuvem, sendo gerenciados programaticamente por meio do
> SDK, em vez da interface do portal do Microsoft Foundry. Após serem
> implementados, esses agentes permanecem no ambiente de execução
> gerenciado pelo Foundry e continuam a operar como serviços.
>
> A partir dos próximos exercícios, você continuará trabalhando
> localmente para configurar recursos de observabilidade, monitoramento
> e rastreamento, permitindo visualizar, analisar e governar o
> comportamento desses agentes enquanto operam na nuvem.

**Resumo**

Neste laboratório, você implementou com sucesso seu sistema multiagente
desenvolvido localmente no Microsoft Foundry Agent Service.

Você concluiu este laboratório com êxito. Clique em Next \>\> para
prosseguir.
