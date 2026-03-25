# Laboratorio 2: Configurar un AI Project y realizar un Chat Completion desde VS Code

**Descripción general**

En este laboratorio, preparará el entorno de desarrollo completo
necesario para crear agentes de IA mediante la creación y configuración
de un AI Project en Microsoft Foundry, la implementación de un Large
Language Model (LLM) y un modelo de embeddings, y la conexión del
proyecto con Visual Studio Code. Luego validará la configuración
ejecutando un chat completion desde código, asegurando que su entorno
está correctamente configurado y listo para desarrollar aplicaciones
basadas en IA.

Objetivos del laboratorio

Realizará las siguientes tareas en este laboratorio:

- Tarea 1: Configurar el AI Project en Microsoft Foundry.

- Tarea 2: Implementar un LLM y modelos de embeddings.

- Tarea 3: Instalar dependencias, crear un entorno virtual y crear un
  archivo de variables de entorno.

## Tarea 1: Configurar el AI Project en Microsoft Foundry

En esta tarea, creará y configurará un **AI Project** dentro de
**Microsoft Foundry**. Esto implica configurar los recursos necesarios,
definir parámetros del proyecto y asegurar que el entorno esté listo
para implementar modelos de IA. Al finalizar esta tarea, tendrá un AI
Project completamente inicializado, que servirá como base para el
desarrollo y experimentación posterior.

1.  En la página del Azure Portal, en el cuadro Search resources en la
    parte superior del portal, ingrese **Microsoft Foundry (1)** y luego
    seleccione **Microsoft Foundry (2)** en Services.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

2.  En el panel de navegación izquierdo para **Use with Foundry**,
    seleccione **AI Hubs** **(1)**. En la página AI Hubs, haga clic en
    **Create** **(2)** y seleccione **Hub** **(3)** en el menú
    desplegable.

![](./media/image2.png)

3.  En el panel **Create an Azure AI hub**, ingrese los siguientes
    detalles en **Basics** **(1)**:

    - Subscription: **Deje la suscripción predeterminada (2)**

    - Resource Group : **AgenticAI** **(3)**

    - Region : **East US2** (4)

    - Name : ** <+++ai-foundry-hub@lab.LabInstance.Id>+++ (5)**

    - Connect AI Services incl. OpenAI: Haga clic en **Create
      New** **(6)**.

    - Connect AI Services incl. OpenAI: Proporcione el
      nombre **<+++my-ai-service@lab.LabInstance.Id>+++ (7)**.

    - Haga clic en **Save** **(8)**, seguido de **Next:Storage** **(9)**

> ![](./media/image3.png)

4.  Haga clic en la pestaña **Review + Create**, seguido de **Create.**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

![](./media/image5.png)

5.  Espere a que la implementación se complete y luego haga clic en **Go
    to resource**.

![](./media/image6.png)

6.  En el panel **Overview**, haga clic en **Launch Azure AI Foundry**.
    Esto lo llevará al portal de **Microsoft Foundry**.

![](./media/image7.png)

7.  Desplácese hacia abajo y haga clic en **+ New project** en el Hub
    Overview.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

8.  Proporcione el nombre del
    proyecto **<+++ai-foundry-project@lab.LabInstance.Id>+++,** luego
    haga clic en **Create (2)**.

![](./media/image9.png)

9.  Una vez creado el proyecto, desplácese hacia abajo y copie el
    **Project connection string**, luego péguelo en un **bloc de notas**
    o en un lugar seguro, ya que será necesario en tareas posteriores.

![A screenshot of a project AI-generated content may be
incorrect.](./media/image10.png)

## Tarea 2: Implementar un LLM y modelos de embeddings

En esta tarea, implementará un Large Language Model (LLM) y un modelo de
embeddings dentro de su proyecto en Microsoft Foundry. Estos modelos se
utilizarán en aplicaciones impulsadas por IA y en funcionalidades de
búsqueda vectorial en los laboratorios posteriores.

1.  En su proyecto de Microsoft Foundry, navegue a la sección **My
    assets** **(1)**, luego seleccione **Models + endpoints** **(2)**.
    Haga clic en **Deploy model** **(3)** y seleccione **Deploy base
    model** **(4)**.

![](./media/image11.png)

2.  En la ventana **Select a model**, busque **gpt-4o** **(1),**
    seleccione **gpt-4o** **(2)** y haga clic en **Confirm** **(3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.png)

3.  En la ventana **Deploy model gpt-4o**, seleccione **Customize**.

![](./media/image13.png)

- Deployment Name: **gpt-4o** **(1)**

- Deployment type: **Global Standard** **(2)**

- **Model version**: Cambie a **2024-08-06 (Default) (3)**

- **Tokens per Minute Rate Limit**: 200K (4)

- Haga clic en **Connect and Deploy** (5)

![](./media/image14.png)

4.  Haga clic en **Model + Endpoints** **(1)** donde podrá ver el modelo
    **gpt-4o** **(2)** implementado.

![](./media/image15.png)

5.  Regrese al **Azure Portal**, busque **Open AI** **(1)** y seleccione
    **Azure Open AI** **(2)**.

![](./media/image16.png)

6.  En la página **Microsoft Foundry | Azure OpenAI**, seleccione **+
    Create** **(1)** y luego **Azure OpenAI** **(2)**.

![](./media/image17.png)

7.  En la página **Create Azure OpenAI**, proporcione las siguientes
    configuraciones y haga clic en **Next** (6):

[TABLE]

> ![](./media/image18.png)

8.  Haga clic en **Next** hasta que aparezca la pestaña **Review +
    submit**.

9.  En **Review + submit**, haga clic en **Create.**

![](./media/image19.png)

10. Espere a que la implementación sea exitosa y seleccione **Go to
    resource**.

![](./media/image20.png)

11. En la página del recurso **my-openai-service**, seleccione **Go to
    Foundry portal**.

![](./media/image21.png)

12. En su proyecto de **AI Foundry**, navegue a la sección **Shared
    resources**, luego seleccione **Deployments** **(1)**. Haga clic en
    **Deploy model** **(2)** y seleccione **Deploy base model** **(3)**.

![](./media/image22.png)

**Nota:  
**El asistente de importar y vectorizar en Azure AI Search, que se
utilizará en los próximos laboratorios, aún no soporta modelos de
embeddings dentro del AI Foundry Project. Por ello, debemos crear un
servicio de Azure OpenAI e implementar allí un modelo de text
embeddings. Este modelo se utilizará más adelante para crear nuestro
índice vectorial.

13. En la ventana **Select a model**, busque **text-embedding-3-large**
    **(1)**, luego seleccione **text-embedding-3-large** **(2)** y haga
    clic en **Confirm** **(3).**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

14. En **Deploy model text-embedding-3-large**,

    - Deployment type: **Standard (1).**

    - Tokens per Minutes Rate Limit: **120K (2).**

    - Seleccione **Deploy (3).**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

15. Haga clic en **Deployment** **(1),** donde podrá ver el modelo
    **text-embedding-3-large** **(2)** implementado.

![](./media/image25.png)

## Tarea 3: Instalar dependencias, crear un entorno virtual y crear un archivo de variables de entorno

En esta tarea, instalará las dependencias necesarias, configurará un
**virtual environment**, y creará el archivo de variables de entorno.
Esto garantiza un entorno controlado y la gestión segura de
configuraciones para su AI Project.

1.  En su **Lab VM**, inicie **Visual Studio Code**.

2.  Haga clic en **File** **(1)**, luego **Open Folder**.

![](./media/image26.png)

3.  Navegue a **C:\LabFiles\Day-1** **(1)**, seleccione la carpeta
    **azure-ai-agents-labs** **(2)** y haga clic en **Select folder**
    **(3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

4.  Haga clic en **Yes, I Trust the authors**.

![](./media/image28.png)

5.  Haga clic en las **elipses (...)** **(1)**, luego en la **Terminal**
    **(2)** y finalmente en **New Terminal** **(3)**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.png)

6.  Asegúrese de estar en el directorio del proyecto
    **azure-ai-agents-labs**. Ejecute los siguientes comandos de
    PowerShell para crear y activar el entorno virtual:

7.  python -m venv venv

+++venv/Scripts/activate+++

![](./media/image30.png)

8.  Ejecute el siguiente comando de PowerShell para instalar los
    paquetes necesarios:

9.  pip install -r requirements.txt

+++pip install azure-ai-ml azure-identity+++

![A screen shot of a computer code AI-generated content may be
incorrect.](./media/image31.png)

10. Ejecute el siguiente comando de PowerShell para instalar o
    actualizar pip a la última versión:

+++python.exe -m pip install --upgrade pip+++

![](./media/image32.png)

11. Ejecute el siguiente comando para iniciar sesión en su cuenta de
    Azure:

+++az login+++

12. Seleccione la cuenta de usuario para autorizar.

13. Una vez completada la autorización, regrese a **Visual Studio
    Code**.

![](./media/image33.png)

14. Abra el archivo **Sample.env** y proporcione las variables de
    entorno necesarias.

![](./media/image34.png)

- Navegue al portal de **Microsoft Foundry**, haga clic en el modelo
  **gpt-4o** **(2)** desde la sección **Models + endpoints** **(1)**
  bajo **My assets**, copie el **Endpoint** del panel derecho y luego
  copie y pegue el **Target URI (1)** y la **Key (2)** en un bloc de
  notas.

![](./media/image35.png)

![](./media/image36.png)

15. En el archivo **Sample.env**:

    - AIPROJECT_CONNECTION_STRING: Proporcione el valor del **Project
      connection string** copiado en el **Paso 9 de la Tarea 1**.

    - CHAT_MODEL_ENDPOINT: Proporcione el **Target URI** del modelo
      **gpt-4o** copiado en el paso anterior.

    - CHAT_MODEL_API_KEY: Proporcione la **Key copiada** en el paso
      anterior.

    - CHAT_MODEL: **gpt-4o**

![](./media/image37.png)

16. Guarde los cambios en **Sample.env**.

17. Ejecute el siguiente comando de PowerShell para crear su archivo
    **.env**:

+++cp sample.env .env+++

![](./media/image38.png)

18. Luego, abra el archivo **Lab 1 - Project Setup.ipynb**. Este
    notebook lo guía en la configuración del AI Project en Microsoft
    Foundry, la implementación del LLM y del embedding model, y la
    configuración de la conectividad de VS Code. También incluye una
    llamada simple a la API de Chat Completion para verificar la
    configuración.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image39.png)

19. Seleccione **Select kernel** **(1)** en la esquina superior derecha
    y seleccione **Install/enable selected extensions (python+jupyter)**
    **(2).**

![](./media/image40.png)

20. Seleccione **Python Environments** para asegurarse de que el Jupyter
    Notebook se ejecute en el intérprete de Python correcto con las
    dependencias instaladas.

![](./media/image41.png)

21. Seleccione **venv (Python 3.x.x)** de la lista, ya que esta versión
    probablemente es necesaria para la compatibilidad con el SDK de
    Microsoft Foundry.

![](./media/image42.png)

**Nota:** Si **venv (Python 3.x.x**) no aparece en la lista, cierre y
vuelva a abrir **Visual Studio Code.**

22. Ejecute la primera celda para importar las librerías necesarias de
    Python para trabajar con Azure AI Services.

![](./media/image43.png)

23. Ejecute la siguiente celda para recuperar el **project connection
    string** y el nombre del modelo desde las variables de entorno.
    Estos valores son necesarios para interactuar con el LLM de manera
    segura sin exponer información sensible.

![](./media/image44.png)

24. Ejecute la siguiente celda para conectarse a su proyecto de
    Microsoft Foundry utilizando el connection string. Esto establece
    una conexión segura con **AIProjectClient**.

![](./media/image45.png)

25. Ejecute la siguiente celda para interactuar con el modelo **GPT-4o**
    en su proyecto Microsoft Foundry. Este código inicializa un chat
    client, envía una solicitud para un chiste sobre un oso de peluche y
    muestra la respuesta.

![](./media/image46.png)

> **Nota:** El resultado de este paso se genera dinámicamente por el
> modelo de IA y puede variar cada vez.

**Resumen**

En este laboratorio, configuró un entorno completo de desarrollo de IA
mediante la creación y configuración de un AI Project en Microsoft
Foundry, la implementación del modelo GPT-4o y del modelo de
text-embedding-3-large, y la conexión segura desde Visual Studio Code.
Instaló las dependencias requeridas, creó un entorno virtual y configuró
un archivo de variables de entorno para manejar la información sensible
de forma segura.

Finalmente, validó la configuración ejecutando una llamada simple a la
API de Chat Completion, confirmando que su entorno está listo para
desarrollar aplicaciones impulsadas por IA.

Ha completado este laboratorio correctamente. Haga clic en **Next \>\>**
para continuar.
