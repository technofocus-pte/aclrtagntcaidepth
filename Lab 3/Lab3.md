# Laboratorio 3: Crear un agente analizador de planes de seguro médico

**Descripción general**

En este laboratorio, creará un **agente analizador de planes de seguro
médico** diseñado específicamente para procesar y analizar datos de
planes de salud. Este agente inteligente automatiza la comparación de
diferentes planes de beneficios de salud interpretando los detalles de
los planes, analizando opciones de cobertura y generando
representaciones visuales para apoyar la toma de decisiones.

Usando **Microsoft Foundry** y **Azure AI services**, el agente creará
gráficos de barras comparativos que muestran claramente las diferencias
entre los planes de seguro, facilitando que los usuarios comprendan sus
opciones y elijan la cobertura de salud más adecuada.

**Objetivos del laboratorio**

Realizará la siguiente tarea en este laboratorio:

- Tarea 1: Crear un agente de IA Simple

## Tarea 1: Crear un agente de IA Simple

En esta tarea, construirá un **agente de IA simple** que procesa datos y
genera un gráfico de barras comparando diferentes planes de beneficios
de salud, utilizando Azure AI services para análisis y visualización.

1.  Abra el archivo **Lab 2 - Create A Simple AI Agent.ipynb**. Este
    notebook lo guiará en la creacción de un agente de IA simple que
    procesa datos y genera un gráfico de barras comparando diferentes
    planes de beneficios de salud.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

2.  Seleccione la opción **Select kernel** disponible en la esquina
    superior derecha. Seleccione **venv (Python 3.x.x)** de la lista.

![A blue and red rectangle with white text AI-generated content may be
incorrect.](./media/image2.png)

3.  Ejecute la celda siguiente para importar las librerías necesarias y
    cargar las variables de entorno para trabajar con Azure AI Projects.
    Esta configuración permite la autenticación segura e interacción con
    los servicios de Azure AI.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image3.png)

4.  Ejecute la celda siguiente para conectarse a su proyecto en
    **Microsoft Foundry** y acceder al modelo **gpt-4o** implementado.
    Esto establece una conexión segura utilizando el **project
    connection string** y las credenciales de Azure.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image4.png)

5.  Ejecute esta celda para crear un **Agente de IA simple** que procesa
    datos y genera un gráfico de barras comparando diferentes planes de
    beneficios de salud utilizando **Microsoft Foundry**. Este script
    inicializa el agente, envía un prompt con los datos de los planes de
    salud y solicita un gráfico de barras. El agente procesa la
    solicitud, genera el gráfico, guarda el archivo de imagen y luego
    elimina el agente.

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image5.png)

6.  Finalmente, observe el resultado.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image6.png)

**Resumen**

En este laboratorio, construyó exitosamente un agente analizador de
planes de seguro médico utilizando Microsoft Foundry y Azure AI services
para automatizar el análisis y la comparación de planes de beneficios de
salud. Aprendió a conectarse a su proyecto en Microsoft Foundry, acceder
al modelo GPT-4o implementado y crear un agente inteligente que procesa
datos complejos de planes de seguro.

El agente interpretó los detalles de los planes, analizó las opciones de
cobertura y generó automáticamente gráficos de barras comparativos para
visualizar las diferencias entre los planes de seguro. Esta experiencia
práctica demostró cómo los agentes de IA pueden agilizar el análisis de
datos, apoyar la toma de decisiones y ayudar a los usuarios a comprender
y comparar fácilmente sus opciones de seguro de salud.

Ha completado este laboratorio correctamente. Haga clic en **Next \>\>**
para continuar.
