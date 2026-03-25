# Laboratorio 8: Implementación de agentes y gestión de la ejecución en Microsoft Foundry

**Duración estimada:** 15 minutos

**Descripción general**

En este laboratorio, implementará su sistema multiagente (desarrollado
utilizando **Microsoft Agent Framework SDK**) en el **Microsoft Foundry
Agent Service**. Publicará el agente configurado en un entorno de
ejecución gestionado.

Hasta ahora, ha creado un **Chat Response Agent**, lo que significa:

- Maneja interacciones de un solo turno (single-turn) y sin estado
  (stateless), respondiendo inmediatamente a las entradas del usuario.

- Se ejecuta localmente dentro de su aplicación o SDK, sin necesidad de
  un backend persistente.

- Cada solicitud se procesa de forma independiente, sin mantener memoria
  ni contexto a largo plazo.

- Es ideal para experiencias de chat rápidas o para probar la lógica
  central antes de una implementación completa.

Ahora, actualizará este agente para convertirlo en un **Persistent
Agent** en Microsoft Foundry, lo que significa:

- Se ejecuta como un servicio gestionado y de larga duración dentro del
  entorno de Foundry.

- Puede mantener estado y contexto a través de sesiones para continuidad
  y aprendizaje.

- Soporta integración con herramientas externas y otros agentes mediante
  MCP y protocolos A2A.

- Está optimizado para fiabilidad a nivel empresarial, monitoreo y
  cumplimiento normativo.

**Objetivos del laboratorio**

En este laboratorio, realizará la siguiente tarea:

- **Tarea 1:** Implementar agentes en Microsoft Foundry Agent Service

## Tarea 1: Implementar agentes en Microsoft Foundry Agent Service

En esta tarea, actualizará el agente existente para convertirlo en
**persistent agent** y publicará cada agente en Microsoft Foundry Agent
Service como un modelo independiente.

1.  Actualice los archivos de código para soportar el sistema de agentes
    persistentes, registrando el agente en el **Microsoft Foundry Agent
    Service**.

2.  En el panel de **Visual Studio Code**, desde el menú izquierdo,
    seleccione el archivo .env para actualizar las claves del proyecto
    AI Foundry.

3.  Agregue las siguientes variables al archivo:

> \# Azure AI Project Configuration
>
> AZURE_AI_PROJECT_ENDPOINT=**\<Microsoft Foundry endpoint\>**
>
> AZURE_AI_MODEL_DEPLOYMENT_NAME=gpt-4o-mini
>
> Obtenga el endpoint del proyecto de **Microsoft Foundry** desde la
> página **Overview** y reemplace **\<Microsoft Foundry endpoint\>** con
> ese valor.
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image1.png)

![](./media/image2.png)

4.  Una vez actualizado, el archivo se verá similar a este.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image3.png)

5.  Ahora, debe actualizar los agentes uno por uno. Seleccione
    **compliance_agent.py** desde el menú del explorador bajo la carpeta
    **agents**. Reemplace el contenido con el siguiente fragmento de
    código:

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

> **Integración con Azure AI Project Client:**

- **AIProjectClient** se conecta directamente al endpoint del proyecto
  en Microsoft Foundry, permitiendo al script **listar, recuperar o
  crear agentes** que se alojan persistentemente dentro de Foundry.

> **Lógica de reutilización de agentes:**

- Antes de crear un nuevo agente, el código verifica primero si existe
  un agente llamado **Enterprise-ComplianceAgent**.

- Si existe, reutiliza el agente existente usando su agent_id gestionado
  por Foundry.

> **Creación de agente persistente:**

- Si no existe, se crea mediante project_client.agents.create_agent().

- El agente se registra en Foundry con su modelo, nombre y conjunto de
  instrucciones detalladas, haciéndolo accesible de forma permanente.

> **Encapsulado con ChatAgent:**

- Una vez creado o recuperado, el agente persistente de Foundry se
  envuelve en una instancia de ChatAgent usando AzureAIAgentClient.

- Esto permite comunicación programática con el agente alojado,
  manteniendo su estado, políticas y funciones de monitoreo dentro de
  Microsoft Foundry.

6.  Una vez hecho, seleccione **File (1)** y luego haga clic en **Save
    (2)** para guardar el archivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image5.png)

7.  Seleccione el archivo **finance_agent.py** y reemplace su contenido
    con el siguiente snippet para configurar el agente financiero
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

> **Gestión de agentes persistentes en Microsoft Foundry:**

- **AIProjectClient** se conecta a su proyecto en **Microsoft Foundry**,
  permitiendo que el script **liste, busque o cree agentes
  persistentes** que viven dentro del entorno de Foundry, en lugar de
  ejecutarse localmente.

> **Reutilización de agentes existentes:**

- Antes de crear un nuevo agente, la función verifica si ya existe un
  "Enterprise-FinanceAgent".

- Si se encuentra, reutiliza ese agente desplegado inicializando un
  ChatAgent a través de su ID gestionado por Foundry, evitando
  despliegues duplicados.

> **Creación automática de agente (si no existe):**

- Si el agente no existe, se crea un nuevo agente persistente en Foundry
  mediante project_client.agents.create_agent().

- Se registra con un nombre de despliegue del modelo, un nombre único de
  agente e instrucciones específicas del dominio, centradas en finanzas
  y reembolsos.

> **Integración con AzureAIAgentClient para la comunicación**

- El agente creado o reutilizado se envuelve en un **ChatAgent**
  utilizando **AzureAIAgentClient**.

- Esto permite **gestionar autenticación, enrutamiento de modelo y
  comunicación persistente** con el agente desplegado en Foundry.

8.  Una vez hecho, seleccione **File (1)** y haga clic en **Save (2)**
    para guardar el archivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image5.png)

9.  Ahora, seleccione el archivo hr_agent.py y reemplace el código con
    el siguiente snippet, que convierte un **chat agent sin estado** en
    un **agente persistente**.

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

> Esta actualización convierte al **HR Agent** en un **agente
> persistente** alojado en la **nube** dentro de **Microsoft
> Foundry**.  
> Se conecta al proyecto de Foundry utilizando **AIProjectClient**,
> reutilizando el **“Enterprise-HRAgent”** existente si ya está
> implementado, o creando uno nuevo con **instrucciones especializadas
> del dominio de recursos humanos**.  
> Una vez implementado, se envuelve en un **ChatAgent** vinculado a
> través de **AzureAIAgentClient**, lo que permite una **automatización
> de HR con estado**, **reutilizable** y **gestionada centralmente**
> dentro del entorno de Foundry.

10. Una vez hecho, seleccione **File (1)** y haga clic en **Save (2)**
    para guardar el archivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image5.png)

11. Ahora, seleccione el archivo **planner_agent.py** y reemplace el
    contenido con el snippet de código proporcionado a continuación para
    configurar el orquestador persistente.

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

Se conecta mediante **AIProjectClient**, reutiliza el
"Enterprise-PlannerAgent" existente si ya está desplegado, o crea uno
nuevo con **lógica de enrutamiento** que clasifica las consultas en
categorías de **HR**, **Finance** o **Compliance**.

12. Después de completar este paso, seleccione **File (1)** y haga clic
    en **Save (2)** para guardar el archivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image5.png)

13. Ahora, seleccione **main.py** desde la raíz del proyecto y reemplace
    el código con el snippet proporcionado.  
    Para todos los archivos de código, asegúrese de reemplazar el
    contenido con cuidado, ya que Python es sensible a las
    indentaciones.

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

14. Después de completar este paso, seleccione **File (1)** y haga clic
    en **Save (2)** para guardar el archivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image5.png)

15. Los agentes se han actualizado correctamente con las configuraciones
    de persistencia. Ahora, ejecute el agente para verificar la creación
    de los agentes en **Microsoft Foundry Portal**.

16. Seleccione la opción **… (1)** en el menú superior para expandirlo.
    Seleccione **Terminal (2)** y haga clic en **New Terminal (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.png)

17. En la **Terminal de VS Code**, ejecute el comando de inicio de
    sesión de Azure CLI:

+++az login+++

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image11.png)

18. En la ventana **Sign in**, seleccione **Work or school account** y
    haga clic en **Continue**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.png)

19. En la pestaña **Sign into Microsoft**, inicie sesión utilizando las
    siguientes credenciales:

- Username - <+++@lab.CloudPortalCredential(User1).Username>+++

- TAP - +++@lab.CloudPortalCredential(User1).TAP+++

20. Cuando se le soliciten las opciones de inicio de sesión, seleccione
    **No, this app only** para continuar sin vincular otras aplicaciones
    de escritorio.

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image13.png)

21. Cuando aparezca el mensaje para seleccionar una suscripción y un
    tenant, escriba **1** y presione **Enter** para continuar.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

22. Ejecute el siguiente comando para comprobar el funcionamiento de la
    herramienta de búsqueda.

+++python main.py+++

![](./media/image15.png)

23. Abra Azure Portal, que ya fue abierto previamente; navegue a su
    grupo de recursos y, desde la lista de recursos, seleccione el
    recurso **agent-AI-foundry**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image16.png)

24. En el panel siguiente, haga clic en **Go to Foundry portal**. Será
    redirigido al **Microsoft Foundry Portal**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.png)

25. Una vez en el Microsoft Foundry Portal, seleccione **Agents (1)**
    desde el menú izquierdo. Verá que todos los agentes se registraron
    correctamente en el portal de Microsoft Foundry.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.png)

> Como parte del uso del Microsoft Agent Framework, los agentes están
> diseñados para operar en entornos locales o alojados en la nube,
> gestionados de manera programática a través del SDK, en lugar de
> hacerlo mediante la interfaz del Microsoft Foundry Portal.
>
> Una vez implementados, estos agentes persisten dentro del entorno
> gestionado por Foundry y continúan ejecutándose como servicios.
>
> A partir de los siguientes ejercicios, continuará trabajando de forma
> local para configurar observabilidad, monitoreo y trazabilidad, lo que
> le permitirá visualizar, analizar y gobernar el comportamiento de
> estos agentes mientras operan en la nube.

**Resumen**

En este laboratorio, implementó correctamente su sistema multiagente
creado localmente en el Microsoft Foundry Agent Service.

Ha completado exitosamente este laboratorio. Haga clic en **Next** \>\>
para continuar.
