# Diseñar agentes de IA escalables con Microsoft Foundry y Agent Framework

**Descripción general**

En este laboratorio práctico, desarrollado a lo largo de 3 días, se
diseñarán y construirán agentes de IA escalables utilizando Microsoft
Foundry y el Microsoft Agent Framework. Los participantes iniciarán
creando su primer agente de IA a través del portal de Microsoft Foundry,
donde aprenderán a cargar documentos de políticas empresariales e
ingerirlos en Azure AI Search para preparar una base de conocimiento
consultable.  
El taller avanza luego hacia la construcción de sistemas multiagente
utilizando el SDK de Microsoft Agent Framework, en los que múltiples
agentes especializados colaboran mediante patrones de comunicación
*Agent-to-Agent* (A2A). Los alumnos ampliarán las capacidades del agente
integrando herramientas externas y fuentes de datos mediante el *Model
Context Protocol* (MCP), conectándose tanto a Azure AI Search para la
recuperación de conocimiento como a API externas, como Freshdesk, para
la gestión de tickets.  
La capacitación progresa hacia la implementación de agentes en el
Microsoft Foundry Agent Service como soluciones persistentes en la nube,
con administración de estado y confiabilidad a nivel empresarial.
Finalmente, los participantes implementarán patrones avanzados de flujo
de trabajo, incluidos sistemas multiagente orquestados con coordinación
centralizada y sistemas basados en transferencia, donde las
conversaciones pasan sin interrupciones entre agentes especializados
según la intención del usuario y el dominio de experiencia.

**Objetivos**

Al finalizar este laboratorio, podrá:

- **Configurar un proyecto de IA y ejecutar chat completion desde VS
  Code:** Configurará un entorno de desarrollo de IA listo para
  producción creando un proyecto de Microsoft Foundry, implementando
  modelos GPT-4 y modelos de embedding, y estableciendo conexiones
  seguras desde Visual Studio Code. Validará la configuración ejecutando
  llamadas de chat completion, garantizando una integración fluida entre
  su entorno local de desarrollo y los servicios de Azure AI con la
  autenticación y configuración del proyecto correctas.

- **Crear un agente de IA analizador de planes de seguros de salud:**
  Desarrollará un agente de IA inteligente especializado en analizar y
  visualizar datos de seguros de salud. Creará un agente que procesa
  información compleja de planes de beneficios de salud y genera
  automáticamente gráficos de barras comparativos, demostrando
  capacidades fundamentales de un agente de IA, incluyendo
  interpretación de datos, comprensión del lenguaje natural, ejecución
  de código y generación automática de visualizaciones para el soporte
  de decisiones.

- **Desarrollar un sistema colaborativo multiagente:** Diseñará e
  implementará una arquitectura multiagente avanzada donde agentes de IA
  especializados trabajan en conjunto para analizar documentos de planes
  de salud y generar reportes completos. Creará un Search Agent para la
  recuperación inteligente de documentos utilizando Azure AI Search, un
  Report Agent para generar reportes analíticos detallados, un
  Validation Agent para garantizar precisión y cumplimiento, y un
  Orchestrator Agent para gestionar la comunicación entre agentes y la
  coordinación del flujo de trabajo, demostrando patrones de
  colaboración entre agentes a nivel empresarial.

**Requisitos previos**

Los participantes deben tener:

**• Experiencia con Azure y la nube:** Familiaridad con Azure Portal,
grupos de recursos y servicios de Azure AI.  
**• Habilidades de programación:** Conocimientos básicos de Python
(async/await, variables de entorno, llamadas API).**  
• Conceptos de IA:** Comprensión de LLMs, embeddings, RAG y prompt
engineering.**  
• Herramientas de desarrollo:** Dominio de Visual Studio Code, uso de
terminal y Git.**  
• Conocimiento del Agent Framework:** Conocimiento básico de
arquitecturas de agentes, herramientas y patrones de orquestación.

Explicación de los componentes:

- **Microsoft Foundry:** Microsoft Foundry es una plataforma en la nube
  para desarrollar, implementar y administrar agentes de IA
  empresariales. Proporciona un entorno administrado de Agent Service,
  administración centralizada de proyectos y monitoreo mediante
  Application Insights, garantizando confiabilidad, seguridad y
  observabilidad a nivel empresarial durante todo el ciclo de vida del
  agente.

- **Microsoft Agent Framework SDK:** El SDK oficial de Python para
  construir agentes inteligentes y modulares que reemplaza AutoGen y
  Semantic Kernel. Incluye comunicación nativa Agent-to-Agent,
  integración con Model Context Protocol y compatibilidad con Microsoft
  Foundry, permitiendo sistemas de agentes empresariales listos para
  producción con uso estandarizado de herramientas.

- **Azure AI Search:** Un motor de búsqueda vectorial que habilita
  flujos de trabajo de Retrieval-Augmented Generation. Proporciona
  recuperación híbrida combinando similitud vectorial y búsqueda por
  palabras clave, ranking semántico para mejorar la relevancia y
  capacidades de indexación de documentos, garantizando que los agentes
  entreguen respuestas fundamentadas y precisas desde fuentes de
  conocimiento empresariales.

- **Model Context Protocol (MCP):** Una interfaz estandarizada que
  permite a los agentes acceder de manera segura a conocimiento y
  herramientas externas. MCP se conecta a fuentes de datos
  empresariales, APIs externas como Freshdesk y herramientas
  personalizadas con esquemas estructurados, garantizando interacciones
  confiables y auditables, y formando la base de sistemas de IA
  empresariales extensibles.

- **Chat Response Agent:** Un agente de respuesta de chat de una sola
  interacción y sin estado para desarrollo y pruebas locales. Procesa
  solicitudes de forma independiente sin mantener contexto, ejecutándose
  en entornos locales y respondiendo de inmediato. Es ideal para
  prototipar lógica principal y validar comportamiento antes de avanzar
  a producción con agentes persistentes.

- **Persistent Agent:** Un servicio alojado en la nube y de larga
  duración en Microsoft Foundry que mantiene estado a lo largo de
  conversaciones. Admite integración con herramientas externas mediante
  MCP, colaboración Agent-to-Agent y confiabilidad a escala empresarial
  con monitoreo integrado, proporcionando fundamentos para aplicaciones
  de producción que requieren experiencias conversacionales con estado.

- **Planner Agent**: Un orquestador inteligente que analiza consultas de
  los usuarios para dirigirlas a los agentes especialistas
  correspondientes. Utiliza razonamiento de IA y heurísticas de palabras
  clave para clasificar consultas en dominios como recursos humanos,
  finanzas o cumplimiento, garantizando una distribución óptima de
  tareas y actuando como punto central de coordinación.

- **Worker Agents:** Especialistas de dominio con experiencia en áreas
  específicas como recursos humanos, finanzas o cumplimiento. Cada
  agente tiene instrucciones específicas, herramientas especializadas y
  fuentes de conocimiento relevantes. Colaboran con los planner agents
  mediante comunicación A2A, entregando respuestas precisas y
  autorizadas para consultas complejas específicas del dominio.

- **Azure OpenAI:** Servicio de nivel empresarial que proporciona acceso
  a modelos avanzados de LLM mediante endpoints API seguros. Ofrece chat
  completion, modelos de embedding, filtrado de contenido y funciones de
  cumplimiento. Se integra perfectamente con Microsoft Foundry,
  permitiendo que los agentes aprovechen GPT-4 manteniendo privacidad y
  controles de gobernanza de datos.

# Laboratorio 5: Creación de un agente de IA con RAG mediante Microsoft Foundry

**Descripción general**

En este laboratorio, creará su primer agente de IA utilizando el portal
de Microsoft Foundry. Comenzará cargando documentos de políticas
empresariales e ingiriéndolos en Azure AI Search para preparar una base
de conocimiento. Luego, configurará el agente utilizando el Microsoft
Agent Framework para habilitar Retrieval-Augmented Generation (RAG).
Finalmente, probará las respuestas del agente y analizará los registros
de ejecución para observar cómo recupera y procesa información.

**Objetivos del laboratorio**

Realizará las siguientes tareas en este laboratorio.

- Tarea 1: Crear los recursos de Azure

- Tarea 2: Crear un agente de IA en Microsoft Foundry

- Tarea 3: Conectar Azure AI Search para RAG

- Tarea 4: Probar y observar los registros de ejecución del agente

## Tarea 1: Crear los recursos de Azure

En esta tarea, creará todos los recursos de Azure necesarios para
realizar este laboratorio.

### Tarea 1.1: Crear una cuenta de almacenamiento

1.  Inicie sesión en Azure Portal +++https://portal.azure.com+++ usando
    las credenciales indicadas y seleccione **Storage accounts**.

- Username - +++@lab.CloudPortalCredential(User1).Username+++

- TAP - <+++@lab.CloudPortalCredential(User1).TAP>+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image1.png)

2.  Seleccione **Create**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

3.  Ingrese los siguientes detalles y seleccione **Review + create**.
    Luego seleccione **Create**.

- Storage account name - +++aistorage@lab.LabInstance.Id+++

- Preferred storage type – Seleccione **Azure Blob Storage or Azure Data
  Lake Storage Gen2**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image3.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image4.png)

4.  Una vez creado el recurso, seleccione **Go to resource**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

5.  Seleccione **Upload**, luego **Create new** para crear un nuevo
    contenedor. Nómbrelo **datasets** y seleccione **Ok**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

![A screenshot of a login box AI-generated content may be
incorrect.](./media/image7.png)

6.  Seleccione **Browse for files**, elija los archivos de políticas
    desde **C:\Labfiles\Day 2** y haga clic en **Upload**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

![A screenshot of a upload box AI-generated content may be
incorrect.](./media/image9.png)

La cuenta de almacenamiento se creó correctamente y ya contiene los
documentos de políticas.

### Tarea 1.2: Crear un recurso Foundry

En esta tarea, creará un recurso Foundry necesario para acceder a
Microsoft Foundry.

1.  Desde la página principal de Azure Portal
    (+++https://portal.azure.com+++), seleccione **Foundry**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.png)

2.  Seleccione **Foundry** en el panel izquierdo y luego **Create**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

3.  Ingrese los siguientes detalles y seleccione **Review + create**.

- Name – <+++agentic-@lab.LabInstance.Id>+++

- Default project name – <+++agentic-ai-project-@lab.LabInstance.Id>+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.png)

4.  Seleccione **Create**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

5.  Verifique que el recurso se haya creado.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

6.  Abra **<agentic-ai-project-@lab.LabInstance.Id>** y seleccione **Go
    to Foundry portal**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.png)

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image16.png)

7.  En Microsoft Foundry, seleccione **Models + endpoints**, luego **+
    Deploy model → Deploy base model**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.png)

8.  Busque +++gpt-4o-mini+++, selecciónelo y haga clic en **Confirm**.

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image18.png)

9.  Seleccione **Deploy**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.png)

10. Del mismo modo, busque +++text-embedding-ada-002+++ e impleméntelo.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image21.png)

En esta tarea, ha creado correctamente el recurso de Foundry y ha
implementado un modelo de chat y un modelo de embeddings en él.

### Tarea 1.3: Crear Application insights

En esta tarea, creará un recurso de Application insights, que es
necesario para el monitoreo.

1.  Desde la página principal del Azure portal, haga clic en
    **Subscriptions** y seleccione la suscripción asignada.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

2.  Desde el panel izquierdo, seleccione **Resource providers**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

3.  Busque **+++Operational+++**, seleccione los tres puntos junto a
    **Microsoft.OperationalInsights** y haga clic en **Register**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

4.  Desde el panel izquierdo de Microsoft Foundry, seleccione
    **Monitoring**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

5.  Seleccione **Create New** → proporcione el nombre
    <+++agent-insights-@lab.LabInstance.Id>+++ y, posteriormente,
    seleccione **Create**.

![A screenshot of a application AI-generated content may be
incorrect.](./media/image27.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.png)

En esta tarea, usted ha creado el recurso de **Application Insights**.

### Tarea 1.4: Crear un recurso Search

Antes de que un AI Agent pueda responder preguntas empresariales con
precisión, debe acceder a fuentes de datos confiables. **Azure AI
Search** habilita **Retrieval-Augmented Generation (RAG)** al indexar
documentos como políticas, contratos y manuales. Un índice actúa como un
catálogo consultable que divide el contenido en fragmentos, agrega
metadatos y permite que el agente recupere la información correcta
durante una conversación.

En esta tarea, usted indexará los documentos cargados utilizando **Azure
AI Search** para crear una base de conocimiento consultable.

1.  Desde la página principal del Azure portal, seleccione **Foundry**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.png)

2.  Seleccione **AI Search** desde el panel izquierdo y luego seleccione
    **+ Create.**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

3.  Ingrese los siguientes detalles y seleccione **Review + create**:

- Service name - +++ai-knowledge-@lab.LabInstance.Id+++

- Region - East US2

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image32.png)

4.  Seleccione **Create** una vez que la validación finalice. Seleccione
    **Go to resource** cuando el recurso haya sido creado.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image34.png)

5.  Seleccione **Import data (new)**.

![A screenshot of a search engine AI-generated content may be
incorrect.](./media/image35.png)

6.  Seleccione **Azure Blob Storage** en **Choose data source**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.png)

7.  En el siguiente panel, seleccione la opción **RAG**, ya que se está
    creando un agente basado en recuperación.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image37.png)

A continuación, se describe el propósito de cada una de estas opciones:

a\. **Keyword Search:** Utilizado para experiencias de búsqueda
tradicionales basadas en coincidencias exactas de palabras clave. Indexa
texto para que el usuario pueda localizar información mediante
coincidencias de palabras, sin razonamiento de IA.  
b. **RAG (Retrieval-Augmented Generation):** Combina recuperación de
documentos con generación de IA. Ingresa texto (y OCR simple) para que
un agente de IA proporcione respuestas fundamentadas y con contexto.  
c. **Multimodal RAG:** Amplía RAG para manejar contenido visual complejo
como diagramas, tablas, flujos de trabajo o gráficos. Permite que la IA
interprete tanto elementos textuales como visuales para ofrecer
respuestas más completas.

8.  Seleccione <aistorage@lab.LabInstance.Id> en **Storage account** y
    **datasets** en **Blob container**, y seleccione **Next**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

9.  Seleccione los siguientes detalles y seleccione **Next.**

- Kind – Azure AI Foundry (Preview)

- Azure AI Foundry/Hub project –
  <agentic-ai-project-@lab.LabInstance.Id>

- Model deployment – text-embedding-002-ada

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.png)

10. Seleccione **Next** en las pantallas siguientes hasta llegar a
    **Review and create**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image40.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.png)

11. Seleccione **Create** en la pantalla **Review and create**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.png)

12. Seleccione **Close** en el cuadro de diálogo **Create succeeded**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.png)

Usted ha ingestido correctamente el conjunto de datos en **Azure AI
Search** y creado un índice consultable. En la siguiente tarea, creará
un AI Agent y conectará este índice como su fuente de conocimiento.

## Tarea 2: Crear un AI Agent en Microsoft Foundry

En esta tarea, usted creará un nuevo AI Agent en Microsoft Foundry y
configurará su propósito principal, instrucciones y modelo utilizando la
interfaz del Microsoft Agent Framework.

1.  Regrese a su grupo de recursos y, desde la lista de recursos,
    seleccione el recurso **agentic**-foundry.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.png)

2.  En el panel siguiente, haga clic en **Go to Foundry portal**. Será
    dirigido al Microsoft Foundry portal, donde creará su primer agente.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.png)

3.  Una vez dentro del Foundry Portal, seleccione **Agents (1)** desde
    el menú izquierdo. Verá un agente previamente creado; si no aparece,
    haga clic en **+ New agent (2)** para crearlo.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image46.png)

4.  Seleccione el **agente** recién creado y se abrirá un panel de
    configuración en el lado derecho. Proporcione los siguientes
    detalles:

[TABLE]

> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image47.png)

5.  Usted ha creado correctamente un agente en Microsoft Foundry. En la
    siguiente tarea, lo enriquecerá con conocimiento conectando su
    índice previamente creado.

## Tarea 3: Conectar Azure AI Search para RAG

En esta tarea, usted integrará **Azure AI Search** con su agente
utilizando el panel de integración de conocimiento, habilitando
respuestas con recuperación aumentada mediante **MCP (Model Context
Protocol)**.

1.  En el mismo panel de configuración del agente, desplácese hacia
    abajo y haga clic en **+ Add** para el parámetro **Knowledge**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image48.png)

2.  En el panel **Add knowledge**, seleccione **Azure AI Search**, ya
    que usted tiene el índice preparado en dicho recurso.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image49.png)

3.  En el siguiente panel, en **Azure AI Search resource connection**,
    haga clic en la flecha desplegable **(1)** y seleccione **Connect
    other Azure AI Search resource (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.png)

4.  En el siguiente panel, revise que el recurso correcto de AI Search
    esté seleccionado y haga clic en **Add connection**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image51.png)

5.  En el paso **Adding Azure AI Search**, configure los siguientes
    parámetros y haga clic en **Connect (5)** al finalizar:

[TABLE]

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image52.png)

6.  El agente ahora ha sido enriquecido correctamente con el índice de
    Azure AI Search, el cual actúa como una base de conocimiento
    consultable para recuperar información precisa durante las
    conversaciones.

## Tarea 4: Probar y observar los registros de ejecución del agente

En esta tarea, usted probará su agente formulando preguntas relacionadas
con políticas y revisará los registros estructurados para verificar el
uso de herramientas, llamadas de búsqueda y respuestas fundamentadas.

1.  Antes de probar el agente, conecte **Application Insights** para
    habilitar registros detallados y visibilidad de trazas.

2.  En Microsoft Foundry portal, seleccione **Monitoring (1)** desde el
    menú izquierdo, seleccione **agent-insights- (2)** y haga clic en
    **Connect (3)**.

![](./media/image53.png)

3.  Una vez completado, seleccione **Agents (1)** desde el menú
    izquierdo, elija el agente **EnterpriseAssistant (2)** y haga clic
    en **Try in playground (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image54.png)

4.  Se abrirá un panel de chat donde usted podrá ingresar sus
    indicaciones. El agente responderá utilizando los documentos y
    conjuntos de datos que usted ha conectado.

Ejemplos de indicaciones:

- +++What is the employee travel reimbursement policy?+++

- +++Summarize the contract approval rules and cite the document.+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image55.png)

5.  Una vez que el agente responda, haga clic en **Thread logs** desde
    el menú superior para revisar los registros y trazas del hilo
    actual.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image56.png)

6.  Explore y revise estas métricas, trazas y evaluaciones, que
    presentan una vista detallada del registro del agente.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image57.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image58.png)

7.  Ahora, navegue al panel de monitoring, donde previamente conectó
    **Application Insights**, y seleccione la pestaña **Resource usage**
    para revisar todas las métricas y valores.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image59.png)

8.  Usted ha creado exitosamente un agente basado en RAG, respaldado por
    conjuntos de datos empresariales seleccionados. En la siguiente
    etapa, habilitará la colaboración entre múltiples agentes,
    permitiendo delegación, razonamiento y trabajo conjunto inteligente.

**Resumen**

En este laboratorio, usted creó su primer AI Agent en Microsoft Foundry
y lo conectó a una base de conocimiento indexada. Cargó documentos, los
ingestó en Azure AI Search y habilitó RAG mediante la integración con
Microsoft Agent Framework. Al probar el agente y revisar los registros
de ejecución, obtuvo experiencia directa en cómo los agentes recuperan
información fundamentada y generan respuestas preparadas para entornos
empresariales.
