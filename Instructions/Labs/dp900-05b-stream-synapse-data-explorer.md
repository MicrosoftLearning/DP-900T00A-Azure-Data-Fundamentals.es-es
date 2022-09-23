---
lab:
  title: Exploración de Data Explorer de Azure Synapse
  module: Explore fundamentals of real-time analytics
---

# <a name="explore-azure-synapse-data-explorer"></a>Exploración de Data Explorer de Azure Synapse

En este ejercicio, usará Azure Synapse Data Explorer para analizar datos de serie temporal.

Este laboratorio se tarda aproximadamente **25** minutos en completarse.

## <a name="before-you-start"></a>Antes de empezar

Necesitará una [suscripción de Azure](https://azure.microsoft.com/free) en la que tenga acceso de nivel administrativo.

## <a name="provision-a-synapse-analytics-workspace"></a>Aprovisionar un área de trabajo de Synapse Analytics

> **Sugerencia**: Si todavía tiene un área de trabajo de Azure Synapse de un ejercicio anterior, omita esta sección y vaya directamente a **[Creación de un grupo de Data Explorer](#create-a-data-explorer-pool)** .

1. Abra Azure Portal en [https://portal.azure/com](https://portal.azure.com?azure-portal=true) e inicie sesión con las credenciales asociadas con su suscripción de Azure.

    > <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Ensure you are working in the directory containing your subscription - indicated at the top right under your user ID. If not, select the user icon and switch directory.

1. En la página **Inicio** de Azure Portal, use el icono **&#65291; Crear un recurso** para crear un recurso.
1. Busque *Azure Synapse Analytics*, y cree un recurso de **Azure Synapse Analytics** con la siguiente configuración:
    - **Suscripción**: *suscripción de Azure*
        - **Grupo de recursos**: *cree un grupo de recursos con un nombre apropiado, como "synapse-rg"*.
        - **Grupo de recursos administrado**: *escriba un nombre adecuado, por ejemplo, "synapse-managed-rg"*.
    - **Nombre del área de trabajo**: *escriba un nombre único para el área de trabajo, por ejemplo, "synapse-ws-<your_name>*.
    - **Región**: *seleccione cualquier región disponible*.
    - **Seleccionar Data Lake Storage Gen 2**: en la suscripción.
        - **Nombre de cuenta**: *cree una cuenta con un nombre único, por ejemplo, "datalake<your_name>"*.
        - **Nombre del sistema de archivos**: *cree un sistema de archivos con un nombre único, por ejemplo, "fs<your_name>"*.

    > <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: A Synapse Analytics workspace requires two resource groups in your Azure subscription; one for resources you explicitly create, and another for managed resources used by the service. It also requires a Data Lake storage account in which to store data, scripts, and other artifacts.

1. Cuando haya especificado estos detalles, seleccione **Revisar y crear** y, a continuación, seleccione **Crear** para crear el área de trabajo.
1. Espere a que se cree el área de trabajo; puede tardar unos cinco minutos.
1. Una vez completada la implementación, vaya al grupo de recursos que se creó y observe que contiene el área de trabajo de Synapse Analytics y una cuenta de almacenamiento de Data Lake.
1. Seleccione el área de trabajo de Synapse y, en su página **Información general**, en la tarjeta **Abrir Synapse Studio**, seleccione **Abrir** para abrir Synapse Studio en una nueva pestaña del explorador. Synapse Studio es una interfaz basada en web que puede usar para trabajar con el área de trabajo de Synapse Analytics.
1. En el lado izquierdo de Synapse Studio, use el icono **&rsaquo;&rsaquo;** para expandir el menú; esto muestra las distintas páginas de Synapse Studio que usará a fin de administrar recursos y realizar tareas de análisis de datos.

## <a name="create-a-data-explorer-pool"></a>Crear un grupo de Data Explorer

1. En Synapse Studio, seleccione la página **Administrar**.
1. Seleccione la pestaña **Grupos de Data Explorer** y, luego, utilice el icono **&#65291; Nuevo** para crear un grupo con esta configuración:
    - **Nombre del grupo de Data Explorer**: dxpool
    - **Carga de trabajo**: optimizado para proceso
    - **Tamaño**: extra pequeño (2 núcleos)
1. Seleccione **Siguiente: Configuración adicional >** y habilite la opción **Ingesta de streaming**. Esto permite habilitar Data Explorar para ingerir datos nuevos desde un origen de streaming como Azure Event Hubs.
1. Seleccione **Revisar y crear** para crear el grupo de Data Explorer y, luego, espere a que se implemente (lo que puede tardar 15 minutos o más; el estado cambiará de *Creando* a *En línea*).

## <a name="create-a-database-and-ingest-data"></a>Creación de una base de datos e ingesta de datos

1. En Synapse Studio, seleccione la página **Datos**.
1. Asegúrese de que la pestaña **Área de trabajo** esté seleccionada y, si es necesario, seleccione el icono **&#8635;** que se encuentra en la parte superior izquierda de la página para actualizar la vista de modo que se muestre la opción **Bases de datos de Data Explorer**.
1. Expanda **Base de datos de Data Explorer** y compruebe que aparece **dxpool**.
1. En el panel **Datos**, use el icono **&#65291;** para crear una **base de datos de Data Explorer** en el grupo **dxpool** con el nombre **iot-data**.
1. Mientras espera a que se cree la base de datos, descargue el archivo **devices.csv** desde [https://github.com/MicrosoftLearning/DP-900T00A-Azure-Data-Fundamentals/raw/master/streaming/data/devices.csv](https://github.com/MicrosoftLearning/DP-900T00A-Azure-Data-Fundamentals/raw/master/streaming/data/devices.csv?azure-portal=true) y guárdelo en cualquier carpeta del equipo local.
1. En Synapse Studio, espere a que se cree la base de datos si es necesario y, luego, en el menú **…** de la base de datos **iot-data** nueva, seleccione **Abrir en Azure Data Explorer**.
1. En la pestaña nueva del explorador que contiene Azure Data Explorer, en la pestaña **Datos**, seleccione **Ingerir nuevos datos**.
1. En la página **Destino**, seleccione esta configuración:
    - **Clúster**: *el grupo **dxpool** de Data Explorer en el área de trabajo de Azure Synapse*
    - **Base de datos**: iot-data
    - **Tabla**: cree una tabla denominada **devices**
1. Seleccione **Siguiente: Origen** y, en la página **Origen**, seleccione estas opciones:
    - **Tipo de origen**: archivo
    - **Archivos**: cargue el archivo **devices.csv** desde el equipo local.
1. Seleccione **Siguiente: Esquema** y, en la página **Esquema**, asegúrese de que la configuración siguiente es correcta:
    - **Tipo de compresión**: sin comprimir
    - **Formato de datos**: CSV
    - **Omitir el primer registro**: seleccionado
    - **Asignación**: devices_mapping
1. Ensure the column data types have been correctly identified as <bpt id="p1">*</bpt>Time (datetime)<ept id="p1">*</ept>, <bpt id="p2">*</bpt>Device (string)<ept id="p2">*</ept>, and <bpt id="p3">*</bpt>Value (long)<ept id="p3">*</ept>). Then select <bpt id="p1">**</bpt>Next: Start Ingestion<ept id="p1">**</ept>.
1. Una vez que se complete la ingesta, seleccione **Cerrar**.
1. En la pestaña **Consulta** de Azure Data Explorer, asegúrese de que esté seleccionada la base de datos **iot-data** y, luego, en el panel de consulta, escriba la consulta siguiente.

    ```kusto
    devices
    ```

1. En la barra de herramientas, seleccione **&#9655; Ejecutar** para ejecutar la consulta y revise los resultados, que deben tener un aspecto similar al siguiente:

    | Hora | Dispositivo | Value |
    | --- | --- | --- |
    | 2022-01-01T00:00:00Z | Dev1 | 7 |
    | 2022-01-01T00:00:01Z | Dev2 | 4 |
    | ... | ... | ... |

    Si los resultados coinciden, significa que creó correctamente la tabla **devices** a partir de los datos del archivo.

    > <bpt id="p1">**</bpt>Tip<ept id="p1">**</ept>: In this example, you imported a very small amount of batch data from a file, which is fine for the purposes of this exercise. In reality, you can use Data Explorer to analyze much larger volumes of data; and since you enabled stream ingestion, you could also have configured Data Explorer to ingest data into the table from a streaming source such as Azure Event Hubs.

## <a name="use-kusto-query-language-to-query-the-table-in-synapse-studio"></a>Uso del lenguaje de consulta Kusto para consultar la tabla en Synapse Studio

1. Cierre la pestaña del explorador de Azure Data Explorer y vuelva a la pestaña que contiene Synapse Studio.
1. On the <bpt id="p1">**</bpt>Data<ept id="p1">**</ept> page, expand the <bpt id="p2">**</bpt>iot-data<ept id="p2">**</ept> database and its <bpt id="p3">**</bpt>Tables<ept id="p3">**</ept> folder. Then in the <bpt id="p1">**</bpt>...<ept id="p1">**</ept> menu for the <bpt id="p2">**</bpt>devices<ept id="p2">**</ept> table, select <bpt id="p3">**</bpt>New KQL Script<ept id="p3">**</ept><ph id="ph1"> &gt; </ph><bpt id="p4">**</bpt>Take 1000 rows<ept id="p4">**</ept>.
1. Review the generated query and its results. The query should contain the following code:

    ```kusto
    devices
    | take 1000
    ```

    Los resultados de la consulta contienen las primeras 1000 filas de datos.

1. Modifique la consulta del siguiente modo:

    ```kusto
    devices
    | where Device == 'Dev1'
    ```

1. Select <bpt id="p1">**</bpt>&amp;#9655; Run<ept id="p1">**</ept> to run the query. Then review the results, which should contain only the rows for the <bpt id="p1">*</bpt>Dev1<ept id="p1">*</ept> device.

1. Modifique la consulta del siguiente modo:

    ```kusto
    devices
    | where Device == 'Dev1'
    | where Time > datetime(2022-01-07)
    ```

1. Ejecute la consulta y revise los resultados, que solo deben contener las filas correspondientes al dispositivo *Dev1* posteriores del 7 de enero de 2022.

1. Modifique la consulta del siguiente modo:

    ```kusto
    devices
    | where Time between (datetime(2022-01-01 00:00:00) .. datetime(2022-07-01 23:59:59))
    | summarize AvgVal = avg(Value) by Device
    | sort by Device asc
    ```

1. Ejecute la consulta y revise los resultados, que deben contener el valor promedio del dispositivo registrado entre el 1 de enero y el 7 de enero de 2022 en orden ascendente del nombre del dispositivo.

1. Cierre la pestaña de la consulta de KQL y descarte los cambios.

## <a name="delete-azure-resources"></a>Eliminación de recursos de Azure

Ahora que ha terminado de explorar Azure Synapse Analytics, debe eliminar los recursos que ha creado para evitar costos innecesarios de Azure.

1. Cierre la pestaña del explorador de Synapse Studio, sin guardar los cambios, y vuelva a Azure Portal.
1. En Azure Portal, en la página **Inicio**, seleccione **Grupos de recursos**.
1. Seleccione el grupo de recursos del área de trabajo de Synapse Analytics (no el grupo de recursos administrado) y compruebe que contiene el área de trabajo de Synapse, la cuenta de almacenamiento y el grupo de Data Explorer del área de trabajo (si completó el ejercicio anterior, también incluirá un grupo de Spark).
1. En la parte superior de la página **Información general** del grupo de recursos, seleccione **Eliminar grupo de recursos**.
1. Escriba el nombre del grupo de recursos para confirmar que quiere eliminarlo y seleccione **Eliminar**.

    Después de unos minutos, se eliminarán el área de trabajo de Azure Synapse y el área de trabajo administrada asociada a ella.
