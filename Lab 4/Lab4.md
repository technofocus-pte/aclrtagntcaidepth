# Laboratorio 4: Desarrollar un sistema multiagente para la generación de informes de planes de salud

**Descripción general**

En este laboratorio, desarrollará un sistema multiagente inteligente
diseñado específicamente para automatizar la generación de informes
completos de planes de salud. Este sistema aprovecha el poder
colaborativo de cuatro agentes de IA especializados que trabajan en
coordinación para recuperar, analizar, generar y validar documentación
detallada de seguros de salud.  
La arquitectura multiagente demuestra cómo los agentes autónomos pueden
trabajar juntos para realizar tareas complejas que serían difíciles de
manejar de manera efectiva por un solo agente.

Usted creará estos 4 agentes de IA:

- **Search Agent:** busca en un Azure AI Search index información sobre
  políticas específicas de planes de salud.

- **Report Agent:** genera un informe detallado sobre la póliza del plan
  de salud basado en la información proporcionada por el Search Agent.

- **Validation Agent:** valida que el informe generado cumpla con los
  requisitos especificados. En este caso, verificar que el informe
  incluya información sobre exclusiones de cobertura.

- **Orchestrator Agent:** actúa como orquestador y gestiona la
  comunicación entre los agentes Search, Report y Validation.

![A diagram of a company AI-generated content may be
incorrect.](./media/image1.png)

La orquestación es una parte clave de los sistemas multiagente, ya que
los agentes creados deben poder comunicarse entre sí para cumplir el
objetivo.

Usaremos **Azure AI Agent Service** para crear los agentes Search,
Report y Validation. Sin embargo, para crear el Orchestrator Agent
utilizaremos **Semantic Kernel**, que proporciona funcionalidad
integrada para la orquestación de sistemas multiagente.

**Objetivos del laboratorio**

Realizará las siguientes tareas en este laboratorio:

- Tarea 1: Crear el Azure AI Search Index.

- Tarea 2: Crear los agentes Search, Report y Validation.

## Tarea 1: Crear el Azure AI Search Index

En esta tarea, creará un **Azure AI Search index** para almacenar
representaciones vectorizadas de documentos de planes de seguro médico,
lo que permitirá una recuperación eficiente para búsquedas y análisis
impulsados por IA.

1.  Navegue al **Azure portal**, busque **AI Search (1)** y seleccione
    **AI Search (2)** en los servicios.

![](./media/image2.png)

2.  Esto lo llevará a **AI Foundry**. Dentro de **AI Search (1)**, haga
    clic en **Create (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

3.  En el panel **Create a Search service**, ingrese los siguientes
    detalles y haga clic en **Review + Create (4)**:

    - Subscription: **Mantenga la suscripción predeterminada**

    - Resource Group: Seleccione **AgenticAI (1)**

    - Service Name: **my-search-service- (2)**

    - Location : **(3)**

![](./media/image4.png)

4.  En **Review + Create**, haga clic en **Create.**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

5.  Espere hasta que la implementación se complete y luego haga clic en
    **Go to resource**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

6.  Navegue a **Keys (1)** bajo Settings en el menú izquierdo. En API
    Access control, seleccione **Both (2)**.

![](./media/image7.png)

7.  Seleccione **Yes** para confirmar que desea actualizar el **API
    Access Control** para este servicio de búsqueda.

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image8.png)

8.  Navegue a **Identity (1)** bajo **Settings**. En
    **System-assigned**, establezca **Status** en **On (2)** y haga clic
    en **Save (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.png)

9.  Seleccione **Yes** para habilitar la **identidad administrada
    asignada por el sistema**.

![A close-up of a computer error AI-generated content may be
incorrect.](./media/image10.png)

10. En el Azure portal, busque **Storage accounts (1)** y seleccione
    **Storage accounts (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

11. Seleccione la storage account que inicia con **aifoundry**.

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image12.png)

12. Seleccione **Access control (IAM) (1)**, haga clic en **Add (2)** y
    seleccione **Add role assignment**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

13. Bajo **Job function roles**, busque **Storage Blob Data Reader
    (1)**, selecciónelo **(2)** y luego seleccione **Next (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

14. En la página **Add role assignment**,

    - En Members, seleccione **Managed identity (1)**

    - Haga clic en **Select** **Members (2)**

    - Managed identity: **search service(1)** **(3)**

    - Luego seleccione **my-search-service- (4)**.

    - Haga clic en **Select (5)**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.png)

15. Haga clic en **Review + assign** dos veces.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image16.png)

16. Vaya a **Azure OpenAI**, recurso **my-openai-service**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.png)

17. Seleccione **Access control (IAM) (1)**, haga clic en **Add (2)** y
    seleccione **Add role assignment**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.png)

18. Bajo **Job function roles**, busque **Cognitive Services OpenAI User
    (1)**, selecciónelo **(2)** y haga clic en **Next (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.png)

19. En la página **Add role assignment**:

    - En Members, seleccione **Managed identity (1)**

    - Haga clic en **Select** **Members (2)**

    - Managed identity: **search service(1)** **(3)**

    - Seleccione **my-search-service- (4)**

    - Haga clic en **Select (5)**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

20. Seleccione **Review + assign** dos veces.

![](./media/image21.png)

21. Navegue al **Azure portal**, busque **Storage account (1)** y
    seleccione **Storage account (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

22. Seleccione la **Storage account** que inicia con **aifoundryhub**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

23. Haga clic en **Containers (1)** bajo **Data storage** y luego
    seleccione **+Container (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

24. En **New Container**, ingrese **healthplan (1)** como nombre y haga
    clic en **Create (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

25. Abra el contenedor **healthplan** haciendo clic en él.

![](./media/image26.png)

26. Haga clic en **Upload (1)** y luego en **Browse for files (2)**.

> ![](./media/image27.png)

27. Navegue a **C:\LabFiles\Day-1\azure-ai-agents-labs\data (1)**,
    seleccione ambos archivos PDF (2) y haga clic en **Open (3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.png)

28. Haga clic en **Upload**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.png)

**Nota:** Si se le solicita seleccionar un contenedor existente, elija
**healthplan**.

29. Navegue al **Azure AI search service** y seleccione
    **my-search-service-**.

![](./media/image30.png)

30. Haga clic en **import data (new)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

31. Seleccione **azure blob storage**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image32.png)

32. Seleccione **RAG Model**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

33. En **Configure your Azure Blob Storage**, ingrese lo siguiente y
    luego haga clic en **Next (5)**:

[TABLE]

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image34.png)

34. En **Vectorize your text**, ingrese lo siguiente y luego haga clic
    en **Next (7)**:

[TABLE]

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image35.png)

35. Haga clic en **Next** dos veces.

36. Ingrese **health-plan (1)** para **Objects name prefix** y haga clic
    en **Create (2)**.

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image36.png)

**Note**: La carga en el search index puede tardar 5–10 minutos.

37. Haga clic en **Start searching** en la ventana emergente.

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image37.png)

38. Navegue a la página **Overview (1)** de **ai-foundry-project-** y
    haga clic en **Open In management center (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

39. Seleccione **Connected resources (1)** y haga clic en **New
    connection (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.png)

40. Ingrese **Azure AI Search (1)** en la barra de búsqueda y seleccione
    **Azure AI Search (2)**.

![](./media/image40.png)

41. Haga clic en **Add connection** para continuar.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.png)

## Tarea 2: Crear los agentes Search, Report y Validation

En esta tarea, usted creará los agentes Search, Report y Validation para
recuperar, generar y validar reportes de planes de salud. Estos agentes
trabajarán juntos para garantizar la precisión y el cumplimiento de los
requisitos. Cada agente desempeña una función específica en la
recuperación, compilación y verificación de la exactitud de los
reportes.

1.  Abra el archivo **Lab 4 - Develop a Multi-Agent System.ipynb**. Este
    notebook lo guiará en el desarrollo de un sistema multiagente con
    los agentes Search, Report, Validation y Orchestrator, diseñados
    para generar y validar informes de planes de salud.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.png)

2.  Seleccione la opción **Select kernel (1)** y luego **venv (Python
    3.x.x) (2)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.png)

3.  Ejecute esta celda para desarrollar un sistema multiagente que
    integra **Azure AI Search, GPT-4o** y **Semantic Kernel** para
    ejecución inteligente de tareas. Esta configuración habilita la
    colaboración entre agentes para recuperar información, generar
    respuestas y manejar consultas complejas.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image44.png)

4.  Ejecute esta celda para crear el **Search Agent**, el cual recupera
    información sobre planes de salud desde **Azure AI Search**
    utilizando **GPT-4o**. Este agente permite realizar búsquedas
    eficientes y obtener información estructurada a partir de documentos
    de planes de salud.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.png)

5.  Ejecute esta celda para crear el **Report Agent**, que genera
    informes detallados sobre los planes de salud utilizando GPT-4o.
    Este agente mejora la documentación al proporcionar información
    estructurada, detalles de cobertura y exclusiones para los distintos
    planes.

![](./media/image46.png)

6.  Ejecute esta celda para crear el **Validation Agent**, el cual
    garantiza que los informes generados por el Report Agent cumplan con
    los estándares de calidad, verificando específicamente las
    exclusiones de cobertura.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.png)

7.  **Cree el sistema multiagente:** al ejecutar la celda, verá una caja
    de texto en la parte superior de VS Code solicitando el nombre de un
    plan de salud.

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image48.png)

8.  Como recordará, subimos dos planes de salud al índice de búsqueda.
    Cuando se le solicite, escriba uno de los siguientes nombres y
    presione **Enter** para ejecutar el sistema multiagente:

    - **Northwind Health Standard**

    - **Northwind Health Plus**1

![](./media/image49.png)

9.  Cuando aparezca nuevamente la caja en la parte superior, escriba
    **exit** y presione **Enter** para detener el bloque de código.

**Nota**: Tras una ejecución exitosa, verá el siguiente resultado:

> Orchestrator Agent is starting...
>
> Calling SearchAgent...
>
> SearchAgent completed successfully.
>
> Calling ReportAgent...
>
> ReportAgent completed successfully.
>
> Calling ValidationAgent...
>
> ValidationAgent completed successfully.
>
> The report for Northwind Plus has been generated. Please check the
> Northwind Plus Report.md file for the report.
>
> Orchestrator Agent is starting...

**Resumen**

En este laboratorio, desarrolló exitosamente un sistema inteligente
multiagente diseñado para automatizar la generación de reportes
completos de planes de salud mediante la coordinación de cuatro agentes
especializados.

Creó un índice de Azure AI Search, construyó un Search Agent para
recuperar información, un Report Agent para generar documentación
detallada, un Validation Agent para asegurar el cumplimiento de
requisitos y un Orchestrator Agent basado en Semantic Kernel para
gestionar la comunicación entre todos ellos.

Al ejecutar el sistema con datos reales de planes de salud, demostró
cómo los agentes autónomos pueden colaborar eficazmente para completar
tareas complejas que serían difíciles para un solo agente, mostrando
patrones de orquestación empresariales aplicables a escenarios reales.

**¡Felicidades! Ha completado correctamente el laboratorio.**
