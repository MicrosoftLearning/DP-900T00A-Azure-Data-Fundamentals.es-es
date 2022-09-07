---
lab:
  title: Exploración de Spark Streaming en Azure Synapse Analytics
  module: Explore fundamentals of real-time analytics
---

# <a name="explore-spark-streaming-in-azure-synapse-analytics"></a>Exploración de Spark Streaming en Azure Synapse Analytics

En este ejercicio, usará *Spark Structured Streaming* y *tablas delta* en Azure Synapse Analytics para procesar datos de flujos.

Este laboratorio se tarda aproximadamente **15** minutos en completarse.

## <a name="before-you-start"></a>Antes de empezar

Necesitará una [suscripción de Azure](https://azure.microsoft.com/free) en la que tenga acceso de nivel administrativo.

## <a name="provision-a-synapse-analytics-workspace"></a>Aprovisionar un área de trabajo de Synapse Analytics

Para usar Synapse Analytics, debe aprovisionar un recurso en el área de trabajo de Synapse Analytics en la suscripción de Azure.

1. Abra Azure Portal en [Azure Portal](https://portal.azure.com?azure-portal=true) e inicie sesión con las credenciales asociadas con su suscripción de Azure.

    > <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Ensure you are working in the directory containing your own subscription - indicated at the top right under your user ID. If not, select the user icon and switch directory.

2. En la página **Inicio** de Azure Portal, use el icono **&#65291; Crear un recurso** para crear un recurso.
3. Busque *Azure Synapse Analytics*, y cree un recurso de **Azure Synapse Analytics** con la siguiente configuración:
    - **Suscripción**: *suscripción de Azure*
        - **Grupo de recursos**: *cree un grupo de recursos con un nombre apropiado, como "synapse-rg"*.
        - **Grupo de recursos administrado**: *escriba un nombre adecuado, por ejemplo, "synapse-managed-rg"*.
    - **Nombre del área de trabajo**: *escriba un nombre único para el área de trabajo, por ejemplo, "synapse-ws-<your_name>*.
    - **Región**: *seleccione cualquier región disponible*.
    - **Seleccionar Data Lake Storage Gen 2**: en la suscripción.
        - **Nombre de cuenta**: *cree una cuenta con un nombre único, por ejemplo, "datalake<your_name>"*.
        - **Nombre del sistema de archivos**: *cree un sistema de archivos con un nombre único, por ejemplo, "fs<your_name>"*.

    > <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: A Synapse Analytics workspace requires two resource groups in your Azure subscription; one for resources you explicitly create, and another for managed resources used by the service. It also requires a Data Lake storage account in which to store data, scripts, and other artifacts.

4. Cuando haya especificado estos detalles, seleccione **Revisar y crear** y, a continuación, seleccione **Crear** para crear el área de trabajo.
5. Espere a que se cree el área de trabajo; puede tardar unos cinco minutos.
6. Una vez completada la implementación, vaya al grupo de recursos que se creó y observe que contiene el área de trabajo de Synapse Analytics y una cuenta de almacenamiento de Data Lake.
7. Seleccione el área de trabajo de Synapse y, en su página **Información general**, en la tarjeta **Abrir Synapse Studio**, seleccione **Abrir** para abrir Synapse Studio en una nueva pestaña del explorador. Synapse Studio es una interfaz basada en web que puede usar para trabajar con el área de trabajo de Synapse Analytics.
8. En el lado izquierdo de Synapse Studio, use el icono **&rsaquo;&rsaquo;** para expandir el menú; esto muestra las distintas páginas de Synapse Studio que usará para administrar recursos y llevar a cabo tareas de análisis de datos, como se muestra aquí:

    ![Synapse Studio](images/synapse-studio.png)

## <a name="create-a-spark-pool"></a>Crear un grupo de Spark

Para usar Spark para procesar datos de flujos, tiene que agregar un grupo de Spark al área de trabajo de Azure Synapse.

1. En Synapse Studio, seleccione la página **Administrar**.
2. Seleccione la pestaña **Grupos de Apache Spark** y, a continuación, use el icono **&#65291; Nuevo** para crear un grupo de Spark con la siguiente configuración:
    - **Nombre del grupo de Apache Spark**: sparkpool
    - **Familia de tamaños de nodo**: optimizada para memoria
    - **Tamaño del nodo**: pequeño (4 núcleos virtuales/32 GB)
    - **Escalabilidad automática**: habilitada
    - **Número de nodos**: 3----3
3. Revise y cree el grupo de Spark y espere a que se implemente (puede tardar unos minutos).

## <a name="explore-stream-processing"></a>Exploración del procesamiento de flujos

Para explorar el procesamiento de flujos con Spark, usará un cuaderno que contiene código y notas de Python para ayudarlo a llevar a cabo algún procesamiento de flujos básico con Spark Structured Streaming y tablas delta.

1. Descargue el cuaderno [Structured Streaming and Delta Tables.ipynb](https://github.com/MicrosoftLearning/DP-900T00A-Azure-Data-Fundamentals/raw/master/streaming/Spark%20Structured%20Streaming%20and%20Delta%20Tables.ipynb) en su equipo local (si el cuaderno se abre como archivo de texto en el explorador, guárdelo en una carpeta local; tenga en cuenta que debe guardarlo como **Structured Streaming and Delta Tables.ipynb**, no como archivo .txt)
2. En Synapse Studio, seleccione la página **Desarrollar**.
3. En el menú **&#65291;**, seleccione **&#8612; Importar** y seleccione el archivo **Structured Streaming and Delta Tables.ipynb** en el equipo local.
4. Siga las instrucciones del cuaderno para adjuntarlo al grupo de Spark y ejecutar las celdas de código que contiene para explorar varias maneras de usar Spark para el procesamiento de flujos.

## <a name="delete-azure-resources"></a>Eliminación de recursos de Azure

> <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: If you intend to complete other exercises that use Azure Synapse Analytics, you can skip this section. Otherwise, follow the steps below to avoid unnecessary Azure costs.

1. Cierre la pestaña del explorador de Synapse Studio, sin guardar los cambios, y vuelva a Azure Portal.
1. En Azure Portal, en la página **Inicio**, seleccione **Grupos de recursos**.
1. Seleccione el grupo de recursos del área de trabajo de Synapse Analytics (no el grupo de recursos administrado) y compruebe que contiene el área de trabajo de Synapse, la cuenta de almacenamiento y el grupo de Data Explorer del área de trabajo (si completó el ejercicio anterior, también incluirá un grupo de Spark).
1. En la parte superior de la página **Información general** del grupo de recursos, seleccione **Eliminar grupo de recursos**.
1. Escriba el nombre del grupo de recursos para confirmar que quiere eliminarlo y seleccione **Eliminar**.

    Después de unos minutos, se eliminarán el área de trabajo de Azure Synapse y el área de trabajo administrada asociada a ella.
