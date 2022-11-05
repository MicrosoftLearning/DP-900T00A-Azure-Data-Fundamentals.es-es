---
lab:
  title: Exploración de Azure Stream Analytics
  module: Explore fundamentals of real-time analytics
---

# <a name="explore-azure-stream-analytics"></a>Exploración de Azure Stream Analytics

En este ejercicio aprovisionará un trabajo de Azure Stream Analytics en su suscripción de Azure y lo usará para procesar un flujo de datos en tiempo real.

Este laboratorio se tarda aproximadamente **15** minutos en completarse.

## <a name="before-you-start"></a>Antes de empezar

Necesitará una [suscripción de Azure](https://azure.microsoft.com/free) en la que tenga acceso de nivel administrativo.

## <a name="create-azure-resources"></a>Creación de recursos de Azure

1. Inicie sesión en la suscripción de Azure en [Azure Portal](https://portal.azure.com) con sus credenciales de suscripción de Azure.

1. Use el botón **[\>_]** situado a la derecha de la barra de búsqueda en la parte superior de la página para crear una nueva instancia de Cloud Shell en Azure Portal, para lo que deberá seleccionar un entorno de ***Bash*** y crear almacenamiento si se solicita. Cloud Shell proporciona una interfaz de línea de comandos en un panel situado en la parte inferior de Azure Portal, como se muestra a continuación:

    ![Azure Portal con un panel de Cloud Shell](./images/cloud-shell.png)

1. En Azure Cloud Shell, escriba el siguiente comando para descargar los archivos que necesitará para este ejercicio.

    ```bash
    git clone https://github.com/MicrosoftLearning/DP-900T00A-Azure-Data-Fundamentals dp-900
    ```

1. Espere a que se complete el comando y escriba el siguiente comando para cambiar el directorio actual a la carpeta que contiene los archivos de este ejercicio.

    ```bash
    cd dp-900/streaming
    ```

1. Escriba el siguiente comando para ejecutar un script que cree los recursos de Azure que necesitará en este ejercicio.

    ```bash
    bash setup.sh
    ```

    Espere mientras se ejecuta el script y este lleva a cabo las siguientes acciones:

    1. Instalar las extensiones de la CLI de Azure necesarias para crear recursos (*puede omitir las advertencias sobre las extensiones experimentales*)
    1. Identifica el grupo de recursos de Azure proporcionado para este ejercicio.
    1. Crear un recurso de *Azure IoT Hub*, que se usará para recibir un flujo de datos de un dispositivo simulado.
    1. Crear una *cuenta de Azure Storage*, que se usará para almacenar datos procesados.
    1. Crear un trabajo de *Azure Stream Analytics*, que procesará los datos del dispositivo entrantes en tiempo real y escribir los resultados en la cuenta de almacenamiento.

## <a name="explore-the-azure-resources"></a>Exploración de los recursos de Azure

1. En la página principal de [Azure Portal](https://portal.azure.com?azure-portal=true), seleccione **Grupos de recursos** para ver los grupos de recursos de la suscripción. Debería incluir el grupo de recursos **learn*xxxxxxxxxxxxxxxxx...** * que identifica el script de configuración.
2. Seleccione el grupo de recursos **learn*xxxxxxxxxxxxxxxxx...** * y revise los recursos que contiene, que deberían incluir los siguientes:
    - Una instancia de *IoT Hub* llamada **iothub*xxxxxxxxxxxxx***, que se usa para recibir datos del dispositivo entrantes.
    - Una *cuenta de almacenamiento* llamada **store*xxxxxxxxxxxx***, en la que se escribirán los resultados del procesamiento de datos.
    - Un *trabajo de Stream Analytics* llamado **stream*xxxxxxxxxxxxx***, que se usará para procesar datos de flujos.

    Si no se muestran los tres recursos, haga clic en el botón **&#8635; Actualizar** hasta que aparezcan.

    > **Nota**: Si usa el espacio aislado de Learn, el grupo de recursos también puede contener una segunda *cuenta de almacenamiento* llamada **cloudshell*xxxxxxxx***, que se usará a fin de almacenar datos para la instancia de Azure Cloud Shell que ha usado para ejecutar el script de configuración.

3. Seleccione el trabajo de Stream Analytics **stream*xxxxxxxxxxxxx*** y consulte la información en la página **Información general** y tenga en cuenta los siguientes detalles:
    - El trabajo tiene una *entrada* llamada **iotinput** y una *salida* llamada **bloboutput**. Hacen referencia a la instancia de IoT Hub y a la cuenta de almacenamiento creada por el script de configuración.
    - El trabajo tiene una *consulta*, que lee datos de la entrada **iotinput**, los suma al contar el número de mensajes procesados cada 10 segundos y escribe los resultados en la salida **bloboutput**.

## <a name="use-the-resources-to-analyze-streaming-data"></a>Uso de los recursos para analizar datos de flujos

1. En la parte superior de la página **Información general** del trabajo de Stream Analytics, seleccione el botón **&#9655; Iniciar** y, a continuación, en el panel **Iniciar trabajo**, seleccione **Iniciar** para iniciar el trabajo.
2. Espere a recibir la notificación de que el trabajo de flujo se inició correctamente.
3. Vuelva a Azure Cloud Shell y escriba el comando siguiente para simular un dispositivo que envía datos a IoT Hub.

    ```
    bash iotdevice.sh
    ```

4. Espere a que se inicie la simulación, que se indicará mediante una salida como la siguiente:

    ```
    Device simulation in progress: 6%|#    | 7/120 [00:08<02:21, 1.26s/it]
    ```

5. Mientras se ejecuta la simulación, en Azure Portal, vuelva a la página del grupo de recursos **learn*xxxxxxxxxxxxxxxxx...** * y seleccione la cuenta de almacenamiento **store*xxxxxxxxxxxx***.
6. En el panel de la izquierda de la hoja de la cuenta de almacenamiento, seleccione la pestaña **Contenedores**.
7. Abra el contenedor de **datos**.
8. En el contenedor de **datos**, navegue por la jerarquía de carpetas, que incluye una carpeta para el año actual, con subcarpetas para el mes, el día y la hora.
9. En la carpeta de la hora, fíjese en el archivo creado, que debe tener un nombre parecido a **0_xxxxxxxxxxxxxxxx.json**.
10. En el menú **…** del archivo (a la derecha de los detalles del archivo), seleccione **Ver/Editar** y revise el contenido del archivo, que debe consistir en un registro JSON para cada período de 10 segundos que muestre la cantidad de mensajes recibidos de los dispositivos IoT, como este:

    ```
    {"starttime":"2021-10-23T01:02:13.2221657Z","endtime":"2021-10-23T01:02:23.2221657Z","device":"iotdevice","messages":2}
    {"starttime":"2021-10-23T01:02:14.5366678Z","endtime":"2021-10-23T01:02:24.5366678Z","device":"iotdevice","messages":3}
    {"starttime":"2021-10-23T01:02:15.7413754Z","endtime":"2021-10-23T01:02:25.7413754Z","device":"iotdevice","messages":4}
    ...
    ```

11. Use el botón **&#8635; Actualizar** para actualizar el archivo. Tenga en cuenta que los resultados adicionales se escriben en el archivo cuando un trabajo de Stream Analytics procesa los datos del dispositivo en tiempo real a medida que se transmiten desde el dispositivo a IoT Hub.
12. Vuelva a Azure Cloud Shell y espere a que finalice la simulación del dispositivo (debería ejecutarse durante unos 3 minutos).
13. De nuevo en Azure Portal, actualice el archivo una vez más para ver el conjunto completo de resultados que se produjeron durante la simulación.
14. Vuelva al grupo de recursos **learn*xxxxxxxxxxxxxxxxx...** * y vuelva a abrir el trabajo de Stream Analytics **stream*xxxxxxxxxxxxx***.
15. En la parte superior de la página del trabajo de Stream Analytics, use el botón **&#11036; Detener** para detener el trabajo y confírmelo cuando se lo solicite.

> **Nota**: Si ha terminado de explorar la solución de streaming, elimine el grupo de recursos que creó en este ejercicio.
