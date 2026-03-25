# Laboratorio 6: Desarrollo de sistemas multiagente con comunicación Agent-to-Agent (A2A)

**Duración estimada:** 30 minutos

**Descripción general**

En este laboratorio, creará un sistema multiagente utilizando el
**Microsoft Agent Framework**. Definirá roles de agentes independientes
(Planner, HR y Compliance), los implementará y configurará
**comunicación A2A (Agent-to-Agent)** para permitir que un agente llame
a otros. Finalmente, probará un escenario donde una consulta de usuario
es delegada a través de la red de agentes y revisará trazas y registros
para confirmar el enrutamiento correcto.

El **Microsoft Agent Framework SDK** es el nuevo kit oficial para crear
agentes inteligentes y modulares capaces de razonar, ejecutar acciones y
colaborar con otros agentes.  
  
Proporciona:

- **Arquitectura Unificada de Agentes:** Sustituye AutoGen, Semantic
  Kernel y los orquestadores fragmentados.

- **Compatibilidad nativa con Microsoft Foundry:** Permite implementar
  agentes directamente en el Agent Service de Foundry.

- **Herramientas mediante MCP (Model Context Protocol):** Integración
  estandarizada con datos, APIs y sistemas.

- **Comunicación A2A nativa:** Los agentes pueden invocar a otros
  agentes como colaboradores autónomos.

Este SDK está diseñado para soportar sistemas de agentes de nivel
empresarial, con confiabilidad, observabilidad y gobernanza incorporadas
desde el inicio.

Objetivos del laboratorio

En este laboratorio realizará las siguientes tareas:

- Tarea 1: Abrir el proyecto preconfigurado en VS Code.

- Tarea 2: Crear el Planner Agent.

- Tarea 3: Crear los Worker Agents de HR y Compliance.

- Tarea 4: Definir la lógica de enrutamiento A2A (Agent Graph /
  Workflow).

- Tarea 5: Probar la conversación multiagente e inspeccionar logs.

## Tarea 1: Abrir el proyecto preconfigurado en VS Code

En esta tarea revisará la estructura de carpetas preconfiguradas para
comprender dónde se almacenan las definiciones de agentes, flujos de
trabajo y herramientas. Esto le prepara para extender el sistema
utilizando el Microsoft Agent Framework SDK.

1.  Desde el escritorio de la LabVM, abra **Visual Studio Code**.

2.  Una vez que Visual Studio Code esté abierto, haga clic en **File
    (1)** y seleccione la opción **Open Folder (2)** para abrir la
    carpeta del proyecto.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

3.  Una vez en el panel de carpetas abiertas, navegue hasta
    C:\Labfiles\Day 2\Enterprise-Agent-Code-Files y haga clic en
    **Select Folder**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

4.  Cuando se abra, aparecerá una ventana emergente. Haga clic en **Yes,
    I trust the authors**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

5.  Revise la estructura de carpetas del agente empresarial.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

6.  Haga clic derecho sobre el archivo **.env.example (1)** y seleccione
    **Rename (2)** para cambiar el nombre del archivo.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image5.png)

7.  Una vez hecho, cambie el nombre del archivo de **.env.example a
    .env** para activar este archivo de configuración de entorno para el
    agente.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image6.png)

8.  Reemplace el contenido del archivo **.env** con el siguiente:

> AZURE_OPENAI_ENDPOINT=https://agentic-
> @lab.LabInstance.Id.cognitiveservices.azure.com/
>
> AZURE_OPENAI_API_KEY=**\<Replace with Azure OpenAI key\>**
>
> AZURE_OPENAI_RESPONSES_DEPLOYMENT_NAME=gpt-4o-mini
>
> AZURE_OPENAI_API_VERSION=2025-03-01-preview

Desde la página de **Microsoft Foundry Overview**, copie la **API Key**
y reemplace el marcador \<**Replace with Azure OpenAI key**\> en el
archivo .env.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

9.  Una vez hecho, seleccione **File (1)** y luego haga clic en **Save
    (2)** para guardar el archivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image9.png)

## Tarea 2: Crear Planner Agent

En esta tarea, definirá un **Planner Agent**, que interpreta las
consultas del usuario y decide a qué agente especialista delegar tareas.
Configurará el agente utilizando el **Agent Framework SDK** con
instrucciones específicas según su rol.

1.  Desde la lista, seleccione **planner_agent.py** en la carpeta de
    agentes.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.png)

2.  Agregue el siguiente código Python para configurar el Planner Agent:

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

- **Propósito del Planner Agent:**

Este agente está diseñado para analizar las consultas de los usuarios y
decidir qué agente especialista (HR, Finance o Compliance) debe manejar
la respuesta.

**Creación del agente utilizando AzureOpenAIResponsesClient:**

- La función build_planner_agent() inicializa el Planner utilizando el
  Agent Framework SDK con credenciales basadas en API cargadas desde
  variables de entorno.

**Enrutamiento guiado por LLM (Lógica principal):**

- Se indica al Planner agent que devuelva exactamente una palabra — HR,
  FINANCE o COMPLIANCE — basada en palabras clave y contexto de la
  consulta.

**classify_target() para toma de decisiones:**

- Esta función primero utiliza una llamada await agent.run() para
  preguntar al Planner qué especialista seleccionar. Si la respuesta no
  es clara, aplica un análisis de respaldo basado en palabras clave.

> **Estrategia de IA híbrida + heurística:**

- El diseño asegura un enrutamiento confiable, combinando razonamiento
  del modelo con puntuación manual de palabras clave, haciendo que el
  Planner sea sólido incluso cuando la salida de la IA es vaga.

3.  Una vez hecho, seleccione **File (1)** y luego haga clic en **Save
    (2)** para guardar el archivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

## Tarea 3: Crear Worker Agents

En esta tarea, desarrollará agentes específicos de dominio responsables
del conocimiento de HR, Finance y Compliance. Cada agente se registrará
en el Agent Registry para habilitar su descubrimiento y delegación
mediante comunicación A2A.

1.  Desde la lista, seleccione **hr_agent.py** dentro de la carpeta
    agent y agregue el siguiente código Python para configurar el agente
    HR:

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

> **Propósito del HR Agent:**

- Este agente actúa como un experto dedicado en políticas de HR,
  entrenado para responder a preguntas relacionadas con el bienestar de
  los empleados, estructuras de permisos, beneficios y procedimientos
  internos.

> **Inicialización del agente con Azure Responses Client:**

- La función build_hr_agent() inicializa el agente usando
  AzureOpenAIResponsesClient, autenticado mediante claves API y valores
  de endpoint almacenados en variables de entorno.

> **Especialización por dominio:**

- La sección de instrucciones define claramente el alcance del HR agent
  incluyendo tipos de permisos, beneficios, incorporación, relaciones
  laborales y gestión del desempeño, asegurando que responda únicamente
  a consultas relacionadas con HR.

> **Tono profesional y empático:**

- El agente está diseñado para imitar estándares reales de comunicación
  de HR, proporcionando orientación precisa, profesional y empática,
  ideal para asistentes internos de la organización.

> **Base para la colaboración multiagente:**

- Una vez construido, este HR agent será invocado por el Planner Agent,
  permitiendo delegación automatizada en flujos de trabajo multiagente
  cuando se detecten consultas relacionadas con HR.

2.  Una vez hecho, seleccione **File (1)** y luego haga clic en **Save
    (2)** para guardar el archivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

3.  Desde la lista, seleccione **finance_agent.py** dentro de la carpeta
    agent y agregue el siguiente código Python para configurar el agente
    finance:

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

> Rol especializado en finanzas:

- Este agente está diseñado para manejar todos los temas relacionados
  con finanzas, incluyendo políticas de reembolso, presupuestos de
  viaje, asignaciones y aprobaciones de compras.

> **Inicialización vía Agent Framework SDK:**

- La función build_finance_agent() crea un agente usando
  AzureOpenAIResponsesClient, aprovechando la autenticación con clave
  API desde variables de entorno seguras.

> **Instrucciones orientadas a políticas:**

- Las instrucciones del agente limitan claramente su responsabilidad a
  procedimientos financieros, asegurando respuestas precisas sobre
  costos, pagos, presupuestos y reglas corporativas de gastos.

> **Precisión y resultados accionables:**

- A diferencia de agentes de propósito general, este asistente
  financiero está instruido para proporcionar valores específicos de
  políticas, como límites, elegibilidad o flujos de aprobación,
  haciéndolo práctico para los empleados.

> **Soporte a la delegación del Planner (A2A):**

- Este agente será invocado automáticamente cuando el Planner detecte
  palabras clave o consultas relacionadas con finanzas, habilitando
  colaboración multiagente sin interrupciones.

4.  Una vez hecho, seleccione **File (1)** y luego haga clic en **Save
    (2)** para guardar el archivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

5.  Desde la lista, seleccione **compliance_agent.py** dentro de la
    carpeta agent y agregue el siguiente código Python para configurar
    el agente Compliance:

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

> **Propósito del agente:**

- Este agente sirve como autoridad dedicada en temas legales y de
  cumplimiento, responsable de manejar consultas relacionadas con GDPR,
  marcos regulatorios, derecho contractual, evaluaciones de riesgo y
  estándares de seguridad.

> **Inicialización del agente:**

- La función build_compliance_agent() utiliza AzureOpenAIResponsesClient
  con autenticación por clave API para registrar el Compliance agent
  mediante Microsoft Agent Framework SDK.

> **Experiencia regulatoria definida en las instrucciones:**

- Las instrucciones proporcionan un alcance de cumplimiento claro —
  incluyendo regulaciones globales de privacidad (GDPR, HIPAA, SOX),
  preparación de auditorías, acuerdos legales y protocolos de
  notificación de incidentes, asegurando respuestas confiables.

> **Tono y expectativas de salida:**

- Este agente está configurado para entregar respuestas en tono formal y
  autoritario, incluyendo citas legales o recomendaciones de
  implementación cuando corresponda.

> **Rol en sistema multiagente:**

- Durante la delegación A2A, el Planner Agent enviará consultas legales
  o de cumplimiento a este especialista, manteniendo precisión y
  gobernanza en los flujos de decisión empresariales.

6.  Una vez hecho, seleccione **File (1)** y luego haga clic en **Save
    (2)** para guardar el archivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

## Tarea 4: Definir la lógica de enrutamiento A2A (Agent Graph / Workflow)

Agent-to-Agent (A2A) es una capacidad central del Microsoft Agent
Framework que permite a un agente delegar tareas autónomamente a otro
agente.

En esta tarea, implementará la lógica de enrutamiento utilizando un
Agent Workflow para que el Planner pueda invocar de manera autónoma los
agentes HR o Compliance según la intención de la consulta. Esto
establece una verdadera colaboración multiagente.

1.  Desde la lista, seleccione **main.py** dentro de la carpeta agent y
    agregue el siguiente código Python para configurar el flujo de
    comunicación A2A:

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

> **Motor central de ejecución:**

- Este script actúa como el núcleo orquestador, coordinando todos los
  agentes (Planner, HR, Finance, Compliance) y gestionando el
  enrutamiento multiagente usando Microsoft Agent Framework.

> **Inicialización de la red de agentes:**

- Carga las configuraciones del entorno, construye cada agente con await
  build\_\*\_agent() y los registra en un diccionario compartido para
  facilitar la delegación.

> **Enrutamiento A2A Avanzado:**

- La función run_multi_agent() dirige las consultas del usuario al
  especialista adecuado a través del Planner, luego espera la respuesta
  del agente especialista. Captura enrutamiento, tiempo, estado de éxito
  y respuesta final.

> **Múltiples modos de ejecución:**

- Modo Batch: Ejecuta consultas de prueba predefinidas.

- Modo Interactivo (--interactive): Permite chat en tiempo real para
  pruebas y exploración.

> **Resiliencia lista para producción:**

- Incluye formateo de respuesta, marcas de tiempo, mecanismos de
  respaldo ante errores y logging, estableciendo una base sólida para
  observabilidad, telemetría y AgentOps en ejercicios posteriores.

2.  Una vez hecho, seleccione **File (1)** y luego haga clic en **Save
    (2)** para guardar el archivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

## Tarea 5: Probar la conversación multiagente e inspeccionar logs

En esta tarea, ejecutará consultas de prueba de extremo a extremo a
través del sistema multiagente y observará la colaboración de los
agentes mediante logs y telemetría en Microsoft Foundry.

1.  Ha configurado correctamente el sistema multiagente con un Planner
    agent y agentes especialistas. Ahora, probará el funcionamiento de
    este sistema.

> **Nota:** Aunque el sistema multiagente ahora tiene capacidades LLM,
> todavía no cuenta con integración MCP ni acceso a fuentes externas de
> conocimiento como datasets o índices de Azure AI Search. En esta
> etapa, los agentes dependerán únicamente de su inteligencia general de
> modelo para responder preguntas.

2.  Seleccione la opción **... (1)** desde el menú superior para
    expandir el menú. Seleccione **Terminal (2)** y haga clic en **New
    Terminal (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image16.png)

3.  Una vez abierto el terminal, ejecute el comando,

+++pip install -r requirements.txt+++ para instalar todos los paquetes
requeridos.

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image17.png)

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image18.png)

4.  Una vez finalizada la instalación correctamente, ejecute el
    siguiente comando para ejecutar el agente y revisar las respuestas
    de las pruebas proporcionadas en el archivo de código.

+++python main.py+++

> ![A computer screen shot of a program AI-generated content may be
> incorrect.](./media/image19.png)

Revise el parámetro **Routed to** y observe cómo el agente determina y
enruta las solicitudes a los agentes especialistas correspondientes.

5.  Ahora, ejecute nuevamente el agente en modo interactivo agregando la
    bandera --interactive. Esto le permitirá ingresar preguntas y
    obtener respuestas en tiempo real. Proporcione el siguiente prompt
    cuando se solicite.

    - Comando:

> +++python main.py –interactive+++

- Prompt:

> +++How much reimbursement is allowed for international flights?+++

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image20.png)

6.  Una vez obtenida la respuesta, en el siguiente prompt, ingrese **q**
    para salir o detener el agente.

![A black screen with white text AI-generated content may be
incorrect.](./media/image21.png)

**Resumen**

En este laboratorio, definió tres agentes (Planner, HR y Compliance)
utilizando el Microsoft Agent Framework SDK y los registró. Creó un
flujo de trabajo de enrutamiento para delegar consultas de usuarios
mediante llamadas Agent-to-Agent. Probó un escenario multiagente e
inspeccionó los logs para confirmar el enrutamiento correcto de los
mensajes y el flujo de ejecución.
