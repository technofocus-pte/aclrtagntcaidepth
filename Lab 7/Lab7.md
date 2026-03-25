# Laboratório 7: Criando um Sistema de Gerenciamento de Tíquetes com AI usando MCP

**Duração estimada:** 60 minutos

**Visão geral**

Neste laboratório, você irá implementar a extensão do seu sistema de
agentes corporativos conectando-o a dados organizacionais reais e a
serviços externos usando o Model Context Protocol (MCP). Você irá
integrar o Azure AI Search para fornecer respostas contextualizadas e
fundamentadas a partir de bases de conhecimento indexadas e conectar a
API do Freshdesk para permitir que os agentes executem ações no mundo
real, como criar tíquetes de RH ou Finanças.

Ao concluir este laboratório, seus agentes evoluirão de modelos
conversacionais estáticos para assistentes inteligentes, orientados por
dados e por ações, capazes de interagir de forma segura com sistemas
corporativos.

Objetivos do laboratório

Neste laboratório, você irá realizar as seguintes tarefas:

- Tarefa 1: Construir a ferramenta MCP do Azure Search

- Tarefa 2: Anexar a ferramenta aos agentes, enriquecer prompts e
  executar testes

- Tarefa 3: Configurar o Freshworks para gerenciamento de tíquetes

- Tarefa 4: Conectar agentes a uma API externa (integração MCP com
  Freshdesk)

## Tarefa 1: Construir a ferramenta MCP do Azure Search

Nesta tarefa, você irá implementar a atualização das variáveis de
ambiente com as credenciais do AI Search, criar uma classe de ferramenta
assíncrona que consulta o Azure AI Search e retorna os principais
trechos de documentos (top-N), que os agentes utilizarão como contexto.

O MCP é um padrão que permite que agentes de AI acessem de forma segura
conhecimentos externos e ferramentas por meio de contratos estruturados
de entrada e saída. Ele permite que agentes recuperem informações
factuais, invoquem APIs e executem ações de maneira controlada e
auditável, formando a base de sistemas corporativos de AI fundamentados
e extensíveis.

1.  No Portal do Azure, navegue até **agenticai** e, na lista de
    recursos, selecione o serviço de Search **ai-knowledge**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

2.  Selecione **Keys (1)** no menu esquerdo, em Setting, e copie a
    **Query Key (2)** usando a opção de cópia conforme mostrado.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

3.  Após copiar, cole a chave com segurança em um bloco de notas. Em
    seguida, selecione **Indexes (1)** no menu esquerdo, em Search
    Management, e copie o **Index Name (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

4.  Como você já implementou anteriormente um sistema multiagente, no
    painel do Visual Studio Code, selecione o arquivo .env, pois será
    necessário adicionar as chaves do AI Search para a conexão.

5.  No arquivo .env, adicione a seção abaixo logo abaixo das chaves do
    AI Foundry. Substitua os valores **\[Query_Key\]** e
    **\[Index_Name\]** pelos valores que você copiou anteriormente.

> AZURE_SEARCH_ENDPOINT=https://ai-knowledge--@lab.LabInstance.Id.search.windows.net/
>
> AZURE_SEARCH_API_KEY=\[Query_Key\]
>
> AZURE_SEARCH_INDEX=\[Index_Name\]

6.  Após concluir, salve o arquivo. Clique na opção **File (1)** no menu
    superior e selecione **Save (2)** para salvar o arquivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image4.png)

7.  Em seguida, clique na opção **Create Folder** conforme mostrado.
    Quando solicitado, informe o nome da pasta como tools. Crie essa
    nova pasta na raiz do projeto.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

8.  Após a criação, a estrutura de pastas ficará semelhante à exibida.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

9.  Agora, selecione a pasta **tools (1)** criada anteriormente e clique
    na opção **Create file (2)** conforme mostrado. Isso criará um
    arquivo dentro da pasta tools.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

10. Informe o nome do arquivo como azure_search_tool.py.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

11. Após a criação do arquivo, adicione o seguinte trecho de código para
    implementar a configuração de uma ferramenta de search para o seu
    agente.

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

> **Propósito do AzureSearchTool:**

- Esta ferramenta fornece uma interface assíncrona compatível com MCP
  para que agentes consultem o índice do Azure Cognitive Search e
  recuperem trechos de contexto factual relevantes para as consultas do
  usuário.

> **Configuração baseada em ambiente:**

- A ferramenta lê as credenciais do Azure Search — endpoint, chave de
  API e nome do índice — diretamente das variáveis de ambiente,
  garantindo uma configuração segura e flexível.

> **Funcionalidade principal de pesquisa (método search):**

- O método search() envia uma solicitação POST assíncrona para a API
  REST do Azure Search, buscando os documentos com melhor
  correspondência e concatenando seus campos de conteúdo em uma única
  string contextual.

> **Modo de teste e diagnóstico:**

- O arquivo inclui uma rotina main() integrada que carrega variáveis de
  ambiente, executa uma verificação de integridade em tempo real e
  executa consultas de exemplo para validação rápida e independente
  antes da integração com o agente.

12. Após concluir, selecione **File (1)** e clique em **Save (2)** para
    salvar o arquivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image10.png)

13. Selecione a opção **... (1)** no menu superior para expandir o menu.
    Em seguida, selecione **Terminal (2)** e clique em **New Terminal
    (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

14. Execute o comando abaixo para testar o funcionamento da ferramenta
    de busca.

+++python .\tools\azure_search_tool.py+++

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image12.png)

15. Você implementou com sucesso uma ferramenta MCP que conecta seus
    dados do Azure AI Search ao agente, permitindo que ele recupere
    contexto relevante a partir da base de conhecimento indexada.

## Tarefa 2: Anexar a ferramenta aos agentes, enriquecer prompts e executar testes

Nesta tarefa, você anexará o AzureSearchTool aos agentes de
RH/Finanças/Conformidade, atualizará a orquestração para buscar o
contexto antes de chamar os agentes e executará testes em modo batch e
interativo.

1.  Como você já implementou a ferramenta, agora é necessário alterar a
    orquestração do agente para acionar a ferramenta de busca antes de
    fornecer uma resposta.

2.  No explorador do Visual Studio Code, abra o arquivo **main.py** e
    substitua todo o conteúdo existente pelo código fornecido abaixo.

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

> **Objetivo do script principal atualizado:**

- Esta versão estende o orquestrador anterior para integrar o Azure AI
  Search via MCP, permitindo que as respostas de cada agente sejam
  fundamentadas em dados corporativos, em vez de raciocínio genérico de
  LLM.

> **Integração da ferramenta de busca (novo recurso):**

- A instância AzureSearchTool é inicializada e anexada ao dicionário de
  agentes como agents\["search_tool"\].

- Antes da execução do sistema, é realizada uma verificação de
  integridade para confirmar a conectividade com o Azure AI Search e a
  prontidão do índice.

> **Lógica de enriquecimento de contexto (Enhanced run_multi_agent):**

- Para cada consulta, o sistema agora recupera trechos de texto
  relacionados do Azure Search (context = await
  agents\["search_tool"\].search(query)).

- O contexto retornado é incorporado diretamente ao prompt antes de ser
  enviado ao agente especializado.

- O LLM é explicitamente instruído a basear suas respostas
  principalmente nesses dados contextuais, garantindo respostas
  fundamentadas em fatos.

3.  Após concluir, selecione **File (1)** e clique em **Save (2)** para
    salvar o arquivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image10.png)

4.  Selecione a opção **... (1)** no menu superior para expandir o menu.
    Em seguida, selecione **Terminal (2)** e clique em **New Terminal
    (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

5.  Após o terminal ser aberto, execute o comando abaixo para iniciar o
    agente e revisar as respostas para os prompts de teste fornecidos no
    arquivo de código.

+++python main.py+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image14.png)

Verifique o parâmetro **Context retrieved** e analise como o agente está
obtendo o contexto a partir dos dados fundamentados.

6.  Agora, execute o agente novamente em modo interativo adicionando o
    parâmetro --interactive. Isso permite inserir a pergunta manualmente
    e receber a resposta. Quando solicitado, utilize o prompt abaixo.

+++python main.py –interactive+++

+++Is employee data protected under GDPR?+++

7.  Após receber a resposta, no próximo prompt, digite q para sair ou
    interromper a execução do agente.

![A black screen with white text AI-generated content may be
incorrect.](./media/image15.png)

## Tarefa 3: Configurando o Freshworks para gerenciamento de tíquetes

Nesta tarefa, você irá implementar a configuração do Freshworks para
habilitar o gerenciamento de tíquetes e a integração corporativa com o
seu sistema multiagente.

O **Freshworks** é uma plataforma baseada em nuvem para atendimento ao
cliente e engajamento, projetada para melhorar as operações de suporte e
aumentar a satisfação do usuário. Ela oferece um conjunto de ferramentas
para gerenciamento de tíquetes, chat ao vivo, criação de central de
ajuda e autoatendimento. O Freshworks oferece suporte à comunicação
omnichannel, permitindo que as empresas gerenciem interações com
clientes por e-mail, chat, telefone e redes sociais a partir de uma
interface centralizada. Seus recursos de automação ajudam a otimizar
fluxos de trabalho, atribuir tíquetes e fornecer análises para
acompanhamento de desempenho. Agora, você irá implementar a configuração
da conta do Freshworks.

1.  Copie a URL fornecida e cole-a em uma nova aba do navegador dentro
    da VM para abrir o portal do **Freshworks**.

    - URL:

+++https://www.freshworks.com/freshdesk/lp/home/?tactic_id=3387224&utm_source=google-adwords&utm_medium=FD-Search-Brand-India&utm_campaign=FD-Search-Brand-India&utm_term=freshdesk&device=c&matchtype=e&network=g&gclid=EAIaIQobChMIuOK90qvLjQMV_dQWBR3JAi9VEAAYASAAEgK87_D_BwE&audience=kwd-30002131023&ad_id=282519464145&gad_source=1&gad_campaignid=671502402+++

2.  No portal, selecione **Start free trial** para iniciar o período de
    avaliação gratuita.

![](./media/image16.png)

3.  No próximo painel, forneça os seguintes detalhes e clique em **Try
    it free (6)**:

    - **First name:** +++LODS+++

    - **Last name:** +++User1+++

    - **Work
      email:** **+++@lab.CloudPortalCredential(User1).Username+++**

    - **Company name:** Zava

    - **Organization size:** Selecione **1-10**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image17.png)

4.  No próximo painel, forneça os seguintes detalhes e clique em **Next
    (4)**:

    - **What industry are you from?: na lista, selecione Software and
      internet (1)**

    - **How many employees are there in your company?:** selecione
      **1–10 (2)**

    - selecione **I'm trying customer service software for the first
      time (3)**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.png)

5.  Após concluir, copie a URL fornecida abaixo e cole-a em uma nova aba
    do navegador dentro da VM para abrir o **Outlook**.

    - URL:
      +++https://go.microsoft.com/fwlink/p/?LinkID=2125442&clcid=0x409&culture=en-us&country=us+++

6.  No painel Pick an account, selecione a conta que foi atribuída a
    você para este laboratório.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

7.  No e-mail de verificação do Freshworks, abra a mensagem e clique em
    **Activate Account**.

> ![A screenshot of a computer screen AI-generated content may be
> incorrect.](./media/image20.png)
>
> **Observação:** Se você não conseguir localizar o e-mail de ativação
> do Freshworks, aguarde alguns minutos, pois pode haver um atraso na
> entrega do e-mail. Caso o e-mail não chegue após algum tempo,
> considere reiniciar as etapas para ativar o período de avaliação
> gratuita em uma nova janela privada/incógnita. Além disso, verifique a
> pasta de spam ou lixo eletrônico, pois o e-mail pode ter sido
> filtrado.

8.  No próximo painel, insira uma senha em **Enter password (1)** e
    forneça a mesma senha em **Confirm password (2)**. Em seguida,
    clique em **Activate your account (3)**.

> ![A screenshot of a login screen AI-generated content may be
> incorrect.](./media/image21.png)

9.  Após acessar o portal, clique no ícone **Profile (1)** no canto
    superior direito e selecione **Profile settings (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

10. Na página de perfil, clique em **View API Key** para obter as chaves
    de API.

![A screenshot of a web page AI-generated content may be
incorrect.](./media/image23.png)

**Observação:** Se você não conseguir localizar essa opção, reduza o
tamanho da tela usando **CTRL + -**.

11. No próximo painel, preencha o **CAPTCHA.**

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image24.png)

12. Copie a API Key para um bloco de notas, pois você irá utilizá-la
    posteriormente.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

13. Na aba do navegador, copie a **Account URL** conforme exibido e
    salve o valor no bloco de notas. Você também utilizará essa
    informação posteriormente.

![](./media/image26.png)

14. No menu à esquerda, clique no ícone **Tickets** e observe que alguns
    tíquetes padrão já estão disponíveis.

![A screenshot of a social media post AI-generated content may be
incorrect.](./media/image27.png)

15. Após concluir, volte ao painel do Visual Studio Code e abra o
    arquivo .env.

16. No arquivo .env, adicione o conteúdo abaixo e insira a API Key e a
    Domain URL que você copiou anteriormente.

> \# Freshdesk Configuration
>
> FRESHDESK_DOMAIN=\[Domain_URL\]
>
> FRESHDESK_API_KEY=\[API_Key\]

![A black and white text with red lines AI-generated content may be
incorrect.](./media/image28.png)

![A computer screen shot of a program AI-generated content may be
incorrect.](./media/image29.png)

17. Após concluir, selecione **File (1)** e clique em **Save (2)** para
    salvar o arquivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image10.png)

## Tarefa 4: Conectar agentes a uma API externa (integração MCP com o Freshdesk)

Nesta tarefa, você irá implementar uma ferramenta MCP adicional que
conecta seus agentes a uma instância externa do Freshdesk por meio da
API REST. Essa ferramenta permitirá que os agentes, especialmente
Finanças e RH, criem tíquetes reais quando os usuários solicitarem ações
como reembolsos, aprovações de viagem ou esclarecimentos de políticas.

1.  Após as variáveis de ambiente estarem configuradas, é hora de
    implementar a ferramenta que realiza a integração com o Freshdesk.

2.  No menu do explorador, selecione a pasta **tools (1)** e clique na
    opção **Create File (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.png)

3.  Informe o nome do arquivo como freshdesk_tool.py.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

4.  Agora, selecione o arquivo e adicione o seguinte trecho de código
    para configurar a ferramenta.

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

> **Propósito do FreshdeskToo:**

- Esta classe fornece uma interface assíncrona para integrar a API REST
  do Freshdesk ao fluxo de trabalho do agente, permitindo a criação
  automatizada de tíquetes diretamente a partir das ações do agente.

> **Autenticação e configuração:**

- A autenticação é realizada usando a API Key do Freshdesk via Basic
  Auth e as requisições são construídas para o endpoint /api/v2/tickets.

- A URL base, a API Key e as configurações padrão (prioridade, ID do
  grupo) são lidas de forma segura a partir de variáveis de ambiente.

> **Lógica de criação de tíquetes:**

- O método create_ticket() cria uma carga JSON estruturada, incluindo
  assunto, descrição, informações do solicitante e tags opcionais.

- Ele realiza uma requisição POST assíncrona ao Freshdesk, criando um
  tíquete real e retornando metadados normalizados (ID, status,
  prioridade, URL).

5.  Após concluir, selecione **File (1)** e clique em **Save (2)** para
    salvar o arquivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image10.png)

6.  No menu do explorador, selecione o arquivo main.py.

7.  Substitua todo o código existente pelo trecho de código fornecido a
    seguir.

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

- Esta função amplia a lógica multiagente original para lidar com
  interações personalizadas do usuário e criação automatizada de
  tíquetes.

- Ela detecta um bloco CREATE_TICKET na resposta do agente, extrai
  detalhes (assunto, corpo, tags, informações do solicitante) e aciona
  automaticamente a criação de um tíquete no Freshdesk.

- Após o tíquete ser criado, o bloco é substituído dinamicamente por uma
  mensagem de confirmação exibindo o ID do tíquete, o assunto e a URL.

> **interactive_ticket_creation():**

- Introduz um fluxo guiado de criação manual de tíquetes, no qual o
  usuário pode inserir diretamente o assunto e a descrição do tíquete
  por meio de prompts.

- Em seguida, a ferramenta do Freshdesk é chamada de forma assíncrona
  para criar o tíquete e exibir interativamente, no terminal, os
  detalhes de confirmação.

8.  Após concluir, selecione **File (1)** e clique em **Save (2)** para
    salvar o arquivo.

![](./media/image10.png)

9.  Como o orquestrador principal já está configurado, é necessário
    adicionar instruções a cada agente para a criação de tíquetes.

10. No painel esquerdo, selecione **finance_agent.py** e, no arquivo,
    localize a seção Instructions, que define as instruções do agente.

11. Adicione as seguintes instruções logo abaixo das instruções
    existentes.

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

12. Após concluir, salve o arquivo.

13. No painel esquerdo, selecione **hr_agent.py** e, no arquivo,
    localize a seção Instructions, que fornece as instruções do agente.

14. Adicione as seguintes instruções logo abaixo das instruções
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

15. Após concluir, selecione **File (1)** e clique em **Save (2)** para
    salvar o arquivo.

![](./media/image10.png)

16. Selecione a opção **... (1)** no menu superior para expandir o menu.
    Em seguida, selecione **Terminal (2)** e clique em **New Terminal
    (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

17. Após o terminal ser aberto, execute o comando abaixo para iniciar o
    agente e revisar as respostas e a criação de tíquetes.

+++python main.py+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.png)

18. Navegue até o portal do Freshdesk, selecione **Tickets** no menu à
    esquerda e verifique o novo tíquete criado.

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image37.png)

**Observação:** Se você não conseguir visualizar o tíquete, aguarde
alguns segundos ou atualize a página.

19. Em seguida, execute o agente novamente em modo interativo
    adicionando o parâmetro --interactive. Isso permite inserir a
    pergunta manualmente e receber a resposta. Quando solicitado,
    utilize o prompt abaixo como pergunta:

Preciso criar um tíquete para o meu pedido de reembolso de viagem.
Viajei para Nova Iorque de 15 a 18 de outubro de 2025 para a conferência
anual de clientes. O objetivo era apresentar nossa nova linha de
produtos às principais partes interessadas e participar de sessões de
planejamento estratégico. Minhas despesas incluem voos (US$ 650),
hospedagem em hotel (US$ 480 por 3 noites), refeições (US$ 320),
transporte local (US$ 85) e materiais da conferência (US$ 45). O valor
total do reembolso é de US$ 1.580. Tenho todos os recibos prontos para
envio.

20. Navegue novamente até o Freshworks, verifique o novo tíquete criado
    e analise como o agente adicionou os detalhes com base nas
    informações fornecidas.

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image38.png)

![A screenshot of a travel reimbursement AI-generated content may be
incorrect.](./media/image39.png)

21. Depois de obter a resposta, no próximo prompt, adicione q para sair
    do agente ou interromper o agente.

![A black screen with white text AI-generated content may be
incorrect.](./media/image15.png)

22. Você criou com sucesso um sistema multiagente que recupera
    conhecimento contextual do Azure AI Search e se integra
    perfeitamente ao Freshdesk para gerenciamento de tíquetes
    corporativos.

**Resumo**

Neste laboratório, você testou com sucesso o sistema multiagente
completo, no qual os agentes recuperaram informações relevantes do Azure
AI Search, identificaram o departamento apropriado para lidar com cada
consulta e criaram tíquetes no Freshdesk quando necessário. Isso
demonstrou como os agentes podem combinar raciocínio contextual com
ações do mundo real, tornando-os mais eficientes e práticos para fluxos
de trabalho empresariais.

Você concluiu este laboratório com sucesso. Clique em Next \>\> para
continuar.
