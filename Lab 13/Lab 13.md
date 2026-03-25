# Laboratorio 13: Implementación de detección de fraude empresarial con IA y participación humana

**Duración Estimada:** 60 minutos

**Descripción general**

Usted es un ingeniero de IA en Contoso Ltd., responsable de implementar
flujos de trabajo de IA con participación humana (HITL). En este
laboratorio, explorará el flujo de trabajo de detección y respuesta de
fraude de Contoso, donde los agentes de IA analizan actividades
sospechosas y envían acciones de alto riesgo a analistas humanos para su
revisión, utilizando un panel de control en tiempo real React + FastAPI
para monitoreo e interacción.

**Objetivo del laboratorio**

Realizará las siguientes tareas en este laboratorio:

- Tarea 1: Implementación de flujos de trabajo de IA con participación
  humana con Azure Agent Framework.

## Tarea 0: Configuración del código 

1.  Desde C:\Labfiles\Day 3, extraiga el archivo
    **OpenAIWorkshop-Framework**.

2.  Haga clic en **Visual Studio Code** desde el escritorio de LabVM.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

3.  Seleccione **File (1)** y haga clic en **Open Folder (2)** para
    abrir la carpeta OpenAIWorkshop-Framework.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

4.  Navegue a C:\Labfiles\Day 3\\**OpenAIWorkshop-Framework**,
    seleccione **OpenAIWorkshop**-Framework y luego **Select Folder**.

5.  Seleccione **Yes, I trust the authors**.

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image3.png)

6.  Haga clic en los puntos suspensivos **(...) (1)**, luego en
    **Terminal (2)** y después en **New Terminal (3)**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

7.  Ingrese el siguiente comando para navegar al directorio de
    **aplicaciones** e instale todas las dependencias requeridas desde
    **pyproject.toml / uv.lock**:

> cd agentic_ai/applications
>
> uv sync

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

**Nota:** Si encuentra algún error, ejecute los siguientes comandos.

> +++pip install uv+++

+++uv sync+++

8.  El comando puede tardar de 5 a 10 minutos en completarse. **Mientras
    tanto, puede continuar con la tarea 1**.

## Tarea 1: Implementación de flujos de trabajo de IA con participación humana con Azure Agent Framework

En este laboratorio, implementará un flujo de trabajo de IA con
participación humana (HITL) para el sistema de detección de fraude de
Contoso. Ejecutará detección de fraude con múltiples agentes, revisará
alertas de alto riesgo, tomará decisiones humanas y visualizará el flujo
de trabajo en tiempo real usando el panel de control React + FastAPI.

1.  Desde Visual Studio Code, expanda **agentic_ai (1) \> workflow (2)
    \> fraud_detection (3)**, seleccione **fraud_detection_workflow.py
    (4)**. Visualice el código **(5)**.

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image6.png)

2.  Dentro de **fraud_detection (1)**, haga clic derecho en
    **.env.sample (2)** y luego seleccione **Rename (3)**.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image7.png)

3.  Renombre el archivo como **.env** y ábralo haciendo clic sobre él.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image8.png)

4.  Reemplace el valor de AZURE_OPENAI_API_KEY **(1)** y
    AZURE_OPENAI_ENDPOINT **(2)** con los valores reales que copió en el
    laboratorio anterior.

5.  Agregue AZURE_OPENAI_CHAT_DEPLOYMENT como **gpt-4o-mini (3).**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.png)

- Navegue al **Microsoft Foundry portal**, seleccione **Overview (1)**,
  luego seleccione **Azure OpenAI (2)**. Copie la **Azure OpenAI key
  (3)** y **Azure OpenAI endpoint (4)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.png)

6.  Seleccione **File (1)** y luego **Save (2)**.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

7.  En la ventana de Visual Studio Code, haga clic en los puntos
    suspensivos (...) **(1)**, luego en la **Terminal (2)** y después en
    **New Terminal (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

8.  Ejecute el siguiente comando:

> cd mcp
>
> uv run python mcp_service.py

9.  Deje que el comando se ejecute, abra una nueva terminal.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

10. Ingrese el comando a continuación para ejecutar el flujo de trabajo
    desde la línea de comandos:

> cd agentic_ai/workflow/fraud_detection
>
> uv run python fraud_detection_workflow.py
>
> ![A black screen with white text AI-generated content may be
> incorrect.](./media/image12.png)

**Nota:** El comando puede tardar de 5 a 10 minutos en completarse.
Espere hasta que finalice.

11. El ejemplo incluye tres alertas de muestra:

    - **Alerta 1: Multi-Country Login** (Alta severidad)

    - alert_id: "ALERT-001"

    - customer_id: 1

    - alert_type: "multi_country_login"

    - description: "Login attempts from USA and Russia within 2 hours."

severity: "high"

- **Alerta 2: Data Spike** (Severidad media)

- alert_id: "ALERT-002"

- customer_id: 2

- alert_type: "data_spike"

- description: "Data usage increased by 500% in the last 24 hours."

severity: "medium"

- **Alerta 3: Unusual Charges** (Alta severidad)

- alert_id: "ALERT-003"

- customer_id: 3

- alert_type: "unusual_charges"

- description: "Three large purchases totaling $5,000 in 10 minutes."

severity: "high"

12. Una vez que la ejecución sea exitosa, podrá ver la terminal como se
    muestra a continuación. Seleccione la acción según la severidad del
    riesgo. Si la severidad del riesgo ≥ 0,6, se requiere revisión
    humana.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

13. Como la severidad del riesgo es alta, puede ingresar 2 para **lock
    the customer account** **(1)**

    - Ingrese Enter analyst notes: High risk confirmed from all three
      analyses. Immediate action: locking account to prevent
      unauthorized access. **(2)**

    - En Enter analyst ID (default: analyst_cli):
      Presione **Enter** **(3)**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

14. Una vez que el flujo de trabajo se complete, recibirá una salida
    como esta.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.png)

15. Una vez que el comando se haya ejecutado correctamente, **elimine
    todas las sesiones de terminal existentes**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image16.png)

## Interfaz de usuario del visualizador en tiempo real del flujo de detección y respuesta de fraude de Contoso

Usará la interfaz de usuario del visualizador de flujo de trabajo en
tiempo real para monitorear e interactuar con el flujo de detección y
respuesta de fraude de Contoso. Iniciará todos los servicios (servidor
MCP, backend y frontend), seleccionará alertas de muestra, observará la
ejecución del flujo de trabajo en vivo, revisará alertas de fraude de
alto riesgo, enviará decisiones de los analistas y monitoreará los event
streams en tiempo real.

1.  Abra una nueva terminal.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

2.  Inicie todos los servicios (3 terminales):

    - Terminal 1 - MCP Server:

> cd mcp
>
> uv run mcp_service.py

- Terminal 2 - FastAPI Backend:

> cd agentic_ai/workflow/fraud_detection
>
> uv run --prerelease allow backend.py
>
> ![A screen shot of a computer program AI-generated content may be
> incorrect.](./media/image17.png)

- Terminal 3 - React Frontend:

> cd agentic_ai/workflow/fraud_detection/ui
>
> npm run dev
>
> **Nota:** Si obtiene algún error, ejecute el comando +++npm install+++
> y luego vuelva a ejecutar +++npm run dev+++.
>
> ![A computer screen with white text AI-generated content may be
> incorrect.](./media/image18.png)

- **ctrl + clic** en **<http://localhost:3000>** para abrir la
  aplicación en un navegador.

> ![A screen shot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

3.  Explore la interfaz de usuario del visualizador en tiempo real del
    flujo de trabajo.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

4.  Puede consultar alertas de muestra en el menú desplegable **Select
    Alerts**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image21.png)

**Nota:** Podrá consultar las alertas en el menú desplegable **Select
Alerts** solo después de que la conexión esté abierta en la segunda
terminal (backend.py). Asegúrese de que la conexión esté **activa.**

5.  **Seleccionar alerta:** elija entre 3 alertas de muestra: ALERT-001,
    ALERT-002, ALERT-003 **(1)**

    - Haga clic en **Start Workflow** para comenzar el procesamiento.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

6.  **Observar actualizaciones en tiempo real:** los nodos cambian de
    color a medida que los ejecutores procesan las tareas.

    - 🔵 En ejecución (Running)

    - 🟢 Completado (Completed)

    - ⚪ Inactivo (Idle)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

7.  **Revisión del analista:** cuando se detecta un fraude de alto
    riesgo, aparece un panel de revisión.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

8.  **Enviar decisión:** seleccione la acción correspondiente y agregue
    notas.

    - Your Decision: Si la severidad es alta, seleccione **Lock Account
      (1).**

    - Analyst Notes: Ingrese: High risk confirmed from all three
      analyses. Immediate action: locking account to prevent
      unauthorized access. **(2)**

    - Seleccione **SUBMIT WORKFLOW (3)**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

9.  **Monitorear eventos:** El panel derecho muestra el flujo completo
    de eventos (complete event stream).

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

**Resumen  
  
**En este laboratorio, se implementó un flujo de trabajo de IA con
participación humana (Human-in-the-Loop, HITL) para la detección de
fraude mediante el Azure Agent Framework. Se mostró cómo los agentes de
IA analizan actividades sospechosas, envían los casos de alto riesgo a
analistas humanos e interactúan con un panel de control en tiempo real,
basado en React y FastAPI, con el fin de monitorear la ejecución del
flujo de trabajo y registrar las decisiones.
