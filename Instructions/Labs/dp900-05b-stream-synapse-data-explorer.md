---
lab:
  title: Exploración de Data Explorer de Azure Synapse
  module: Explore fundamentals of real-time analytics
---

# Exploración de Data Explorer de Azure Synapse

> **Nota**: Debido a los cambios en el producto, hay algunos problemas conocidos con la sección **Creación de una base de datos e ingesta de datos** de este laboratorio. Estamos trabajando para solucionar estos problemas.

En este ejercicio, usará Azure Synapse Data Explorer para analizar datos de serie temporal.

Este laboratorio se tarda aproximadamente **25** minutos en completarse.

## Antes de empezar

Necesitará una [suscripción de Azure](https://azure.microsoft.com/free) en la que tenga acceso de nivel administrativo.

## Aprovisionar un área de trabajo de Synapse Analytics

> **Sugerencia**: Si todavía tiene un área de trabajo de Azure Synapse de un ejercicio anterior, omita esta sección y vaya directamente a **[Creación de un grupo de Data Explorer](#create-a-data-explorer-pool)** .

1. Abra Azure Portal en [https://portal.azure/com](https://portal.azure.com?azure-portal=true) e inicie sesión con las credenciales asociadas con su suscripción de Azure.

    >                 **Nota**: Asegúrese de que está trabajando en el directorio que contiene la suscripción, lo que se indica en la parte superior derecha, debajo del identificador de usuario. Si no es así, seleccione el icono de usuario y cambie el directorio.

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

    >                 **Nota**: Un área de trabajo de Synapse Analytics requiere dos grupos de recursos en la suscripción de Azure; uno para los recursos creados explícitamente y otro para los recursos administrados que utiliza el servicio. También requiere una cuenta de almacenamiento de Data Lake en la que almacenar datos, scripts y otros artefactos.

1. Cuando haya especificado estos detalles, seleccione **Revisar y crear** y, a continuación, seleccione **Crear** para crear el área de trabajo.
1. Espere a que se cree el área de trabajo; puede tardar unos cinco minutos.
1. Una vez completada la implementación, vaya al grupo de recursos que se creó y observe que contiene el área de trabajo de Synapse Analytics y una cuenta de almacenamiento de Data Lake.
1. Seleccione el área de trabajo de Synapse y, en su página **Información general**, en la tarjeta **Abrir Synapse Studio**, seleccione **Abrir** para abrir Synapse Studio en una nueva pestaña del explorador. Synapse Studio es una interfaz basada en web que puede usar para trabajar con el área de trabajo de Synapse Analytics.
1. En el lado izquierdo de Synapse Studio, use el icono **&rsaquo;&rsaquo;** para expandir el menú; se muestran las distintas páginas de Synapse Studio que usará para administrar recursos y realizar tareas de análisis de datos.

## Crear un grupo de Data Explorer

1. En Synapse Studio, seleccione la página **Administrar**.
1. Seleccione la pestaña **Grupos de Data Explorer** y, luego, utilice el icono **&#65291; Nuevo** para crear un grupo con esta configuración:
    - **Nombre del grupo de Data Explorer**: dxpool
    - **Carga de trabajo**: optimizado para proceso
    - **Tamaño**: extra pequeño (2 núcleos)
1. Seleccione **Siguiente: Configuración adicional >** y habilite la opción **Ingesta de streaming**. Esto permite habilitar Data Explorar para ingerir datos nuevos desde un origen de streaming como Azure Event Hubs.
1. Seleccione **Revisar y crear** para crear el grupo de Data Explorer y, luego, espere a que se implemente (lo que puede tardar 15 minutos o más; el estado cambiará de *Creando* a *En línea*).

## Creación de una base de datos e ingesta de datos

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
1. Asegúrese de que los tipos de datos de columna se identificaron correctamente como *Hora (datetime)*, *Dispositivo (string)* y *Valor (long)*). Luego, seleccione **Siguiente: Iniciar ingesta**.
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

    >                 **Sugerencia**: En este ejemplo, ha importado una cantidad muy pequeña de datos por lotes desde un archivo, lo que sirve para los fines de este ejercicio. En realidad, puede usar Data Explorer para analizar volúmenes de datos mucho más grandes y, como habilitó la ingesta de flujos, también podría haber configurado Data Explorer para ingerir datos en la tabla desde un origen de streaming como Azure Event Hubs.

## Uso del lenguaje de consulta Kusto para consultar la tabla en Synapse Studio

1. Cierre la pestaña del explorador de Azure Data Explorer y vuelva a la pestaña que contiene Synapse Studio.
1. En la página **Datos**, expanda la base de datos **iot-data** y su carpeta **Tablas**. Luego, en el menú **…** de la tabla **devices**, seleccione **Nuevo script de KQL** > **Tomar 1000 filas**.
1. Revise la consulta generada y sus resultados. La consulta debe contener el código siguiente:

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

1. Seleccione **&#9655; Ejecutar** para ejecutar la consulta. Luego, revise los resultados, que solo deben contener las filas correspondientes al dispositivo *Dev1*.

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

## Eliminación de recursos de Azure

Ahora que ha terminado de explorar Azure Synapse Analytics, debe eliminar los recursos que ha creado para evitar costos innecesarios de Azure.

1. Cierre la pestaña del explorador de Synapse Studio, sin guardar los cambios, y vuelva a Azure Portal.
1. En Azure Portal, en la página **Inicio**, seleccione **Grupos de recursos**.
1. Seleccione el grupo de recursos del área de trabajo de Synapse Analytics (no el grupo de recursos administrado) y compruebe que contiene el área de trabajo de Synapse, la cuenta de almacenamiento y el grupo de Data Explorer del área de trabajo (si completó el ejercicio anterior, también incluirá un grupo de Spark).
1. En la parte superior de la página **Información general** del grupo de recursos, seleccione **Eliminar grupo de recursos**.
1. Escriba el nombre del grupo de recursos para confirmar que quiere eliminarlo y seleccione **Eliminar**.

    Después de unos minutos, se eliminarán el área de trabajo de Azure Synapse y el área de trabajo administrada asociada a ella.
