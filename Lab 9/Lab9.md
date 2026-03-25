# Laboratorio 9: Implementación de flujos de trabajo de agentes individuales y multiagentes usando Azure AI Framework

**Duración estimada:** 45 minutos

**Descripción general**

Usted es un **AI Engineer** en **Contoso Ltd.**, encargado de
desarrollar **flujos de trabajo inteligentes de agentes** utilizando el
**Azure AI Framework**.  
En este laboratorio, creará un **sistema de agente individual** que se
integra con herramientas externas mediante **MCP**, y luego diseñará
**flujos de trabajo multiagente** donde múltiples agentes especializados
colaboran o transfieren tareas dinámicamente según la **intención del
usuario**.

**Objetivos del laboratorio**

En este laboratorio, realizará las siguientes tareas:

• Tarea 1: Crear y probar un Azure OpenAI Chat Agent.

• Tarea 2: Crear un flujo de trabajo de agente individual con
integración de herramientas.

• Tarea 3: Diseño de flujos de trabajo multiagente.

- Tarea 3.1: Orquestación de flujos de trabajo multiagente.

- Tarea 3.2: Sistema multiagente con patrón de handoff.

## Tarea 0: Configuración del entorno del laboratorio

1.  Desde C:\Labfiles\Day 2, extraiga el archivo
    **OpenAIWorkshop-Framework**.

2.  Haga clic en **Visual Studio Code** desde el escritorio de la LabVM.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

3.  Seleccione **File (1)** y haga clic en **Open Folder (2)** para
    abrir la carpeta **OpenAIWorkshop-Framework**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

4.  Navegue a C:\Labfiles\Day 2\OpenAIWorkshop-Framework, seleccione
    **OpenAIWorkshop-Framework** y luego **Select Folder**.

5.  Seleccione **Yes, I trust the authors**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

6.  Haga clic en el **ellipsis (…) (1)**, luego en **Terminal (2)** y
    después en **New Terminal (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

7.  Ingrese los siguientes comandos para navegar al directorio de
    **aplicaciones** e instalar las dependencias requeridas desde
    **requirements.txt**:

> cd agentic_ai/applications
>
> pip install -r requirements.txt

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image5.png)

8.  El comando puede tardar entre 5 y 10 minutos en completarse.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image6.png)

## Tarea 1: Crear y probar un Azure OpenAI Chat Agent

En esta tarea, construirá y probará un agente simple de Azure OpenAI en
Visual Studio Code.  
Configurará variables de entorno, conectará el agente con su modelo
implementado y observará cómo genera respuestas dinámicas según
distintos prompts.

1.  Regrese a **Visual Studio Code**.

2.  Asegúrese de que el comando **pip install -r requirements.txt** haya
    terminado correctamente. Si aún está ejecutándose, espere.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image6.png)

3.  Desde el Explorer, expanda **agentic_ai (1) \> applications (2)**.
    Haga clic derecho sobre **.env.sample (3)** y seleccione **Rename
    (4)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

4.  Renómbrelo como **.env** y ábralo.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

5.  Reemplace los valores de **AZURE_OPENAI_API_KEY (1)** y
    **AZURE_OPENAI_ENDPOINT (2)** con los valores reales obtenidos desde
    la página **Microsoft Foundry Overview**.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image9.png)

6.  Agregue **AZURE_OPENAI_CHAT_DEPLOYMENT** como **gpt-4o-mini (3)**.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image10.png)

7.  Seleccione **File (1)** y luego **Save (2)**.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

8.  Haga clic derecho sobre la carpeta **application (1)** y seleccione
    **New file (2)** para crear un archivo nuevo.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.png)

9.  Nombre el archivo como:  
    +++simple_agent_test.py+++.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

10. Copie y pegue el siguiente código en el archivo (no se traduce, se
    mantiene intacto):

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

11. Seleccione **File (1)** y luego **Save (2)**.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

12. Haga clic derecho sobre **simple_agent_test.py (1)** y seleccione
    **Open in Integrated Terminal (2)**.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image15.png)

13. Pegue el siguiente comando para ejecutar el agente y observar su
    resultado:

+++python simple_agent_test.py+++

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image16.png)

14. Modifiquemos la instrucción para observar cómo responde el agente.
    Proporcione la instrucción como **Tell me a joke about the Earth
    (1)** (línea n.º 31), luego guarde el archivo. Después, ejecute el
    comando siguiente **(2)** y revise la respuesta del agente **(3)**.

+++python simple_agent_test.py+++

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image17.png)

15. Esto demuestra cómo la respuesta del agente varía dependiendo de la
    instrucción, mostrando su capacidad de adaptarse a distintos
    prompts.

## Tarea 2: Crear un flujo de trabajo de agente individual con integración MCP

En esta tarea, creará y probará un flujo de trabajo de agente individual
que se integra con herramientas externas mediante el **MCP (Model
Context Protocol)**.  
Configurará variables de entorno, ejecutará el servidor MCP, backend y
frontend, y observará cómo el agente utiliza herramientas MCP para
procesar consultas y generar respuestas contextualizadas.

1.  En Visual Studio Code, expanda **agents (1) \> agent_framework (2)
    \> single_agent (3)** y revise el código del flujo de trabajo de
    agente individual con la herramienta **MCPStreamableHTTPTool (4)**.

    - MCPStreamableHTTPTool permite que el agente invoque servicios
      externos basados en HTTP mediante el servidor MCP.

    - Se pasa al ChatAgent y se usa automáticamente según instrucciones
      o prompts.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image18.png)

2.  Revise cómo está integrada en el código:

    - En el método **\_maybe_create_tools**:

> ![A screen shot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

- Crea una herramienta HTTP “streamable” conectada al servidor MCP.

- Permite al agente hacer llamadas HTTP a servicios externos.

&nbsp;

- La herramienta se pasa al **ChatAgent** durante la inicialización:

> ![A screen shot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

- El agente puede utilizarla cuando el prompt activa una llamada a una
  herramienta.

- Soporta streaming vía WebSocket: cuando se invoca la herramienta en
  una conversación con streaming, se transmite el nombre de la
  herramienta y el turno mediante: \_chat_async_streaming.

3.  Navegue al archivo **.env (1)** y agregue la siguiente variable para
    especificar que se ejecutará el flujo de trabajo de agente
    **individual (2)**:

+++AGENT_MODULE=agents.agent_framework.single_agent+++

- Agregue también la variable **DISABLE_AUTH=true (3)** para
  deshabilitar autenticación durante desarrollo local:

> +++DISABLE_AUTH=true+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

4.  Seleccione **File (1)** y luego **Save (2)**.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

5.  Ahora iniciará el servidor **MCP, el backend y el frontend** en
    React para correr el entorno completo de agentes localmente.

6.  En Visual Studio Code, haga clic en **ellipsis (...) (1)**, luego
    **Terminal (2)** y **New Terminal (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

7.  Espere a que termine lo anterior y proceda con el siguiente paso.

8.  **Iniciar el MCP Server (Terminal 1):** (el directorio *mcp* está en
    el nivel raíz del proyecto)

- Ejecute el siguiente comando para iniciar el servidor MCP, el cual
  expone APIs que los agentes pueden invocar como herramientas. (El
  servidor se ejecuta en <http://localhost:8000>)

> cd mcp
>
> uv run python mcp_service.py
>
> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image21.png)
>
> **Nota:** Si encuentra algún error, por favor ejecute los siguientes
> comandos:

+++pip install uv+++

+++uv run python mcp_service.py+++

9.  Deje el comando ejecutándose, abra una nueva terminal.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

10. **Iniciar el Backend (Terminal 2)**:

    - Ejecute el siguiente comando para iniciar el servidor backend que
      aloja sus flujos de trabajo de agentes, la gestión de sesiones y
      los endpoints de API.

> cd agentic_ai/applications
>
> uv run python backend.py

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image22.png)

- Se ejecuta localmente
  en: [http://localhost:7000](http://localhost:7000/).

- Esta es la lógica principal de la aplicación con la que el frontend se
  comunicará. Asegúrese de que la conexión esté activa.

> ![A screen shot of a computer AI-generated content may be
> incorrect.](./media/image23.png)

11. Deje el comando ejecutándose, abra una nueva terminal.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

12. **Iniciar el React Frontend (Terminal 3)**:

    - Ingrese el siguiente comando para navegar al directorio
      react-frontend:

> +++cd agentic_ai/applications/react-frontend+++

- Luego, ejecute el siguiente comando para iniciar la **interfaz React**
  para su UI de agentes, la cual proporciona una interfaz para
  interactuar con los agentes y ver sus respuestas en tiempo real:

> +++npm start+++

- La compilación puede tardar algún tiempo. Ignore las advertencias y
  espere hasta que finalice. Una vez que el **webpack haya compilado
  correctamente**, la aplicación del agente se ejecutará localmente
  en: [http://localhost:3000](http://localhost:3000/).

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image24.png)

13. Después de que los tres terminales estén en ejecución, la aplicación
    del agente se iniciará en su navegador, donde podrá interactuar con
    el agente y probar sus capacidades.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

**Nota:** Asegúrese de que los tres terminales estén ejecutándose. Si
alguno se detiene, vuelva a ejecutar el comando correspondiente. Si los
tres no están activos, podría encontrar un error de conexión.

14. Envíe el siguiente prompt en el chat **(1)** y vea la respuesta
    **(2)**:

+++Customer 251, what's my billing summary?+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

**Nota**: Asegúrese de que los tres terminales estén ejecutándose. Si
alguno se detiene, vuelva a ejecutar el comando correspondiente. Si los
tres no están activos, podría encontrar un error de conexión.

15. Verifique el resultado. Fue el **ChatAgent (self.\_agent)** quien
    interpretó el prompt, posiblemente llamó a la herramienta **MCP** y
    generó el resultado.

    - El agente interpretó su solicitud como una consulta de facturación
      para el **Customer 251**.

    - Utilizó la **herramienta MCP** para obtener datos estructurados de
      facturación.

    - El agente está funcionando como se espera: integra dinámicamente
      resultados de herramientas y razonamiento de IA para responder a
      preguntas específicas del usuario.

16. Tras finalizar sus pruebas, vuelva a VS Code y termine todas las
    sesiones de terminal en ejecución. Esto garantiza que el próximo
    flujo de trabajo multiagente se ejecute sin interferencias.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

## Tarea 3: Diseño de flujo de trabajo multiagente

En esta tarea diseñará e implementará flujos de trabajo avanzados
multiagente que demuestran distintos patrones de coordinación. Comenzará
orquestando múltiples agentes especializados mediante un gestor central
para manejar consultas complejas de forma colaborativa, y luego
explorará un sistema basado en handoff donde el control cambia
dinámicamente entre agentes específicos de dominio según la intención
del usuario.

### Tarea 3.1: Orquestación de flujos de trabajo multiagente

En esta tarea orquestará un flujo de trabajo multiagente donde un
orquestador central coordina múltiples agentes especializados para
procesar colaborativamente consultas complejas del usuario y generar
respuestas precisas basadas en herramientas.

1.  Vaya a **agent (1) \> agent_framework (2) \> multi_agent (3) \>
    magentic_group (4)** y vea el código **(5).**

    - Este código representa un **framework de orquestación
      multiagente** porque define un sistema donde múltiples agentes
      especializados colaboran bajo la guía de un orquestador central.

> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image28.png)

- \_create_participants inicializa múltiples agentes especialistas
  (CRM/Billing, Product/Promotions, Security/Authentication).

- Cada agente:

  - Tiene un dominio y conjunto de herramientas específico.

  - Solo se comunica con el orquestador, no directamente con el usuario.

  - Proporciona respuestas fácticas basadas en herramientas.

- Estos son los agentes utilizados en este flujo de trabajo multiagente:

  - **CRM & Billing Agent –** Gestiona cuentas de clientes,
    suscripciones, facturación, invoices, pagos y consultas relacionadas
    mediante datos fácticos basados en herramientas.

  - **Product & Promotions Agent –** Proporciona disponibilidad de
    productos, promociones, descuentos, elegibilidad y términos usando
    fuentes estructuradas.

  - **Security & Authentication Agent –** Gestiona incidentes de
    seguridad, problemas de autenticación, bloqueos de cuenta y
    orientación de mitigación de riesgos usando logs y herramientas.

2.  Vaya al archivo **.env (1)**, comente la variable del agente
    individual **(2)** e ingrese el siguiente comando para agregar la
    variable de **Orchestrating Multi-Agent (3)**.

+++AGENT_MODULE=agents.agent_framework.multi_agent.magentic_group+++

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image29.png)

3.  Seleccione **File (1)** y luego **Save (2)**.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

4.  Ahora lance la aplicación completa del agente iniciando sus tres
    componentes principales siguiendo los pasos:

5.  En la ventana de **Visual Studio Code**, haga clic en los puntos
    suspensivos (...) **(1)**, luego en la **Terminal (2)** y finalmente
    **New Terminal (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

6.  **Inicie el MCP Server (Terminal 1):** (la carpeta *mcp* está en el
    nivel raíz del proyecto)

    - Ejecute el siguiente comando para iniciar el **servidor MCP**, el
      cual expone las APIs que los agentes pueden utilizar como
      herramientas. (El servidor se ejecuta
      en [http://localhost:8000](http://localhost:8000/))

> cd mcp
>
> uv run python mcp_service.py

7.  Deje el comando ejecutándose, abra una nueva terminal.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

8.  **Inicie el Backend (Terminal 2)**:

    - Ejecute el siguiente comando para iniciar el backend que aloja sus
      flujos de trabajo de agentes, la gestión de sesiones y los
      endpoints de API.

> cd agentic_ai/applications
>
> uv run python backend.py

- Esta es la lógica central de la aplicación con la que se comunicará el
  frontend. Asegúrese de que la **conexión esté abierta**.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image23.png)

9.  Deje el comando ejecutándose, abra una nueva terminal.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

10. **Inicie el React Frontend (Terminal 3):**

    - Ingrese el siguiente comando para navegar al directorio
      react-frontend.

> +++cd agentic_ai/applications/react-frontend+++

- Ingrese el siguiente comando para iniciar el **frontend React** para
  su UI del agente. Esto proporciona una interfaz para interactuar con
  los agentes y ver sus respuestas en tiempo real.

> +++npm start+++

- Una vez que **webpack compile exitosamente**, la aplicación del agente
  se ejecutará localmente
  en: [http://localhost:3000](http://localhost:3000/).

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

11. Envíe el siguiente prompt en el chat y vea la respuesta en el panel
    izquierdo:

+++Customer 251, what's my billing summary?+++

12. El orquestador actúa como un gestor o router. Lee la consulta del
    usuario y decide qué agente especializado debe manejarla. Utiliza el
    contexto y palabras clave (como “billing”, “promotion”, “login”)
    para tomar esta decisión.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.png)

13. El orquestador asigna la tarea a un agente de dominio. Envia la
    consulta a uno de estos agentes internos:

    - crm_billing – billing, invoices, payments

    - product_promotions – products, discounts, offers

    - security_authentication – security, login, account lockouts

14. Para su consulta (“billing summary”), el orquestador la enruta a
    **crm_billing**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

- El agente de dominio usa herramientas conectadas. Cada agente tiene
  acceso a herramientas específicas (APIs) via el MCP server.

- Ejemplo: **crm_billing** puede llamar get_customer_detail,
  get_billing_summary, get_invoice_payment, etc.

- El agente llama la herramienta correcta, obtiene datos estructurados y
  forma una respuesta fáctica.

15. Tras completar sus pruebas, vuelva a VS Code y termine todas las
    sesiones de terminal en ejecución. Esto garantiza que el siguiente
    flujo de trabajo multiagente se ejecute sin interferencias.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

### Tarea 3.2: Sistema multiagente con patrón Handoff

En esta tarea explorará un sistema multiagente basado en handoff, donde
las conversaciones se transfieren automáticamente entre agentes
especializados (como Billing, Promotions o Security) según la intención
del usuario, garantizando interacciones fluidas y conscientes del
contexto entre dominios.

- **¿Cómo funciona?**

  - El usuario interactúa directamente con un agente de dominio, por
    ejemplo, el CRM & Billing agent.

  - Un intent classifier verifica si el nuevo mensaje del usuario
    pertenece a otro dominio (como promotions o security).

  - Si es así, el sistema transfiere automáticamente (“handoff”) la
    conversación al agente especialista adecuado.

  - Cada agente tiene herramientas filtradas relevantes a su dominio
    (billing, promotions o security).

  - El handoff ocurre de forma fluida, con transferencia de contexto
    para que el nuevo agente comprenda el historial de la conversación.

1.  Expanda **agents (1) \> agent_framework (2) \> multi_agent (3) \>
    handoff_multi_domain_agent (4)** y vea el código (5).

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image32.png)

2.  Vaya al archivo **.env (1)**, comente la variable de Orchestrating
    Multi-Agent **(2)** e ingrese el siguiente comando para agregar la
    variable del sistema **Handoff Multi-Agent (3)**:

+++AGENT_MODULE=agents.agent_framework.multi_agent.handoff_multi_domain_agent+++

- Ingrese el siguiente comando para controlar cuánta conversación previa
  se transfiere durante un handoff. **-1** indica que se transfieren
  todos los turnos previos **(4)**.

> +++HANDOFF_CONTEXT_TRANSFER_TURNS=-1+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

3.  Seleccione **File (1)** y luego **Save (2)**.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

4.  Ahora lance la aplicación completa del agente iniciando sus tres
    componentes principales siguiendo los pasos:

5.  En la ventana de Visual Studio Code, haga clic en **(...) (1)**,
    luego **Terminal (2)** y luego **New Terminal (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

6.  **Inicie el MCP Server (Terminal 1)**: (El directorio **mcp** se
    encuentra en la raíz del proyecto)  
    Ejecute el siguiente comando para iniciar el servidor **MCP**, el
    cual expone las APIs que los agentes pueden utilizar como
    herramientas. (El servidor se ejecuta en
    [http://localhost:8000](http://localhost:8000/))

> cd mcp
>
> uv run python mcp_service.py

7.  Deje el comando ejecutándose, abra una nueva terminal.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

8.  **Inicie el Backend (Terminal 2)**:

    - Ejecute el siguiente comando para iniciar el backend server que
      aloja sus flujos de trabajo de agentes, la gestión de sesiones y
      los API endpoints.

> cd agentic_ai/applications
>
> uv run python backend.py

- Esta es la lógica central de la aplicación con la que se comunicará el
  frontend. Asegúrese de que la **conección esté abierta**.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image23.png)

9.  Deje el comando ejecutándose, abra un nuevo terminal.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

10. **Inicie el React Frontend (Terminal 3)**:

    - Ingrese el comando indicado a continuación para navegar al
      directorio react-frontend.

> +++cd agentic_ai/applications/react-frontend+++

- Ingrese el siguiente comando para iniciar el **React frontend** para
  su interfaz de agente. Proporciona una interfaz de usuario para
  interactuar con los agentes y ver sus respuestas en tiempo real.

> +++npm start+++

- Una vez que **webpack compile correctamente**, la aplicación del
  agente se ejecutará localmente
  en: [http://localhost:3000](http://localhost:3000/).

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

11. Envíe el siguiente prompt en el chat y vea la respuesta en el panel
    izquierdo:

+++Customer 251, what's my billing summary?+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image34.png)

- Aquí, el Intent classifier enruta al dominio **crm_billing**.

- La herramienta **get_billing_summary** se llama para el customer 251.

12. Puede proporcionar la siguiente consulta para continuar con respecto
    a billing:

+++Yes, I would like to view the invoice details+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image35.png)

**Note**: Si obtiene una respuesta como *I was unable to retrieve the
invoice details because the referenced numbers are invoice IDs, not
subscription IDs...*, proporcione el siguiente prompt:

+++Yes, I would like to view the invoice details for customer 251+++

13. Ahora intentemos una consulta relacionada con otro dominio para
    probar cómo funciona el handoff.

14. Ingrese la siguiente consulta relacionada con Product & Promotions y
    vea la respuesta:

+++Are there any promotions available for my subscription plan+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image36.png)

- Dado que la conversación previa fue manejada por el **CRM & Billing
  Specialist**, el sistema detecta un cambio de dominio. Decide realizar
  un handoff de la conversación al **Product & Promotions Specialist**.

- El sistema transfiere opcionalmente el contexto previo (como qué
  customer se está utilizando) al nuevo agente, dependiendo de la
  configuración **HANDOFF_CONTEXT_TRANSFER_TURNS**.

- El **Product & Promotions Specialist** solo tiene acceso a
  herramientas relevantes a promociones, planes e información del
  producto (por ejemplo: **get_promotions**,
  **get_eligible_promotions**).

15. Tras completar sus pruebas, vuelva a VS Code y termine todas las
    sesiones de terminal en ejecución. Esto garantiza que el próximo
    flujo de trabajo multiagente se ejecute sin interferencias.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

**Resumen**

En este laboratorio creó un flujo de trabajo de agente individual que se
integra con herramientas externas usando MCP y exploró diseños
multiagente donde múltiples agentes especializados colaboran o se
transfieren conversaciones según la intención del usuario. Configuró
variables de entorno, lanzó el entorno completo del agente y probó cómo
los agentes responden de forma inteligente a consultas específicas de
cada dominio.
