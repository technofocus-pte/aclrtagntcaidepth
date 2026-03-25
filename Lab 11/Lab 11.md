# Laboratorio 11: AgentOps – Observabilidad y gestión

**Duración estimada:** 60 minutos

**Descripción general**

En este laboratorio, se enfocará en AgentOps, la disciplina de
monitorear, gobernar y gestionar agentes de IA en entornos de
producción. Explorará cómo habilitar observabilidad y telemetría
utilizando la integración nativa del Microsoft Agent Framework con
Application Insights mediante **OpenTelemetry**.

Acerca de OpenTelemetry en **Microsoft Agent Framework**

Microsoft Agent Framework se integra de forma nativa con OpenTelemetry,
el estándar abierto para trazabilidad distribuida, métricas y registros.
Proporciona visibilidad de extremo a extremo sobre el comportamiento de
los agentes al capturar automáticamente datos de telemetría, como span
traces, llamadas a herramientas, respuestas del modelo y rendimiento del
flujo de trabajo.

Usando esta integración, los desarrolladores pueden exportar datos de
observabilidad directamente a Azure Monitor, Application Insights o
cualquier otro backend compatible con OpenTelemetry.

Este enfoque estandarizado ayuda a rastrear cada acción del agente en
sistemas multi-agente complejos, permitiendo ajuste de rendimiento,
resolución de problemas y auditorías de cumplimiento con mínima
configuración.

Objetivos del laboratorio

Realizará las siguientes tareas:

- Tarea 1: Habilitar observabilidad del agente con OpenTelemetry.

- Tarea 2: Visualizar métricas del agente.

- Tarea 3: Monitorear métricas específicas del agente en el Foundry
  Portal.

## Tarea 1: Habilitar la observabilidad del agente con OpenTelemetry

En esta tarea integrará OpenTelemetry y la observabilidad del Agent
Framework en su proyecto. Configurará telemetry exporters, inicializará
tracing con setup_observability() y capturará spans detallados para cada
etapa de su flujo de trabajo, incluyendo agent routing, recuperación de
Azure AI Search y creación de tickets.

Esto habilita visibilidad unificada del comportamiento del agente y
correlación entre sistemas usando trace IDs en Application Insights.

1.  En lugar de modificar nuevamente el código anterior, trabajará en
    una nueva carpeta que ya contiene los archivos actualizados con
    observabilidad habilitada. Comprenda cómo se integran telemetría,
    tracing y monitoreo usando Microsoft Agent Framework Observability y
    Application Insights.

2.  En Visual Studio Code, antes de abrir la nueva carpeta, seleccione
    el archivo .env y copie su contenido para guardarlo en un bloc de
    notas.

3.  Una vez hecho, haga clic en **File** desde el menú superior y
    seleccione **Open Folder**.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image1.png)

4.  En el panel de carpetas, navegue a C:\telemetry-codefiles y
    seleccione esa carpeta.

5.  Una vez abierta, los archivos en el explorador deberían verse
    similares a esto.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

6.  Por favor, revise los archivos de código para verificar cómo se ha
    implementado OpenTelemetry en todos los agentes y cómo se realiza el
    proceso de tracing.

> **Versión estándar**
>
> OpenTelemetry ha sido integrado en todo el flujo de trabajo del agente
> utilizando el paquete agent_framework.observability.

- Se importó get_tracer() y se utilizaron spans de OpenTelemetry para
  capturar telemetría estructurada en cada operación crítica.

- Se encapsularon funciones clave (clasificación, routing, RAG, creación
  de tickets) en spans con atributos contextuales.

- Se agregó configuración unificada de observabilidad en el arranque
  mediante setup_observability() para configurar exporters y pipelines
  de métricas.

- Se registraron atributos personalizados como query text, decisiones de
  routing y métodos de fallback para mayor visibilidad.

- Se mejoró el manejo de errores para registrar excepciones y vincular
  cada ejecución del flujo de trabajo con un trace ID para correlación
  multisistema.

> **Mejoras por archivo**

main.py – Trazabilidad y métricas end-to-end

- Configuración del pipeline de trazabilidad de OpenTelemetry y de los
  exporters.

- Orquestación multiagente encapsulada en spans para proporcionar
  visibilidad completa del flujo de trabajo.

- Spans añadidos para el enrutamiento, la recuperación de información
  (RAG), las respuestas del agente y la creación de tickets.

> planner_agent.py – Observabilidad del routing

- Se añadió un tracer (get_tracer()) para monitorear la lógica de
  clasificación.

- Captura de las respuestas sin procesar del LLM, los puntajes de
  confianza y las métricas de fallback.

- Diferenciación entre la clasificación realizada por IA y la heurística
  mediante spans etiquetados (SpanKind.INTERNAL).

> azure_search_tool.py – Observabilidad del RAG

- Spans añadidos para medir latencia y éxito de llamadas a Azure Search
  API.

- Registro de conteo de documentos recuperados y tamaño de payload como
  métricas personalizadas.

- Captura de errores de búsqueda y datos de rendimiento dentro de trazas
  OpenTelemetry.

> freshdesk_tool.py – Observabilidad en creación de tickets

- Spans añadidos para rastrear duración y estado de llamadas al API.

- Registro de ticket IDs, tags y detalles del solicitante para auditoría
  trazable.

- Monitoreo de latencia y errores del API externo para mejor gestión de
  incidentes.

7.  Una vez revisado, haga clic derecho en **.env.example (1)** y
    seleccione **Rename (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

8.  Una vez terminado el paso anterior, renombre el archivo de
    **.env.example → .env** para activarlo.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image4.png)

9.  Seleccione el archivo .env y pegue el contenido que copió
    previamente.

10. En Azure Portal, vaya al grupo de recursos **agenticai** y abra el
    servicio **ai-knowledge-search**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

11. Seleccione **Keys (1)** desde el menú de la izquierda, bajo
    **Settings**, y copie la **Query key (2)** utilizando la opción de
    copiar, tal como se muestra.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

12. Después de copiarla, péguela de forma segura en un bloc de notas;
    luego seleccione Indexes desde el menú de la izquierda, bajo
    **Search Management**, y copie el **Index Name (2).**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

13. En el panel de Visual Studio Code, seleccione el archivo **.env**,
    ya que debe agregar las claves de **AI Search** para la conexión.

> \# Azure AI Search (MCP)
>
> AZURE_SEARCH_ENDPOINT=https://ai-knowledge--@lab.LabInstance.Id.search.windows.net/
>
> AZURE_SEARCH_API_KEY=\[Query_Key\]
>
> AZURE_SEARCH_INDEX=\[Index_Name\]

**Note:** Reemplace los valores de Query_Key y Index_Name con los que
copió anteriormente.

14. Agregue al archivo .env el siguiente contenido.

> AZURE_OPENAI_ENDPOINT=https://agentic-
> @lab.LabInstance.Id.cognitiveservices.azure.com/
>
> AZURE_OPENAI_API_KEY=\<Replace with Azure OpenAI key\>
>
> AZURE_OPENAI_RESPONSES_DEPLOYMENT_NAME=gpt-4o-mini
>
> AZURE_OPENAI_API_VERSION=2025-03-01-preview

15. Agregue las siguientes variables clave del proyecto Foundry al
    archivo .env

> \# Azure AI Project Configuration
>
> AZURE_AI_PROJECT_ENDPOINT=**\<Microsoft Foundry endpoint\>**
>
> AZURE_AI_MODEL_DEPLOYMENT_NAME=gpt-4o-mini
>
> Busque el endpoint del proyecto Microsoft Foundry en la página de
> Overview y reemplace \< **Microsoft Foundry endpoint** \> con ese
> valor.
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image8.png)

![](./media/image9.png)

16. Una vez hecho, agregue las siguientes variables de App Insights al
    mismo archivo:

> \# Observability and Monitoring Configuration
>
> APPLICATIONINSIGHTS_CONNECTION_STRING=**\<Connection string\>**
>
> ENABLE_OTEL=true
>
> ENABLE_SENSITIVE_DATA=true
>
> Abra el recurso de Application Insights desde el portal de Azure,
> copie la cadena de conexión y reemplace **\<Connection string\>** con
> el valor copiado.
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image10.png)

17. En el archivo .env, agregue el siguiente contenido con la clave API
    y la URL de cuenta de Freshdesk que copió anteriormente.

> \# Freshdesk Configuration
>
> FRESHDESK_DOMAIN=\[Domain_URL\]
>
> FRESHDESK_API_KEY=\[API_Key\]

18. El archivo .env final debe verse como en la imagen proporcionada.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

19. Una vez completado, seleccione **File (1)** y luego haga clic en
    **Save (2)** para guardar el archivo.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image12.png)

20. Seleccione la opción … **(1)** en el menú superior para expandir el
    menú. Seleccione **Terminal (2)** y haga clic en **New Terminal
    (3).**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

21. En la terminal de **VS Code**, ejecute el comando de inicio de
    sesión de Azure CLI:

+++az login+++

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image14.png)

22. En la ventana de **inicio de sesión**, seleccione **Work or school
    account** y haga clic en **Continue**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.png)

23. En la pestaña **Sign into Microsoft**, inicie sesión con las
    siguientes credenciales.

- Username - <+++@lab.CloudPortalCredential(User1).Username>+++

- TAP - +++@lab.CloudPortalCredential(User1).TAP+++

24. Cuando se le pregunte sobre las opciones de inicio de sesión,
    seleccione **No, this app only** para continuar sin vincular otras
    aplicaciones de escritorio.

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image16.png)

25. Escriba **1** y presione **Enter** en la opción **Select a
    subscription and tenant.**

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image17.png)

26. Una vez que la terminal esté abierta, ejecute el comando:

> +++pip install -r requirements.txt+++ para instalar todos los paquetes
> requeridos.

27. Ejecute el siguiente comando para probar el funcionamiento de la
    herramienta de búsqueda:

+++python main.py+++

> ![A screenshot of a computer screen AI-generated content may be
> incorrect.](./media/image18.png)

## Tarea 2: Visualizar las métricas del agente

En esta tarea, utilizará Azure Application Insights para visualizar los
datos de telemetría del agente. Explorará métricas personalizadas de
tiempo de respuesta, precisión de routing y éxito en la creación de
tickets. Luego, construirá dashboards interactivos en Azure Monitor para
mostrar indicadores clave de desempeño y tendencias. Esto ayuda a
identificar cuellos de botella, medir eficiencia y asegurar el
funcionamiento saludable de los agentes desplegados en tiempo real.

1.  Navegue al portal de Azure, abra su grupo de recurso y seleccione
    **agent-insights**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.png)

2.  En la página de Overview, podrá ver algunas métricas
    predeterminadas.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

3.  Desde el menú de la izquierda, seleccione **Search (1)** y haga clic
    en **See all data in last 24 hours (2)**.

![A screenshot of a search engine AI-generated content may be
incorrect.](./media/image21.png)

4.  Una vez abierto, desde la parte inferior, revise **Traces (1)** y
    haga clic en **View as individual items (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

5.  Podrá ver todos los detalles de comunicación que ocurrieron con el
    agente, así como todas las transacciones en el rango de tiempo
    seleccionado. También puede ajustar el rango de tiempo para explorar
    más.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

6.  Explore y revise estas transacciones; puede abrir una vista
    detallada haciendo clic sobre ellas. Revise cómo se muestran todos
    los detalles, como agentes, mensajes y detalles de recuperación.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

7.  A continuación, seleccione **Failures (1)**, luego **Review failed
    requests (2)** para obtener una vista centralizada de todas las
    ejecuciones fallidas e identificar las causas subyacentes mediante
    un análisis detallado de trazas.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

8.  Luego, seleccione **Performance (1)** y revise las operaciones y
    tiempos de respuesta **(2)**, para determinar el SLA de desempeño
    del agente.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

9.  Ahora, en Monitoring, desde el menú izquierdo, seleccione
    **Metrics**. Puede explorar las métricas personalizadas publicadas
    mediante spans.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.png)

10. En **Metric Namespace (1)**, seleccione azure.applicationinsights
    **(2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.png)

11. Bajo métricas, seleccione **gen_ai.client.operation.duration** y
    establezca la agregación en **avg (1)**. Revise el **gráfico de
    líneas (2)** para observar la métrica de **Response Time**, que
    indica el tiempo que el agente tarda en responder al usuario.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image30.png)

12. De manera similar, seleccione **gen_ai.client.token.usage** y
    establezca la agregación en **avg (1)**. Revise el **gráfico de
    líneas (2)** para observar el uso de tokens del agente.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

13. A continuación, seleccione **Logs (1)** desde el menú izquierdo y
    cierre el panel **Queries hub (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image32.png)

14. Una vez cerrado, haga clic en **Tables**, pase el cursor sobre el
    parámetro **customMetrics**, verá la opción **Run**, haga clic allí.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

![A close-up of a message AI-generated content may be
incorrect.](./media/image34.png)

15. Una vez que la consulta se ejecute correctamente, verá todas las
    métricas personalizadas listadas como resultados de la consulta.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image35.png)

16. Seleccione **Workbooks (1)** desde el menú izquierdo y haga clic en
    **Empty (2)** bajo **Quick start**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.png)

17. Una vez abierto, haga clic en **+ Add (1)** y seleccione **Add
    metric (2)**.

![A screenshot of a phone AI-generated content may be
incorrect.](./media/image37.png)

18. Cuando se abra el panel de métricas, haga clic en **Add metric**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

19. Seleccione **Metric** como **gen_ai.client.token.usage (1)**,
    proporcione **Display name** como **Token Usage (2)** y haga clic en
    **Save (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.png)

20. Nuevamente haga clic en **Add metric**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

21. Seleccione Metric como **gen_ai.client.operation.duration (1)**,
    proporcione Display name como **Response Time (2)** y haga clic en
    **Save (3)**.

![A screenshot of a screenshot of a metric settings AI-generated content
may be incorrect.](./media/image40.png)

22. Una vez seleccionadas ambas métricas, haga clic en **Run Metrics**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.png)

23. Cambie la visualización a **Area Chart** para obtener una
    visualización similar. Puede explorar muchas otras opciones de
    visualización y también el rango de tiempo.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.png)

24. Una vez completada la edición, haga clic en **Done editing**. Esto
    guardará la tarjeta en su **workbook**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.png)

25. Haga clic nuevamente en **+ Add (1)** y seleccione **Add query
    (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.png)

26. En el panel de consultas, agregue la siguiente consulta **(1)** y
    haga clic en **Run Query (2)**:

+++customMetrics+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.png)

27. Revise los resultados una vez que la consulta se ejecute
    correctamente. Después de revisar, haga clic en **Done Editing**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image46.png)

28. Una vez hecho, haga clic en **Done editing (1)** desde el menú
    superior y luego haga clic en el ícono **Save (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image48.png)

29. En el panel **Save As**, ingrese **Title** como **agent-workbook
    (1)** y haga clic en **Save As (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image49.png)

30. Dado que es un entorno de laboratorio, los datos disponibles pueden
    ser limitados para un monitoreo completo. Sin embargo, puede mejorar
    la visibilidad agregando métricas personalizadas de sus agentes y
    creando dashboards de monitoreo diseñados para objetivos
    específicos, tales como:

- **Agent Performance Dashboard**

> **Métricas mostradas:**

- Tiempos de respuesta de agentes (avg, P95)

- Tasa de éxito por tipo de agente.

- Tendencias de volumen de solicitudes.

- Alertas de tasa de errores.

> **Respuestas a preguntas de negocio:**

- ¿Qué agentes tienen mejor desempeño?

- ¿Se están cumpliendo los SLA?

- ¿Qué causa lentitud en el sistema?

&nbsp;

- **User Experience Dashboard**

> **Métricas mostradas:**

- Latencia de solicitudes de extremo a extremo.

- Tasas de creación de tickets.

- Éxito en recuperación de conocimiento.

- Métricas proxy de satisfacción del usuario.

> **Respuestas a preguntas de negocio:**

- ¿Los usuarios reciben respuestas rápidas?

- ¿Con qué frecuencia las solicitudes se convierten en tickets de
  soporte?

- ¿La base de conocimiento ayuda a los usuarios?

## Tarea 3: Monitorear métricas específicas del agente en el Foundry Portal

En esta tarea, utilizará Azure Application Insights para visualizar
datos de telemetría del agente. Explorará métricas personalizadas
específicas del agente desde el Microsoft Foundry Portal.

1.  Como ya conectó Application Insights al portal de Microsoft Foundry,
    puede regresar al portal y visualizar el funcionamiento de su
    agente.

2.  Regrese a su grupo de recurso y, desde la lista de recursos,
    seleccione el recurso **agent-foundry**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.png)

3.  En el siguiente panel, haga clic en **Go to Foundry portal**. Será
    dirigido al portal Microsoft Foundry, donde creará su primer agente.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image51.png)

4.  Antes de probar el agente, conecte Application Insights para
    habilitar logs detallados y visibilidad de trazas.

5.  En el portal Microsoft Foundry, seleccione **Monitoring (1)** desde
    el menú izquierdo, seleccione **agent-insights- (2)** y haga clic en
    **Connect (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image52.png)

6.  Ahora, navegue al panel **Monitoring**, donde conectó Application
    Insights previamente, y seleccione la pestaña **Resource usage**
    para revisar todas las métricas y valores.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image53.png)

7.  Seleccione **Tracing (1)** desde el menú izquierdo, haga clic en
    cualquiera de las **trazas (2)** y revise las trazas detalladas de
    las interacciones del agente.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image54.png)

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image55.png)

**Resumen**

En este laboratorio, configuró observabilidad y monitoreo para sus
agentes empresariales. Usando OpenTelemetry, capturó datos detallados de
ejecución para cada paso del flujo de trabajo y, al integrarlo con Azure
Application Insights, creó dashboards para visualizar métricas de
rendimiento y salud de los agentes.

Ha completado este laboratorio con éxito. Haga clic en **Next** \>\>
para continuar.
