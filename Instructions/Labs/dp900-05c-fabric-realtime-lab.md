---
lab:
  title: "Exploración del análisis en tiempo real en Microsoft\_Fabric"
  module: Explore fundamentals of large-scale data analytics
---

# Exploración del análisis en tiempo real en Microsoft Fabric

En este ejercicio, explorará el análisis en tiempo real en Microsoft Fabric.

Este laboratorio se tarda aproximadamente **25** minutos en completarse.

> **Nota:** Necesitará una licencia de Microsoft Fabric para realizar este ejercicio. Consulte [Introducción a Fabric](https://learn.microsoft.com/fabric/get-started/fabric-trial) para más información sobre cómo habilitar una licencia de prueba de Fabric gratuita. Para ello, necesitará una cuenta *profesional* o *educativa* de Microsoft. Si no tiene una, puede [registrarse para una evaluación gratuita de Microsoft Office 365 E3 o superior](https://www.microsoft.com/microsoft-365/business/compare-more-office-365-for-business-plans).

## Crear un área de trabajo

Antes de trabajar con datos de Fabric, cree un área de trabajo con la evaluación gratuita de Fabric habilitada.

1. Inicie sesión en [Microsoft Fabric](https://app.fabric.microsoft.com) en `https://app.fabric.microsoft.com`.
2. En la barra de menús de la izquierda, seleccione **Áreas de trabajo** (el icono tiene un aspecto similar a &#128455;).
3. Cree una nueva área de trabajo con el nombre que prefiera y seleccione un modo de licencia en la sección **Avanzado** que incluya la capacidad de Fabric (*Prueba*, *Premium* o *Fabric*).
4. Cuando se abra la nueva área de trabajo, debe estar vacía.

    ![Captura de pantalla de un área de trabajo vacía en Power BI.](./images/new-workspace.png)

## Creación de una base de datos KQL

Ahora que tiene un área de trabajo, puede crear una base de datos de KQL para almacenar datos en tiempo real.

1. En la parte inferior izquierda del portal, cambie a la experiencia **Inteligencia en tiempo real**.

    ![Captura de pantalla del menú del conmutador de experiencias.](./images/fabric-real-time.png)

    La página principal de Inteligencia en tiempo real incluye iconos para crear activos usados habitualmente para los datos en tiempo real.

2. En la página principal de Inteligencia en tiempo real, cree un nuevo **Centro de eventos** con el nombre que prefiera.

    ![Captura de pantalla del editor de RTA con la opción Crear base de datos KQL resaltada.](./images/create-kql-db.png)

    El Centro de eventos se usa para agrupar y administrar las bases de datos entre proyectos. Una base de datos KQL vacía se crea automáticamente con el nombre del centro de eventos y agregaremos datos a ella más adelante en este ejercicio.

## Crear un Eventstream

Los objetos Eventstream proporcionan una manera escalable y flexible de ingerir datos en tiempo real desde un origen de streaming.

1. En la barra de menús de la izquierda, seleccione la página **Inicio** de la experiencia de Inteligencia en tiempo real.
1. En la página principal, seleccione el icono para crear un nuevo objeto **Eventstream** con un nombre de su elección.

    Después de un breve tiempo, se muestra el diseñador visual del objeto Eventstream.

    ![Captura de pantalla del diseñador de Eventstream.](./images/eventstream-designer.png)

    El lienzo del diseñador visual muestra un origen que se conecta al objeto Eventstream, que a su vez se conecta a un destino.

1. En el lienzo del diseñador, en la lista **Nuevo origen** del origen, seleccione **Datos de ejemplo**. A continuación, en el panel **Datos de ejemplo**, especifique el nombre **taxis** y seleccione los datos de ejemplo de **Yellow Taxi** (que representan los datos recopilados de los recorridos de taxi). A continuación, seleccione **Agregar**.
1. Debajo del lienzo del diseñador, seleccione la pestaña **Vista previa de datos** para obtener una vista previa de los datos que se transmiten desde el origen:

    ![Captura de pantalla de la vista previa de datos de Eventstream.](./images/eventstream-preview.png)

1. En el lienzo del diseñador, en la lista **Nuevo destino** del destino, seleccione **Base de datos KQL**. A continuación, en el panel **Base de datos KQL**, especifique el nombre de destino **taxi-data** y seleccione el área de trabajo y la base de datos KQL. Seleccione **Crear nuevo** en Tabla de destino y escriba el nombre de tabla **taxi-data**. Luego, seleccione **Agregar**.
1. Compruebe que el objeto Eventstream completado tiene este aspecto:

    ![Captura de pantalla de un objeto Eventstream completado.](./images/complete-eventstream.png)

## Consulta de datos en tiempo real en una base de datos KQL

El flujo de eventos rellena continuamente una tabla en la base de datos KQL, lo que le permite consultar los datos en tiempo real.

1. En el centro de menús de la izquierda, seleccione la base de datos KQL (o seleccione el área de trabajo y busque allí la base de datos KQL).
1. En el menú **...** de la tabla **taxi-data** (que ha creado el flujo de eventos), seleccione **Tabla de consulta > Registros ingeridos en las últimas 24 horas**.

    ![Captura de pantalla del menú Tabla de consulta en una base de datos KQL.](./images/kql-query.png)

1. Vea los resultados de la consulta, que debe ser una consulta KQL como esta:

    ```kql
    ['taxi-data']
    | where ingestion_time() between (now(-1d) .. now())
    ```

    Los resultados muestran todos los registros de taxi ingeridos desde el origen de streaming en las últimas 24 horas.

1. Reemplace todo el código de consulta de KQL en la mitad superior del editor de consultas por el código siguiente:

    ```kql
    // This query returns the number of taxi pickups per hour
    ['taxi-data']
    | summarize PickupCount = count() by bin(todatetime(tpep_pickup_datetime), 1h)
    ```

1. Use el botón **&#9655; Ejecutar** para ejecutar la consulta y revisar los resultados, que muestran el número de recogidas de taxi para cada hora.

## Limpieza de recursos

Si ha terminado de explorar el análisis en tiempo real en Microsoft Fabric, puede eliminar el área de trabajo que creó para este ejercicio.

1. En la barra de la izquierda, seleccione el icono del área de trabajo para ver todos los elementos que contiene.
2. En el menú **...** de la barra de herramientas, seleccione **Configuración del área de trabajo**.
3. En la sección **Otros**, seleccione **Quitar esta área de trabajo**.
