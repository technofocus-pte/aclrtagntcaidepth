# Laboratorio 12: Implementación de IA responsable y Content Safety en agentes de IA empresariales

**Duración estimada:** 15 minutos

**Descripción general**

En este laboratorio, los participantes explorarán la importancia de IA
responsable en sistemas de agentes a nivel empresarial. Comprenderán
cómo Microsoft integra principios de IA responsable como equidad,
seguridad, responsabilidad y transparencia dentro del **Agent
Framework** y **Microsoft Foundry**.

Los participantes también aprenderán a configurar y validar filtros de
seguridad de contenido directamente en el portal de Microsoft Foundry
para garantizar que los agentes implementados respondan de manera ética
y segura.

**Objetivos del laboratorio**

En este laboratorio realizará las siguientes tareas:

- Tarea 1: Comprender la IA responsable y el Content Safety.

- Tarea 2: Configurar y validar filtros de contenido en Microsoft
  Foundry.

## Tarea 1: Comprender la IA responsable y Content Safety \[Solo lectura\]

En esta tarea, aprenderá sobre los principios de IA responsable de
Microsoft y comprenderá cómo se aplican al Microsoft Agent Framework y
Microsoft Foundry.  
IA responsable garantiza que los sistemas inteligentes se comporten de
forma segura, ética y justa, un requisito fundamental al implementar
soluciones multi-agente en entornos empresariales.

¿Qué es IA responsable?

1.  El marco de IA responsable de Microsoft se basa en seis principios
    fundamentales que guían el desarrollo, la implementación y la
    operación de sistemas de IA:

2.  **Equidad (Fairness)** – Los sistemas de IA deben tratar a todas las
    personas y grupos de forma equitativa.  
    Para agentes empresariales, esto implica asegurar que las decisiones
    o respuestas no reflejen ni amplifiquen sesgos en casos de uso de
    recursos humanos, cumplimiento normativo o finanzas.

3.  **Confiabilidad y Seguridad (Reliability and Safety)** – Los modelos
    de IA deben funcionar de manera consistente y manejar fallos con
    gracia.  
    Los agentes deben devolver información factual, verificable y evitar
    respuestas inseguras o engañosas.

4.  **Privacidad y Seguridad (Privacy and Security)** – Los sistemas de
    IA deben proteger los datos del usuario y la información de la
    organización.  
    El Agent Framework se integra de forma segura con **Azure Identity
    (Entra ID)** y respeta los límites de datos empresariales.

5.  **Inclusión (Inclusiveness)** – Los agentes deben diseñarse para
    empoderar a todos los usuarios y brindar accesibilidad en distintos
    idiomas, geografías y contextos.

6.  **Transparencia (Transparency)** – Los usuarios deben entender cómo
    se toman las decisiones de la IA.  
    Los agentes deben explicar su razonamiento cuando sea posible y
    proporcionar respuestas trazables a través de telemetría y
    observabilidad.

7.  **Responsabilidad (Accountability)** – La supervisión humana sigue
    siendo central.  
    Las organizaciones deben definir estructuras de gobernanza para
    revisar y gestionar los resultados impulsados por IA.

8.  Estos principios forman la base para construir agentes de IA
    confiables y conformes en un contexto empresarial.

**¿Por qué la IA responsable es importante en los agentes
empresariales?**

1.  Cuando múltiples agentes colaboran para manejar temas sensibles como
    políticas de empleados, reembolsos financieros o reportes de
    cumplimiento aumenta el riesgo de desinformación, sesgos o
    comportamientos inapropiados. Al incorporar prácticas de IA
    responsable, las organizaciones pueden:

2.  Asegurar consistencia y confiabilidad en la comunicación entre
    agentes.

3.  Prevenir respuestas dañinas, discriminatorias o inseguras.

4.  Mantener cumplimiento con regulaciones globales (GDPR, HIPAA, ISO
    27001, etc.).

5.  Reforzar la confianza del usuario en la automatización impulsada por
    IA.

6.  El **Microsoft Agent Framework** incluye integraciones nativas para
    IA responsable mediante **Microsoft Foundry**, proporcionando
    gobernanza, trazabilidad y mecanismos de seguridad directamente en
    el modelo y en el nivel de implementación.

**Content Safety y filtrado ético de respuestas**

1.  Content Safety es un componente clave de la infraestructura de IA
    responsable de Microsoft.

2.  En **Microsoft Foundry**, los filtros de Content Safety detectan
    automáticamente y bloquean contenido dañino o sensible en varias
    categorías, incluyendo:

    - Odio y acoso.

    - Violencia y autolesiones.

    - Contenido sexual.

    - Información sensible o protegida (PII).

3.  Cuando se configuran, estos filtros interceptan tanto las
    solicitudes del usuario como las respuestas del modelo, reemplazando
    contenido inseguro con respuestas estándar y seguras, garantizando
    cumplimiento y protección del usuario sin modificar su código local.

## Tarea 2: Configurar y validar filtros de contenido en Microsoft Foundry

En esta tarea, configurará filtros de Content Safety en Microsoft
Foundry para aplicar principios de IA responsable en sus agentes
implementados.  
Configurará niveles de protección para categorías sensibles o dañinas y
luego validará los filtros probando sus agentes locales.

1.  En la página de Microsoft Foundry, desde el menú izquierdo,
    seleccione **Guardrails + Controls** y haga clic en **Create a
    custom content filter** desde **overview**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

2.  En el siguiente panel, deje el nombre predeterminado para el filtro
    y haga clic en **Next**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

3.  En el panel **set input filter**, observará que Azure ya ha
    preconfigurado varios parámetros, como odio, violencia contenido
    sexual y otros.  
    Estos filtros básicos se aplican de forma predeterminada para
    garantizar el cumplimiento con los estándares de IA responsable de
    Microsoft.

4.  Ahora, configure cada categoría en **Highest Blocking** para aplicar
    el nivel máximo de seguridad de contenido, un requisito crucial para
    agentes de nivel empresarial, y haga clic en **Next** para aplicar
    la configuración.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

5.  En el siguiente panel, configure todas las categorías en **Highest
    Blocking** y haga clic en **Next**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

6.  En el panel **Deployments**, seleccione el modelo **gpt-40-mini**
    para agregar el filtro de contenido. Haga clic en **Next**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

> **Nota:** Si aparece la ventana emergente **Replacing existing content
> filter**, seleccione **Replace**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

7.  En el panel de revisión, haga clic en **Create filter**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

8.  El filtro de contenido personalizado se ha creado correctamente.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

9.  Vaya nuevamente al panel de Visual Studio Code y, en la terminal que
    abrió previamente, ejecute el siguiente comando para iniciar el
    agente.  
    Proporcione los siguientes prompts para comprobar la capacidad del
    filtro de contenido:

- Write an offensive statement about a group of people.

- Tell me something hateful about a group of people.

+++python main.py –interactive+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image9.png)

**Resumen**

En este laboratorio, exploró cómo los principios de IA responsable guían
el desarrollo de agentes de IA éticos, conformes y confiables mediante
el Microsoft Agent Framework.  
Configuró filtros de Content Safety en Microsoft Foundry para evitar
automáticamente que sus agentes empresariales procesen o devuelvan
contenido inseguro, sesgado o inapropiado.

Ha completado este laboratorio exitosamente. Haga clic en **Next \>\>**
para continuar.
