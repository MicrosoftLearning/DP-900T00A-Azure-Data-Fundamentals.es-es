---
lab:
  title: "Exploración de análisis de datos en Microsoft\_Fabric"
  module: Explore fundamentals of large-scale data analytics
---

# Exploración de análisis de datos en Microsoft Fabric

En este ejercicio, explorarás la ingesta y el análisis de datos en un almacén de lago de Microsoft Fabric.

Este laboratorio se tarda aproximadamente **25** minutos en completarse.

> **Nota**: Necesitarás una licencia de Microsoft Fabric para realizar este ejercicio. Consulta [Introducción a Fabric](https://learn.microsoft.com/fabric/get-started/fabric-trial) para más información sobre cómo habilitar una licencia de prueba de Fabric gratuita. Para ello, necesitarás una cuenta *profesional* o *educativa* de Microsoft. Si no tienes una, puedes [registrarte para una evaluación gratuita de Microsoft Office 365 E3 o superior](https://www.microsoft.com/microsoft-365/business/compare-more-office-365-for-business-plans).

*La primera vez que uses las características de Microsoft Fabric, pueden aparecer avisos con sugerencias. Descártalos.*

## Creación de un área de trabajo

Antes de trabajar con datos de Fabric, crea un área de trabajo con la evaluación gratuita de Fabric habilitada.

1. Inicia sesión en [Microsoft Fabric](https://app.fabric.microsoft.com) en `https://app.fabric.microsoft.com`.
1. En la parte inferior izquierda del portal, cambia a la experiencia **Ingeniería de datos**.

    ![Captura de pantalla del menú del conmutador de experiencias.](./images/fabric-switcher.png)

1. En la barra de menús de la izquierda, seleccione **Áreas de trabajo** (el icono tiene un aspecto similar a &#128455;).
1. Cree una nueva área de trabajo con el nombre que prefiera y seleccione un modo de licencia en la sección **Avanzado** que incluya la capacidad de Fabric (*Prueba*, *Premium* o *Fabric*).
1. Cuando se abra la nueva área de trabajo, debe estar vacía.

    ![Captura de pantalla de un área de trabajo vacía en Power BI.](./images/new-workspace.png)

## Crear un almacén de lago

Ahora que tienes un área de trabajo, es el momento de crear un almacén de lago de datos para los archivos de datos.

1. En la página principal Ingeniería de datos, crea un nuevo **almacén de lago** con el nombre que prefieras.

    Después de un minuto o así, se habrá creado un nuevo almacén de lago:

    ![Captura de pantalla de un nuevo almacén de lago.](./images/new-lakehouse.png)

1. Mira el nuevo almacén de lago y ten en cuenta que el panel **Explorador del almacén de lago** de la izquierda te permite examinar las tablas y los archivos del almacén de lago:
    - La carpeta **Tablas** contiene tablas que puedes consultar usando SQL. Las tablas de un almacén de lago de Microsoft Fabric se basan en el formato de archivo de *Delta Lake* de código abierto, que se usa habitualmente en Apache Spark.
    - La carpeta **Archivos** contiene archivos de datos del almacenamiento OneLake para el almacén de lago que no están asociados a tablas Delta administradas. También puedes crear *accesos directos* en esta carpeta para hacer referencia a datos almacenados externamente.

    Actualmente, no hay tablas ni archivos en el almacén de lago.

## Ingerir datos

Una manera sencilla de ingerir datos consiste en usar una actividad **Copiar datos** en una canalización para extraer los datos de un origen y copiarlos en un archivo del almacén de lago.

1. En la página **Inicio** del almacén de lago, en el menú **Obtener datos** selecciona **Nueva canalización de datos** y crea una canalización de datos denominada **Ingerir datos**.
1. En el Asistente para **copiar datos**, en la página **Elegir un origen de datos**, selecciona **Datos de ejemplo** y después selecciona el conjunto de datos de ejemplo **NYC Taxi - Green**.

    ![Captura de pantalla de la página "Elegir origen de datos".](./images/choose-data-source.png)

1. Mira las tablas del origen de datos en la página **Conectarse al origen de datos**. Debe haber una tabla que contenga los detalles de los viajes de taxi en la ciudad de Nueva York. A continuación, selecciona **Siguiente** para avanzar a la página **Elegir destino de datos**.
1. En la página **Elegir destino de datos**, selecciona el almacén de lago existente. Luego, selecciona **Siguiente**.
1. Establece las siguientes opciones de destino de datos y, luego, selecciona **Siguiente**:
    - **Carpeta raíz**: Tablas
    - **Configuración de carga**: cargar en una nueva tabla
    - **Nombre de la tabla de destino**: taxi_rides *(es posible que tengas que esperar a que se muestre la vista previa de las asignaciones de columnas antes de poder cambiarlo)*
    - **Asignaciones de columnas**: *deja las asignaciones predeterminadas tal cual*
    - **Habilitar partición**: *no seleccionada*
1. En la página **Revisar y guardar**, asegúrate de que la opción **Iniciar transferencia de datos inmediatamente** esté activa y después selecciona **Guardar y ejecutar**.

    Se crea una nueva canalización que contiene una actividad **Copiar datos**, como se muestra aquí:

    ![Captura de pantalla de una canalización con una actividad Copiar datos.](./images/copy-data-pipeline.png)

    Cuando la canalización comienza a ejecutarse, puedes supervisar su estado en el panel **Salida** en el diseñador de canalizaciones. Usa el icono **&#8635;** (*Actualizar*) para actualizar el estado y espera hasta que la operación se haya realizado correctamente (puede tardar 10 minutos o más).

1. En la barra de menús central, a la izquierda, selecciona el almacén de lago.
1. En la página **Inicio**, en el panel **Explorador de almacén de lago**, en el menú **...** del nodo **Tablas**, selecciona **Actualizar** y expande **Tablas** para comprobar que se ha creado la tabla **taxi_rides**.

    > **Nota**: Si la nueva tabla aparece como *no identificada*, usa la opción de menú **Actualizar** para actualizar la vista.

1. Selecciona la tabla **taxi_rides** para ver su contenido.

    ![Captura de pantalla de la tabla taxi_rides.](./images/dimProduct.png)

## Consulta de datos en un almacén de lago

Ahora que has ingerido datos en una tabla de almacén de lago, puedes usar SQL para consultarlos.

1. En la parte superior derecha de la página del **almacén de lago**, cambia al **punto de conexión de análisis SQL** del almacén de lago.

1. En la barra de herramientas, selecciona **Nueva consulta SQL**. A continuación, escribe el código SQL siguiente en el editor de consultas:

    ```sql
    SELECT  DATENAME(dw,lpepPickupDatetime) AS Day,
            AVG(tripDistance) As AvgDistance
    FROM taxi_rides
    GROUP BY DATENAME(dw,lpepPickupDatetime)
    ```

1. Selecciona el botón **&#9655; Ejecutar** para ejecutar la consulta y revisar los resultados, que deben incluir la distancia media de viaje para cada día de la semana.

    ![Captura de pantalla de una consulta SQL.](./images/sql-query.png)

## Visualización de datos en un almacén de lago

Los almacenes de lago de Microsoft Fabric organizan todas las tablas en un modelo de datos semántico, que puedes usar para crear visualizaciones e informes.

1. En la parte inferior izquierda de la página, en el panel **Explorar**, selecciona la pestaña **Modelo** para ver el modelo de datos de las tablas de un almacén de lago (esto incluye las tablas del sistema así como la tabla **taxi_rides**).
1. En la barra de herramientas, selecciona **Nuevo informe** para crear un nuevo informe basado en **taxi_rides**.
1. En el diseñador de informes:
    1. En el panel **Datos**, expande la tabla **taxi_rides** y selecciona los campos **lpepPickupDatetime** y **passengerCount**.
    1. En el panel **Visualizaciones**, selecciona la visualización **Gráfico de líneas**. A continuación, asegúrate de que el **eje X** contiene el campo **lpepPickupDatetime** y que el **eje Y** contiene **Sum of passengerCount**.

        ![Captura de pantalla de un informe de Power BI](./images/fabric-report.png)

    > **Sugerencia**: Puedes usar los iconos **>>** para ocultar los paneles del diseñador de informes con el fin de ver el informe con más claridad.

1. En el menú **Archivo**, selecciona **Guardar** para guardar el informe como **Taxi Rides Report** en el área de trabajo de Fabric.

    Ahora puedes cerrar la pestaña del explorador que contiene el informe para volver al almacén de lago. Puedes encontrar el informe en la página del área de trabajo en el portal de Microsoft Fabric.

## Limpieza de recursos

Si has terminado de explorar Microsoft Fabric, puedes eliminar el área de trabajo que creaste para este ejercicio.

1. En la barra de la izquierda, selecciona el icono del área de trabajo para ver todos los elementos que contiene.
2. En el menú **...** de la barra de herramientas, selecciona **Configuración del área de trabajo**.
3. En la sección **Otros**, selecciona **Quitar esta área de trabajo**.
