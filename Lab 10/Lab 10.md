# Gestionar, asegurar y monitorear agentes de IA a escala

**Descripción general**

Este laboratorio práctico se centra en gestionar, asegurar y monitorear
agentes de IA a escala utilizando el Azure AI Agent Service SDK y
Microsoft Foundry. Los participantes profundizarán en prácticas de nivel
de producción esenciales para implementaciones de IA empresariales,
comenzando con AgentOps, la disciplina de observar y gobernar agentes de
IA mediante la integración de OpenTelemetry y Azure Application
Insights.

El taller enfatiza la importancia de la IA responsable implementando los
seis principios fundamentales de Microsoft, que incluyen fairness,
reliability, privacy y accountability, mediante filtros de seguridad de
contenido configurables que detectan y bloquean salidas dañinas como
hate speech, violencia e información sensible.

Adicionalmente, los participantes construirán flujos de trabajo
sofisticados human-in-the-loop (HITL), ejemplificados mediante un
sistema de detección de fraude donde agentes especializados analizan
actividad sospechosa y enrutan inteligentemente casos de alto riesgo a
analistas humanos para la toma de decisiones críticas.

A lo largo del laboratorio, trabajará con sistemas multiagente que
colaboran en tareas de retrieval, validación y orquestación, obteniendo
experiencia práctica con end-to-end tracing, visualización de métricas
personalizadas, dashboards de monitoreo de desempeño y gestión de flujos
en tiempo real.

Al final de este taller, los participantes habrán dominado las
habilidades esenciales para implementar, monitorear y gobernar agentes
de IA en entornos empresariales, asegurando que operen de manera segura,
ética y eficiente, cumpliendo con las políticas organizacionales y los
requisitos regulatorios.

**Objetivos**

Al final de este laboratorio, usted podrá:

- **Habilitar observabilidad y monitoreo:** Implementar end-to-end
  tracing y telemetry para agentes de IA usando OpenTelemetry integrado
  con Azure Application Insights, capturando comportamiento de los
  agentes, métricas de desempeño y trazas de ejecución.

- **Visualizar métricas de agentes:** Crear dashboards y workbooks
  personalizados en Application Insights para monitorear rendimiento,
  tiempos de respuesta, uso de tokens, precisión de enrutamiento y
  estado del sistema en tiempo real.

- **Implementar prácticas de IA responsable:** Configurar filtros de
  seguridad de contenido en Microsoft Foundry para detectar y bloquear
  salidas dañinas (hate speech, violence, sensitive content) y asegurar
  comportamiento ético y conforme de los agentes de IA.

- **Crear flujos Human-in-the-Loop:** Diseñar e implementar sistemas de
  detección de fraude donde los agentes de IA analicen alertas y enruten
  casos de alto riesgo a analistas humanos para revisión y toma de
  decisiones.

- **Monitorear sistemas multiagente:** Rastrear la comunicación
  agent-to-agent, trazar flujos distribuidos entre múltiples agentes
  especializados e identificar cuellos de botella o fallos en
  orquestaciones complejas

**Explicación de componentes**

- **Microsoft Foundry:** Plataforma en la nube para desarrollar,
  implementar y gestionar modelos de IA con gobernanza centralizada,
  observabilidad y funciones de cumplimiento para aplicaciones
  empresariales.

- **Azure AI Hub:** Recurso de nivel superior en Azure que proporciona
  un entorno central, seguro y colaborativo para que los equipos creen,
  gestionen e implementen aplicaciones de IA con recursos compartidos y
  políticas de gobernanza.

- **Azure AI Search:** Servicio de búsqueda vectorial que habilita
  Retrieval-Augmented Generation (RAG) indexando y recuperando
  documentos relevantes para mejorar respuestas generadas por IA con
  información fundamentada.

- **Azure AI Services:** Colección de servicios en la nube con APIs y
  modelos preconstruidos y personalizables para visión, lenguaje, voz y
  toma de decisiones.

- **OpenTelemetry:** Estándar abierto para trazado distribuido, métricas
  y logging, integrado nativamente en el Microsoft Agent Framework para
  capturar trazas de ejecución, métricas de desempeño y seguimiento de
  errores.

- **Content Safety Filters:** Sistema de filtrado incorporado en
  Microsoft Foundry que detecta y bloquea automáticamente salidas
  dañinas en categorías como discursos de odio, violencia, contenido
  sexual e información sensible (PII).

- **LLMs y Embeddings:** Los Large Language Models proporcionan
  comprensión y generación de lenguaje natural, mientras que los
  embeddings son representaciones vectoriales usadas para similitud de
  texto, búsqueda y recuperación de conocimiento en aplicaciones de IA.

# Laboratorio 10: Prerrequisitos – Configuración del índice de conocimiento y sistema de tickets

**Duración Estimada:** 30 Minutos

**Descripción general**

En este laboratorio prerrequisito, configurará los componentes
fundamentales necesarios para un flujo de trabajo impulsado por IA que
pueda recuperar conocimiento empresarial y crear tickets de soporte
automáticamente. El enfoque está en preparar una base de conocimiento
**searchable**, habilitar que los agentes de IA consulten esa
información mediante una herramienta **MCP (Model Context Protocol)** e
integrar un sistema de tickets para acciones posteriores.

Al completar estas tareas, establecerá la infraestructura central que
permite a los agentes:

- Recuperar información relevante de datos indexados.

- Utilizar esa información de manera contextual durante conversaciones o
  flujos de trabajo.

- Escalar problemas creando tickets en un servicio externo.

Esta configuración garantiza que los laboratorios posteriores se
ejecuten sin problemas y reflejen un escenario empresarial real.

**Objetivos del Laboratorio**

En este laboratorio realizará las siguientes tareas:

- Tarea 1: Preparar el índice de conocimiento

- Tarea 2: Configuración de Freshworks para gestión de tickets

## Tarea 1: Crear los recursos de Azure

En esta tarea, creará todos los recursos de Azure necesarios para
realizar este laboratorio.

### Tarea 1.1: Crear una cuenta de almacenamiento

1.  Inicie sesión en el portal de Azure en
    +++https://portal.azure.com+++ usando las siguientes credenciales y
    seleccione **Storage accounts**.

- Username - +++@lab.CloudPortalCredential(User1).Username+++

- TAP - <+++@lab.CloudPortalCredential(User1).TAP>+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image1.png)

2.  Seleccione **Create**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

3.  Ingrese los siguientes detalles y seleccione **Review + create**,
    luego **Create** en la siguiente pantalla:

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
    contenedor. Nómbrelo +++**datasets**+++ y seleccione **Ok**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

![A screenshot of a login box AI-generated content may be
incorrect.](./media/image7.png)

6.  Seleccione **Browse for files**, seleccione los archivos de
    políticas desde **C:\Labfiles\Day 2** y haga clic en **Upload**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

![A screenshot of a upload box AI-generated content may be
incorrect.](./media/image9.png)

Ahora, la cuenta de almacenamiento se ha creado correctamente y está
cargada con los documentos de políticas.

### Tarea 1.2: Crear recurso Foundry

En esta tarea, creará un recurso Foundry para acceder a Microsoft
Foundry.

1.  Desde la página principal del portal de Azure
    (+++https://portal.azure.com+++), seleccione **Foundry**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.png)

2.  Seleccione **Foundry** desde el panel izquierdo y luego **Create**
    para crear el recurso.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

3.  Ingrese los siguientes detalles y seleccione **Review + create**.

- Name – <+++agentic-@lab.LabInstance.Id>+++

- Default project name – <+++agentic-ai-project-@lab.LabInstance.Id>+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.png)

4.  Seleccione **Create** una vez validado.

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

7.  En Microsoft Foundry, seleccione **Models + endpoints** en el panel
    izquierdo. Seleccione **+ Deploy model -\> Deploy base model**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.png)

8.  Busque +++gpt-4o-mini+++, selecciónelo y haga clic en **Confirm**
    para implementar el modelo.

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image18.png)

9.  Seleccione **Deploy** en la ventana de implementación.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.png)

10. De manera similar, busque +++text-embedding-ada-002+++ e implemente
    el modelo.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image21.png)

Con esto, ha creado el recurso Foundry e implementado un modelo de chat
y un modelo de embeddings.

### Tarea 1.3: Crear Application Insights

En esta tarea, usted creará un recurso de Application Insights, el cual
es requerido para realizar el monitoreo.

1.  Desde la página principal del portal de Azure, seleccione
    **Subscriptions** y la suscripción asignada.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

2.  Seleccione **Resource providers** desde el panel izquierdo.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

3.  Busque **Operational**, seleccione los tres puntos junto a
    **Microsoft.OperationalInsights** y haga clic en **Register**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

4.  Desde el panel izquierdo de Microsoft Foundry, seleccione
    **Monitoring**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

5.  Seleccione **Create New**, proporcione el nombre
    +++agent-insights-@lab.LabInstance.Id+++ y luego **Create**.

![A screenshot of a application AI-generated content may be
incorrect.](./media/image27.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.png)

Con esto, se ha creado el recurso **Application Insights**.

### Tarea 1.4: Crear un recurso Search

Antes de que un agente de IA pueda responder preguntas empresariales con
precisión, debe acceder a fuentes de datos confiables. **Azure AI
Search** habilita **RAG (Retrieval-Augmented Generation)** indexando
documentos como políticas, contratos y manuales.

En esta tarea, indexará los documentos cargados usando Azure AI Search
para crear una base de conocimiento **searchable**:

1.  Desde la página principal del portal de Azure, seleccione
    **Foundry**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.png)

2.  Seleccione **AI Search** en el panel izquierdo y luego **+ Create.**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

3.  Ingrese los detalles y seleccione **Review + create**.

- Service name - +++ai-knowledge-@lab.LabInstance.Id+++

- Region - East US2

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image32.png)

4.  Seleccione **Create** y luego **Go to resource** una vez creado.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image34.png)

5.  Seleccione **Import data (new)**.

![A screenshot of a search engine AI-generated content may be
incorrect.](./media/image35.png)

6.  Seleccione **Azure Blob Storage** bajo **Choose data source**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.png)

7.  En el siguiente panel, seleccione la opción **RAG** ya que estamos
    creando un agente basado en retrieval.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image37.png)

Opciones explicadas:

1.  **Keyword Search**: Búsqueda tradicional basada en palabras clave
    exactas. Indexa texto para permitir búsqueda por coincidencia de
    palabras, sin razonamiento de IA.

2.  **RAG (Retrieval-Augmented Generation)**: Combina recuperación de
    documentos con generación de IA. Permite que un agente IA entregue
    respuestas fundamentadas y contextuales.

3.  **Multimodal RAG**: Extiende RAG para manejar contenido visual
    complejo como diagramas, tablas o flujos. Permite respuestas más
    ricas e interpretativas.

&nbsp;

8.  Seleccione <aistorage@lab.LabInstance.Id> en **Storage account** y
    **datasets** bajo **Blob containe**r, luego seleccione **Next**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

9.  Seleccione los siguientes detalles y haga clic en **Next**.

- Kind – **Azure AI Foundry (Preview)**

- Azure AI Foundry/Hub project –
  <agentic-ai-project-@lab.LabInstance.Id>

- Model deployment – text-embedding-002-ada

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.png)

10. Haga clic en **Next** hasta llegar a **Review and create**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image40.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.png)

11. Seleccione **Create.**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.png)

12. Cierre el diálogo de creación exitosa (**Close**).

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.png)

Con esto, ha ingerido correctamente el dataset en Azure AI Search y
creado un índice **searchable**. En la siguiente tarea, creará un agente
de IA y conectará este índice como fuente de conocimiento.

# Tarea 2: Configuración de Freshworks para la gestión de tickets

En esta tarea, configurará **Freshworks** para habilitar la gestión de
tickets e integración empresarial con su sistema multi-agente.

**Freshworks** es una plataforma en la nube para soporte al cliente que
mejora operaciones de atención y satisfacción del usuario. Permite
comunicación omnicanal (email, chat, teléfono, redes sociales) desde una
interfaz central, y ofrece herramientas de automatización, asignación de
tickets y analíticas de desempeño.

1.  Copie la URL y péguela en una nueva pestaña dentro del VM para abrir
    el portal de **Freshworks**.

    - URL:

> +++https://www.freshworks.com/freshdesk/lp/home/?tactic_id=3387224&utm_source=google-adwords&utm_medium=FD-Search-Brand-India&utm_campaign=FD-Search-Brand-India&utm_term=freshdesk&device=c&matchtype=e&network=g&gclid=EAIaIQobChMIuOK90qvLjQMV_dQWBR3JAi9VEAAYASAAEgK87_D_BwE&audience=kwd-30002131023&ad_id=282519464145&gad_source=1&gad_campaignid=671502402+++

2.  En el portal, seleccione **Start free trial**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.png)

3.  En la siguiente pantalla, proporcione los detalles y haga clic
    en **Try it free (6)**:

    - **First name:** LODS

    - **Last name:** User1

    &nbsp;

    - **Work
      email:** **+++@lab.CloudPortalCredential(User1).Username+++**

    &nbsp;

    - **Company name:** Zava

    - **Organization size:** Seleccione **1-10**

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image45.png)

4.  Proporcione los detalles adicionales y haga clic en **Next (4)**:

    - **What industry are you from ?:** seleccione **Software and
      internet (1)**

    - **How many employees are there in your company?:** seleccione
      **1-10 (2)**

    - Seleccione **I'm trying customer service software for the first
      time (3)**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image46.png)

5.  Copie la URL proporcionada y ábrala en **Outlook** dentro del VM.

    - URL:

> +++https://go.microsoft.com/fwlink/p/?LinkID=2125442&clcid=0x409&culture=en-us&country=us+++

6.  En **pick an account**, seleccione la cuenta asignada para este
    laboratorio.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.png)

7.  Abra el email de verificación de Freshworks y haga clic en
    **Activate Account**.

> ![A screenshot of a computer screen AI-generated content may be
> incorrect.](./media/image48.png)

**Nota:** Si no logra localizar el correo electrónico de activación de
Freshworks, espere unos minutos, ya que podría existir un retraso en la
entrega del mensaje. Si el correo no llega después de algún tiempo,
considere reiniciar los pasos para activar su prueba gratuita en una
nueva ventana privada/incógnito. Además, revise su carpeta de spam o
correo no deseado, ya que el mensaje podría haber sido filtrado allí.

8.  En el siguiente panel, proporcione la contraseña en **Enter password
    (1)** y proporcione la misma contraseña en **Confirm password (2)**.
    Haga clic en **Activate your account (3)**.

> ![A screenshot of a login screen AI-generated content may be
> incorrect.](./media/image49.png)

9.  Cuando esté dentro del portal, haga clic en el ícono de **Profile
    (1)** en la parte superior derecha y seleccione **Profile settings
    (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.png)

10. En la página del perfil, haga clic en **View API Key** para obtener
    las API Keys.

![A screenshot of a web page AI-generated content may be
incorrect.](./media/image51.png)

**Nota:** Si no logra encontrar esta opción, minimice el tamaño de la
pantalla usando **CTRL + -**.

11. En el siguiente panel, complete el **CAPTCHA**.

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image52.png)

12. Copie la **API Key** en un bloc de notas; la utilizará más adelante.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image53.png)

13. Desde la pestaña del navegador, copie la **Account URL** tal como se
    muestra y pegue el valor en un bloc de notas. También la utilizará
    más adelante.

![](./media/image54.png)

**Resumen**

Al completar este laboratorio prerrequisito, ha configurado la base
esencial para un flujo de trabajo de agentes de punta a punta. Preparó
un índice de conocimiento searchable, habilitó que los agentes consulten
esos datos mediante una herramienta MCP construida sobre Azure AI Search
e integró Freshworks para la gestión automatizada de tickets.

Esta base asegura que los agentes puedan recuperar contexto preciso,
tomar decisiones informadas y escalar incidencias de forma eficiente,
preparando el entorno para escenarios más avanzados impulsados por
agentes en los laboratorios siguientes.

Ha completado este laboratorio con éxito. Por favor, haga clic en **Next
\>\>** para continuar.
