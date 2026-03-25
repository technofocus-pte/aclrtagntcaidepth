# Laboratorio 7: Creación de un sistema de gestión de tickets impulsado por IA mediante MCP

**Duración estimada:** 60 Minutos

**Descripción general**

En este laboratorio, ampliará su sistema de agentes empresariales
conectándolo a datos organizacionales reales y servicios externos
mediante el **Model Context Protocol (MCP)**. Integrará **Azure AI
Search** para proporcionar respuestas fundamentadas en contexto a partir
de bases de conocimiento indexadas y conectará la API de **Freshdesk**
para permitir que los agentes realicen acciones del mundo real, como
crear tickets de Recursos Humanos o Finanzas.

Al completar este laboratorio, sus agentes evolucionarán de modelos de
conversación estáticos a asistentes inteligentes, conscientes de los
datos y capaces de actuar, interactuando de forma segura con los
sistemas empresariales.

Objetivos del laboratorio

En este laboratorio, realizará las siguientes tareas:

- Tarea 1: Crear la herramienta Azure Search MCP.

- Tarea 2: Adjuntar la herramienta a los agentes, enriquecer los prompts
  y ejecutar pruebas.

- Tarea 3: Configurar Freshworks para la gestión de tickets.

- Tarea 4: Conectar agentes a una API externa (Integración Freshdesk
  MCP).

## Tarea 1: Crear la herramienta Azure Search MCP

En esta tarea, actualizará las variables de entorno con las credenciales
de AI Search, creará una clase de herramienta async que consulte Azure
AI Search y devuelva los fragmentos principales de los documentos
**(top-N)**, que los agentes usarán como contexto.  
MCP es un estándar que permite que los agentes de IA accedan de manera
segura a conocimientos y herramientas externas mediante contratos
estructurados de entrada/salida. Esto permite que los agentes recuperen
**información factual**, invoquen APIs y realicen acciones de manera
controlada y auditable, formando la base de sistemas de IA empresariales
fundamentados y extensibles.

1.  En el **Azure Portal**, navegue a **agenticai** y, en la lista de
    recursos, seleccione el servicio **ai-knowledge-Search**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

2.  Seleccione **Keys (1)** en el menú izquierdo, bajo **Settings**, y
    copie la **Query Key (2)** usando la opción de copiar como se
    muestra.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

3.  Una vez copiada, péguela de manera segura en un bloc de notas,
    seleccione **Indexes (1)** en el menú izquierdo bajo **Search
    Management**, y copie el **Index Name (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

4.  Como ya ha creado un sistema multiagente previamente, en el panel de
    **Visual Studio Code**, seleccione el archivo .env para agregar las
    claves de AI Search para la conexión.

5.  En el archivo .env, agregue lo siguiente debajo de las claves de AI
    Foundry. Reemplace **\[Query_Key\]** y **\[Index_Name\]** con los
    valores copiados previamente.

> AZURE_SEARCH_ENDPOINT=https://ai-knowledge--@lab.LabInstance.Id.search.windows.net/
>
> AZURE_SEARCH_API_KEY=\[Query_Key\]
>
> AZURE_SEARCH_INDEX=\[Index_Name\]

6.  Una vez hecho, guarde el archivo. Haga clic en **File (1)** en el
    menú superior y seleccione **Save (2)**.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image4.png)

7.  Haga clic en **Create Folder** y, cuando se le solicite, proporcione
    el nombre de la carpeta: tools. Cree la carpeta en la raíz del
    proyecto.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

8.  Tras crearla, la estructura de carpetas se verá similar a esta.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

9.  Seleccione la carpeta **tools (1)** y haga clic en **Create File
    (2)**. Esto creará un archivo dentro de la carpeta tools.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

10. Nombre el archive cómo azure_search_tool.py

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

11. Una vez creado, agregue el siguiente fragmento de código para
    configurar la herramienta de búsqueda para su agente.

> import os
>
> import sys
>
> import aiohttp
>
> import json
>
> import asyncio
>
> from typing import List
>
> from pathlib import Path
>
> \# Add parent directory to path for imports
>
> sys.path.append(str(Path(\_\_file\_\_).parent.parent))
>
> class AzureSearchTool:
>
> """
>
> Async MCP-like tool to query Azure Cognitive Search index.
>
> Reads endpoint/key/index from environment and returns joined content
> snippets.
>
> """
>
> def \_\_init\_\_(self):
>
> self.endpoint = os.getenv("AZURE_SEARCH_ENDPOINT")
>
> self.api_key = os.getenv("AZURE_SEARCH_API_KEY")
>
> self.index_name = os.getenv("AZURE_SEARCH_INDEX")
>
> if not all(\[self.endpoint, self.api_key, self.index_name\]):
>
> raise RuntimeError("Azure Search env vars not set
> (AZURE_SEARCH_ENDPOINT/KEY/INDEX).")
>
> if not self.endpoint.endswith('/'):
>
> self.endpoint += '/'
>
> self.api_version = "2023-11-01"
>
> async def search(self, query: str, top: int = 5) -\> str:
>
> """
>
> Query the Azure Search index and return concatenated content snippets.
>
> """
>
> url =
> f"{self.endpoint}indexes/{self.index_name}/docs/search?api-version={self.api_version}"
>
> headers = {"Content-Type": "application/json", "api-key":
> self.api_key}
>
> body = {"search": query, "top": top}
>
> async with aiohttp.ClientSession() as session:
>
> async with session.post(url, headers=headers, json=body) as resp:
>
> if resp.status != 200:
>
> text = await resp.text()
>
> raise RuntimeError(f"Azure Search error {resp.status}: {text}")
>
> data = await resp.json()
>
> docs = data.get("value", \[\])
>
> snippets: List\[str\] = \[\]
>
> for d in docs:
>
> snippet = d.get("content") or d.get("text") or d.get("description") or
> json.dumps(d)
>
> snippets.append(snippet.strip())
>
> return "\n\n".join(snippets) if snippets else "No results found."
>
> async def health_check(self):
>
> """Check if Azure Search service is accessible."""
>
> try:
>
> url =
> f"{self.endpoint}indexes/{self.index_name}?api-version={self.api_version}"
>
> headers = {"Content-Type": "application/json", "api-key":
> self.api_key}
>
> async with aiohttp.ClientSession() as session:
>
> async with session.get(url, headers=headers) as resp:
>
> return {
>
> "status": "healthy" if resp.status == 200 else "unhealthy",
>
> "status_code": resp.status,
>
> "endpoint": self.endpoint,
>
> "index": self.index_name
>
> }
>
> except Exception as e:
>
> return {
>
> "status": "error",
>
> "error": str(e),
>
> "endpoint": self.endpoint,
>
> "index": self.index_name
>
> }
>
> \# Example usage and testing
>
> async def main():
>
> """Test the Azure Search tool"""
>
> try:
>
> \# Load environment variables
>
> from utils.env import load_env
>
> load_env()
>
> \# Initialize search tool
>
> search_tool = AzureSearchTool()
>
> \# Health check
>
> health = await search_tool.health_check()
>
> print(f"Health Status: {json.dumps(health, indent=2)}")
>
> \# Test search
>
> test_queries = \[
>
> "travel reimbursement policy",
>
> "GDPR compliance requirements",
>
> "employee leave policies"
>
> \]
>
> for query in test_queries:
>
> print(f"\n{'='\*60}")
>
> print(f"Testing search: {query}")
>
> print('='\*60)
>
> result = await search_tool.search(query, top=3)
>
> print(result)
>
> except Exception as e:
>
> print(f"Error testing Azure Search tool: {e}")
>
> if \_\_name\_\_ == "\_\_main\_\_":
>
> import asyncio
>
> asyncio.run(main())

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image9.png)

> **Propósito de AzureSearchTool:**

- Esta herramienta proporciona una interfaz **async compatible con MCP**
  para que los agentes consulten el índice de Azure Cognitive Search y
  obtengan fragmentos de contexto relevantes para las consultas de los
  usuarios.

> **Configuración basada en entorno:**

- La herramienta lee las credenciales de Azure Search (endpoint, API key
  e index name) directamente de las variables de entorno, asegurando
  configuración segura y flexible.

> **Funcionalidad principal de búsqueda (método search):**

- El método search() envía una solicitud POST asíncrona a la API REST de
  Azure Search, obteniendo los documentos principales y concatenando sus
  campos de contenido en una cadena de texto contextual.

> **Modo de pruebas y diagnóstico:**

- El archivo incluye una rutina main() incorporada que carga las
  variables de entorno, realiza una verificación de salud en vivo y
  ejecuta consultas de ejemplo para una validación rápida antes de la
  integración con agentes.

12. Una vez hecho, seleccione **File (1)** y luego haga clic en **Save
    (2)** para guardar el archivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image10.png)

13. En el menú superior, haga clic en ... **(1)** para desplegar
    opciones adicionales. Luego, seleccione **Terminal (2)** y haga clic
    en **New Terminal (3).**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

14. Ejecute el comando que se indica a continuación para probar el
    funcionamiento de la herramienta de búsqueda.

+++python .\tools\azure_search_tool.py+++

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image12.png)

15. Ha creado correctamente una herramienta **MCP** que conecta sus
    datos de **Azure AI Search** con el agente, permitiéndole recuperar
    el **contexto relevante** de su **base de conocimientos indexada**.

## Tarea 2: Adjuntar la herramienta a los agentes, enriquecer prompts y ejecutar pruebas

En esta tarea, conectará la herramienta **AzureSearchTool** que creó en
la Tarea 1 con sus agentes, enriquecerá los **prompts** con
instrucciones contextuales y probará la recuperación de información
basada en Azure Search. Esto permite que los agentes respondan consultas
con datos verificados y específicos de la organización.

1.  Como ya ha creado la herramienta, ahora debe modificar la
    orquestación del agente para que **active la herramienta de búsqueda
    antes de proporcionar una respuesta**.

2.  En el explorador de **Visual Studio Code**, abra el archivo main.py
    y reemplace su contenido existente con el siguiente código:

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
> Advanced multi-agent system with routing, search context, and ticket
> creation capabilities.
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
> Please provide a comprehensive answer based PRIMARILY on the context
> information provided above.
>
> Use the knowledge base content as your primary source of truth. If the
> context contains relevant
>
> information, base your answer on that. Only supplement with general
> knowledge if the context
>
> doesn't cover the specific question.
>
> If no relevant information is found in the context, clearly state that
> and provide general guidance
>
> while recommending the user contact the appropriate department for
> specific details.
>
> """
>
> \# Step 4: Get response from appropriate agent
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
> "ticket_created": False, \# Tickets only created in interactive mode
>
> "ticket_info": None,
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
> "context_retrieved": False,
>
> "ticket_created": False,
>
> "ticket_info": None,
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
> - Ask any question about HR, Finance, or Compliance
>
> - 'quit' or 'exit' - Exit the system
>
> - 'help' - Show this help message
>
> 🎯 Example questions:
>
> - "What's the travel reimbursement limit for meals?"
>
> - "How many vacation days do employees get?"
>
> - "Do we need GDPR compliance for EU customers?"
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
> """Run focused test queries with grounded data integration."""
>
> test_queries = \[
>
> "What is the travel reimbursement limit for hotel stays?",
>
> "How many vacation days are allowed per year?"
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
> print("🚀 Initializing Enterprise Agent System with Tools...")
>
> try:
>
> \# Load environment and build agents
>
> load_env()
>
> logging.info("Building agent network...")
>
> \# Build core agents
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
> \# Freshdesk integration removed - focusing on grounded search
> responses only
>
> logging.info("✅ All agents and tools initialized")
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
> \# Try to run with minimal configuration
>
> logging.info("Attempting to run with minimal configuration...")
>
> try:
>
> minimal_agents = {
>
> "planner": await build_planner_agent(),
>
> "hr": await build_hr_agent(),
>
> "compliance": await build_compliance_agent(),
>
> "finance": await build_finance_agent(),
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
> print(f"❌ Even minimal configuration failed: {minimal_error}")
>
> if \_\_name\_\_ == "\_\_main\_\_":
>
> asyncio.run(main())

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image13.png)

> **Propósito del script principal actualizado:**

- Esta versión extiende el orquestador anterior para **integrar Azure AI
  Search mediante MCP**, permitiendo que las respuestas de cada agente
  estén fundamentadas en datos empresariales en lugar de depender
  únicamente del razonamiento genérico del LLM.

> **Integración de la herramienta de búsqueda (Nueva funcionalidad):**

- La instancia AzureSearchTool se inicializa y se adjunta al diccionario
  agents como agents\["search_tool"\].

- Antes de ejecutar el sistema, realiza un **health check** para
  confirmar la conectividad a Azure AI Search y la disponibilidad del
  índice.

> **Lógica de enriquecimiento del contexto (run_multi_agent mejorado):**

- Para cada consulta, el sistema obtiene fragmentos relacionados desde
  Azure Search (context = await agents\["search_tool"\].search(query)).

- El contexto recuperado se incorpora directamente en el **prompt**
  antes de enviarlo al agente especializado.

- Se instruye explícitamente al LLM a basar las respuestas
  principalmente en estos datos contextuales, garantizando respuestas
  fundamentadas.

3.  Una vez hecho, seleccione **File (1)** y luego haga clic en **Save
    (2)** para guardar el archivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image10.png)

4.  Seleccione la opción **... (1)** en el menú superior para desplegar
    el menú, luego seleccione **Terminal (2)** y haga clic en **New
    Terminal (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

5.  Una vez abierta la terminal, ejecute el siguiente comando para
    iniciar el agente y revisar las respuestas de los prompts de prueba
    incluidos en el archivo de código:

+++python main.py+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image14.png)

Verifique el parámetro **Context retrieved** y observe cómo el agente
obtiene el contexto a partir de los datos fundamentados.

6.  Ahora, ejecute nuevamente el agente en **modo interactivo**
    agregando la bandera --interactive. Esto le permitirá ingresar
    preguntas y recibir respuestas en tiempo real.

- Ejemplo de pregunta a ingresar cuando se le solicite:

+++python main.py –interactive+++

+++Is employee data protected under GDPR?+++

7.  Una vez obtenida la respuesta, en el siguiente prompt ingrese q para
    **salir** o detener el agente.

![A black screen with white text AI-generated content may be
incorrect.](./media/image15.png)

## Tarea 3: Configuración de Freshworks para la gestión de tickets

En esta tarea, configurará Freshworks para habilitar la gestión de
tickets y una integración empresarial para su sistema multiagente.

**Freshworks** es una plataforma en la nube para servicio al cliente y
gestión de interacciones, diseñada para mejorar las operaciones de
soporte y aumentar la satisfacción del usuario. Ofrece un conjunto de
herramientas para la gestión de tickets, chat en vivo, creación de
centros de ayuda y autoservicio del cliente. Freshworks permite la
comunicación omnicanal, lo que facilita que las empresas administren
interacciones a través de correo electrónico, chat, teléfono y redes
sociales desde una interfaz centralizada. Sus funcionalidades de
automatización ayudan a optimizar flujos de trabajo, asignar tickets y
proporcionar análisis para el seguimiento del rendimiento. Ahora
configurará la cuenta de Freshworks.

1.  Copie la URL y péguela en una nueva pestaña de su navegador dentro
    de la VM para abrir el portal de **Freshworks**.

    - URL:

+++https://www.freshworks.com/freshdesk/lp/home/?tactic_id=3387224&utm_source=google-adwords&utm_medium=FD-Search-Brand-India&utm_campaign=FD-Search-Brand-India&utm_term=freshdesk&device=c&matchtype=e&network=g&gclid=EAIaIQobChMIuOK90qvLjQMV_dQWBR3JAi9VEAAYASAAEgK87_D_BwE&audience=kwd-30002131023&ad_id=282519464145&gad_source=1&gad_campaignid=671502402+++

2.  En el portal, seleccione **Start free trial** para iniciar la prueba
    gratuita.

![](./media/image16.png)

3.  En el siguiente panel, proporcione estos datos y haga clic en **Try
    it free (6):**

    - **First name:** +++LODS+++

    - **Last name:** +++User1+++

    - **Work
      email:** **+++@lab.CloudPortalCredential(User1).Username+++**

    - **Company name:** Zava

    - **Organization size:** Seleccione **1-10**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image17.png)

4.  En el siguiente panel, proporcione los siguientes datos y haga clic
    en **Next (4):**

    - **What industry are you from?:** de la lista, seleccione
      **Software and internet (1)**

    - **How many employees are there in your
      company?:** seleccione **1-10 (2)**

    - Seleccione **I'm trying customer service software for the first
      time (3)**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.png)

5.  Una vez hecho, copie la URL mostrada a continuación y péguela en una
    nueva pestaña de su navegador dentro de la VM para abrir
    **Outlook.**

    - URL:
      +++https://go.microsoft.com/fwlink/p/?LinkID=2125442&clcid=0x409&culture=en-us&country=us+++

6.  En el panel **pick an account**, seleccione la cuenta asignada para
    este laboratorio.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

7.  En el correo de verificación de Freshworks, ábralo y haga clic en
    **Activate Account**.

> ![A screenshot of a computer screen AI-generated content may be
> incorrect.](./media/image20.png)
>
> **Nota:** Si no puede localizar el correo de activación de Freshworks,
> espere unos minutos, ya que podría haber un retraso en la entrega.  
> Si el correo no llega después de un tiempo, considere repetir los
> pasos para iniciar la prueba gratuita en una ventana
> privada/incógnito.  
> Además, revise las carpetas de **spam** o **junk**, ya que el correo
> pudo haber sido filtrado allí.

8.  En el siguiente panel, proporcione un valor en **Enter password
    (1)** y proporcione la misma contraseña en **Confirm password (2)**.
    Haga clic en **Activate your account (3)**.

> ![A screenshot of a login screen AI-generated content may be
> incorrect.](./media/image21.png)

9.  Una vez en el portal, haga clic en el ícono **Profile (1)** en la
    esquina superior derecha y seleccione **Profile settings (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

10. En la página del perfil, haga clic en **View API Key** para obtener
    las API Keys.

![A screenshot of a web page AI-generated content may be
incorrect.](./media/image23.png)

**Nota:** Si no puede encontrar esta opción, reduzca el tamaño de la
pantalla usando **CTRL + -**.

11. En el siguiente panel, complete el **CAPTCHA**.

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image24.png)

12. Copie la API Key en un bloc de notas; la utilizará más adelante.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

13. Desde la pestaña del navegador, copie el **Account URL** como se
    muestra y copie el valor en un bloc de notas. Lo utilizará más
    adelante.

![](./media/image26.png)

14. Desde la izquierda, haga clic en el ícono **Tickets** del menú
    izquierdo y podrá ver algunos tickets predeterminados.

![A screenshot of a social media post AI-generated content may be
incorrect.](./media/image27.png)

15. Una vez hecho lo anterior, navegue al panel de Visual Studio Code y
    abra el archivo .env.

16. En el archivo .env, agregue el siguiente contenido e incluya la
    clave y el dominio que copió anteriormente.

> \# Freshdesk Configuration
>
> FRESHDESK_DOMAIN=\[Domain_URL\]
>
> FRESHDESK_API_KEY=\[API_Key\]

![A black and white text with red lines AI-generated content may be
incorrect.](./media/image28.png)

![A computer screen shot of a program AI-generated content may be
incorrect.](./media/image29.png)

17. Una vez hecho, seleccione **File (1)** y luego haga clic en **Save
    (2)** para guardar el archivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image10.png)

## Tarea 4: Conectar los agentes con una API Externa (Integración Freshdesk MCP)

En esta tarea, usted creará una herramienta MCP adicional que conectará
sus agentes con una instancia externa de Freshdesk mediante su REST API.
Esta herramienta permitirá que los agentes, especialmente Finance y HR,
creen tickets reales cuando los usuarios soliciten acciones como
reembolsos, aprobaciones de viaje o aclaraciones de políticas.

1.  Una vez que las variables de entorno estén configuradas, es momento
    de crear la herramienta que gestionará la integración con Freshdesk.

2.  Desde el menú Explorer, seleccione la carpeta **tools (1)** y haga
    clic en la opción **Create File (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.png)

3.  Proporcione el nombre del archivo como **freshdesk_tool.py**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

4.  Ahora, seleccione el archivo y agregue el siguiente fragmento de
    código para configurar la herramienta.

> import os
>
> import aiohttp
>
> import base64
>
> import ssl
>
> from typing import Dict, Any, Optional
>
> class FreshdeskTool:
>
> """
>
> Async Freshdesk tool to create tickets via Freshdesk REST API.
>
> """
>
> def \_\_init\_\_(self):
>
> self.domain = os.getenv("FRESHDESK_DOMAIN")
>
> self.api_key = os.getenv("FRESHDESK_API_KEY")
>
> self.default_priority = int(os.getenv("FRESHDESK_DEFAULT_PRIORITY",
> "1") or 1)
>
> self.default_group_id = os.getenv("FRESHDESK_DEFAULT_GROUP_ID") or
> None
>
> if not self.domain or not self.api_key:
>
> raise RuntimeError("Freshdesk domain/API key missing in environment.")
>
> self.base_url = f"https://{self.domain}/api/v2"
>
> auth_bytes = f"{self.api_key}:X".encode("utf-8")
>
> auth_header = base64.b64encode(auth_bytes).decode("utf-8")
>
> self.headers = {
>
> "Authorization": f"Basic {auth_header}",
>
> "Content-Type": "application/json"
>
> }
>
> async def create_ticket(self, subject: str, description: str,
> requester: Optional\[Dict\[str, str\]\] = None, tags: Optional\[list\]
> = None) -\> Dict\[str, Any\]:
>
> url = f"{self.base_url}/tickets"
>
> payload: Dict\[str, Any\] = {
>
> "subject": subject,
>
> "description": description,
>
> "priority": self.default_priority,
>
> "status": 2 \# 2 = Open status in Freshdesk
>
> }
>
> if self.default_group_id:
>
> try:
>
> payload\["group_id"\] = int(self.default_group_id)
>
> except ValueError:
>
> pass
>
> if tags:
>
> payload\["tags"\] = tags
>
> if requester:
>
> if requester.get("email"):
>
> payload\["email"\] = requester.get("email")
>
> if requester.get("name"):
>
> payload\["name"\] = requester.get("name")
>
> \# Create SSL context that allows insecure connections for testing
>
> ssl_context = ssl.create_default_context()
>
> ssl_context.check_hostname = False
>
> ssl_context.verify_mode = ssl.CERT_NONE
>
> connector = aiohttp.TCPConnector(ssl=ssl_context)
>
> async with aiohttp.ClientSession(connector=connector) as session:
>
> async with session.post(url, headers=self.headers, json=payload) as
> resp:
>
> data = await resp.json()
>
> if resp.status not in (200, 201):
>
> raise RuntimeError(f"Freshdesk API error {resp.status}: {data}")
>
> ticket = {
>
> "id": data.get("id"),
>
> "status": data.get("status"),
>
> "priority": data.get("priority"),
>
> "url": f"https://{self.domain}/helpdesk/tickets/{data.get('id')}"
>
> }
>
> return {"success": True, "ticket": ticket, "raw": data}
>
> async def health_check(self):
>
> """Check Freshdesk connectivity by fetching sample endpoint (accounts
> may not allow GETs; this is best-effort)."""
>
> try:
>
> url = f"{self.base_url}/agents"
>
> \# Create SSL context that allows insecure connections for testing
>
> ssl_context = ssl.create_default_context()
>
> ssl_context.check_hostname = False
>
> ssl_context.verify_mode = ssl.CERT_NONE
>
> connector = aiohttp.TCPConnector(ssl=ssl_context)
>
> async with aiohttp.ClientSession(connector=connector) as session:
>
> async with session.get(url, headers=self.headers) as resp:
>
> return {"status": "healthy" if resp.status in (200, 403) else
> "unhealthy", "status_code": resp.status}
>
> except Exception as e:
>
> return {"status": "error", "error": str(e)}

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image32.png)

> **Propósito de FreshdeskTool:**

- Esta clase proporciona una interfaz asincrónica para integrar la REST
  API de Freshdesk en el flujo de trabajo de su agente, lo que permite
  la creación automatizada de tickets directamente a partir de las
  acciones del agente.

> **Autenticación y configuración:**

- Realiza la autenticación utilizando su Freshdesk API key mediante
  Basic Auth y crea solicitudes hacia el endpoint /api/v2/tickets.

- El URL base, la API key y las configuraciones predeterminadas
  (priority, group ID) se leen de forma segura desde las variables de
  entorno.

> **Lógica de creación de tickets:**

- El método create_ticket() crea un payload JSON estructurado que
  incluye subject, description, la información del requester y etiquetas
  opcionales (tags).

- Realiza una solicitud asincrónica POST a Freshdesk, creando un ticket
  real y devolviendo metadatos normalizados (ID, status, priority, URL).

5.  Una vez que haya terminado, seleccione **File** **(1)** y luego haga
    clic en **Save** **(2)** para guardar el archivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image10.png)

6.  Desde el menú del explorador, seleccione **main.py**.

7.  Reemplace el código existente con el siguiente fragmento.

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
> "I need to create a ticket for my travel reimbursement request"
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
> print("🚀 Initializing Enterprise Agent System with Tools...")
>
> try:
>
> \# Load environment and build agents
>
> load_env()
>
> logging.info("Building agent network...")
>
> \# Build core agents
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
> logging.info("✅ All agents and tools initialized")
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
> \# Try to run with minimal configuration
>
> logging.info("Attempting to run with minimal configuration...")
>
> try:
>
> minimal_agents = {
>
> "planner": await build_planner_agent(),
>
> "hr": await build_hr_agent(),
>
> "compliance": await build_compliance_agent(),
>
> "finance": await build_finance_agent(),
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
> print(f"❌ Even minimal configuration failed: {minimal_error}")
>
> if \_\_name\_\_ == "\_\_main\_\_":
>
> asyncio.run(main())

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image33.png)

> **run_multi_agent_with_user_info():**

- Esta función amplía la lógica original del sistema multiagente para
  manejar interacciones personalizadas con el usuario y la creación
  automática de tickets.

- Detecta un bloque CREATE_TICKET en la respuesta del agente, extrae los
  detalles (subject, body, tags, información del requester) y activa
  automáticamente la creación del ticket en Freshdesk.

- Una vez que el ticket es creado, reemplaza dinámicamente el bloque por
  un mensaje de confirmación que muestra el ID del ticket, el asunto y
  la URL.

> **interactive_ticket_creation():**

- Introduce un flujo guiado de creación manual de tickets, donde el
  usuario puede ingresar el asunto y la descripción del ticket
  directamente a través de los prompts.

- Luego llama de forma asincrónica a la herramienta Freshdesk para crear
  el ticket y muestra los detalles de confirmación directamente en la
  terminal.

8.  Una vez terminado, seleccione **File** **(1)** y luego haga clic en
    **Save (2)** para guardar el archivo.

![](./media/image10.png)

9.  Dado que el archivo principal (main orchestrator) ya está
    configurado, ahora debe agregar instrucciones a cada agente para
    habilitar la creación de tickets.

10. Seleccione **finance_agent.py** en el panel izquierdo y, dentro del
    archivo, busque la sección **Instructions**, donde se definen las
    instrucciones del agente.

11. Agregue las siguientes instrucciones debajo de las ya existentes.

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

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image34.png)

12. Una vez realizado esto, guarde el archivo seleccionando **Save**.

13. Seleccione **hr_agent.py** desde el panel izquierdo y, dentro del
    archivo, busque la sección **Instructions**, donde se definen las
    instrucciones del agente.

14. Agregue las siguientes instrucciones debajo de las instrucciones ya
    existentes.

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
> "Email: john.doe@example.com (optional)\n"
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

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image35.png)

15. Una vez hecho esto, seleccione **File (1)** y luego haga clic en
    **Save** **(2)** para guardar el archivo.

![](./media/image10.png)

16. Seleccione la opción **…** **(1)** en el menú superior para expandir
    el menú. Seleccione **Terminal (2)** y haga clic en **New Terminal**
    **(3).**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

17. Una vez que el terminal esté abierto, pegue el siguiente comando
    para ejecutar el agente y revisar las respuestas y la creación de
    tickets:

+++python main.py+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.png)

18. Navegue al portal de Freshdesk, seleccione **Tickets** desde el menú
    izquierdo y verifique el nuevo ticket creado.

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image37.png)

**Nota:** Si no puede ver el ticket, espere unos segundos o actualice la
página para visualizarlo.

19. Ahora, ejecute el agente nuevamente en modo interactivo agregando el
    flag **--interactive**. Esto le permitirá ingresar la pregunta y
    obtener la respuesta. Proporcione el siguiente prompt como pregunta
    cuando el sistema lo solicite:

I need to create a ticket for my travel reimbursement request. I
traveled to New York City from October 15-18, 2025, for the annual
client conference. The purpose was to present our new product line to
key stakeholders and attend strategic planning sessions. My expenses
include flights ($650), hotel accommodation ($480 for 3 nights), meals
($320), local transportation ($85), and conference materials ($45). The
total reimbursement amount is $1,580. I have all receipts ready for
submission.

20. Regrese a Freshworks y verifique el nuevo ticket creado, y revise
    cómo el agente añadió los detalles utilizando la información
    proporcionada.

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image38.png)

![A screenshot of a travel reimbursement AI-generated content may be
incorrect.](./media/image39.png)

21. Una vez que haya obtenido la respuesta, en el siguiente prompt
    agregue **q** para cerrar el agente o detener su ejecución.

![A black screen with white text AI-generated content may be
incorrect.](./media/image15.png)

22. Ha creado satisfactoriamente un sistema multiagente que recupera
    conocimiento contextual desde Azure AI Search e integra de manera
    fluida Freshdesk para la gestión de tickets a nivel empresarial.

**Resumen**

En este laboratorio, usted probó con éxito el sistema multiagente
completo, donde los agentes recuperaron información relevante desde
Azure AI Search, identificaron el departamento adecuado para manejar
cada consulta y crearon tickets en Freshdesk cuando fue necesario. Esto
demostró cómo los agentes pueden combinar razonamiento contextual con
acciones del mundo real, haciéndolos más eficientes y prácticos para
flujos de trabajo empresariales.

Ha completado este laboratorio con éxito. Haga clic en **Next \>\>**
para continuar.
