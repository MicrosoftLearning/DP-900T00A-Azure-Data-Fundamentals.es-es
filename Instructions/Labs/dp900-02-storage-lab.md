---
lab:
  title: "Exploración de Azure\_Storage"
  module: Explore Azure Storage for non-relational data
---

# <a name="explore-azure-storage"></a>Exploración de Azure Storage

En este ejercicio aprovisionará una cuenta de Azure Storage en su suscripción de Azure y explorará las distintas formas en que puede usarla para almacenar datos.

Este laboratorio se tarda aproximadamente **15** minutos en completarse.

## <a name="before-you-start"></a>Antes de empezar

Necesitará una [suscripción de Azure](https://azure.microsoft.com/free) en la que tenga acceso de nivel administrativo.

## <a name="provision-an-azure-storage-account"></a>Aprovisionamiento de una cuenta de Azure Storage

El primer paso para usar Azure Storage es aprovisionar una cuenta de Azure Storage en su suscripción de Azure.

1. Si todavía no lo ha hecho, inicie sesión en [Azure Portal](https://portal.azure.com?azure-portal=true).
1. On the Azure portal home page, select <bpt id="p1">**</bpt>&amp;#65291; Create a resource<ept id="p1">**</ept> from the upper left-hand corner and search for <bpt id="p2">*</bpt>Storage account<ept id="p2">*</ept>. Then in the resulting <bpt id="p1">**</bpt>Storage account<ept id="p1">**</ept> page, select <bpt id="p2">**</bpt>Create<ept id="p2">**</ept>.
1. Escriba los valores siguientes en la página **Crear una cuenta de almacenamiento**:
    - **Suscripción**: Seleccione su suscripción a Azure.
    - **Grupo de recursos**: cree un grupo de recursos con el nombre que prefiera.
    - **Nombre de la cuenta de almacenamiento**: escriba un nombre único para la cuenta de almacenamiento con números y letras minúsculas.
    - **Región**: seleccione cualquier ubicación disponible.
    - **Rendimiento**: *Estándar*
    - **Redundancia**: *almacenamiento con redundancia local (LRS)*

1. Select <bpt id="p1">**</bpt>Next: Advanced &gt;<ept id="p1">**</ept> and view the advanced configuration options. In particular, note that this is where you can enable hierarchical namespace to support Azure Data Lake Storage Gen2. Leave this option <bpt id="p1">**</bpt><bpt id="p2">&lt;u&gt;</bpt>unselected<ept id="p2">&lt;/u&gt;</ept><ept id="p1">**</ept> (you'll enable it later), and then select <bpt id="p3">**</bpt>Next: Networking &gt;<ept id="p3">**</ept> to view the networking options for your storage account.
1. Select <bpt id="p1">**</bpt>Next: Data protection &gt;<ept id="p1">**</ept> and then in the <bpt id="p2">**</bpt>Recovery<ept id="p2">**</ept> section, <bpt id="p3">&lt;u&gt;</bpt>de<ept id="p3">&lt;/u&gt;</ept>select all of the <bpt id="p4">**</bpt>Enable soft delete...<ept id="p4">**</ept> options. These options retain deleted files for subsequent recovery, but can cause issues later when you enable hierarchical namespace.
1. Continúe por el resto de las páginas **Siguiente >** sin cambiar la configuración predeterminada y, luego, en la página **Revisar y crear**, espere la validación de sus selecciones y seleccione **Crear** para crear una cuenta de Azure Storage.
1. Wait for deployment to complete. Then go to the resource that was deployed.

## <a name="explore-blob-storage"></a>Exploración de almacenamiento de blobs

Ahora que tiene una cuenta de Azure Storage, puede crear un contenedor para los datos de blobs.

1. Descargue el archivo JSON [product1.json](https://aka.ms/product1.json?azure-portal=true) desde `https://aka.ms/product1.json` y guárdelo en el equipo (puede guardarlo en cualquier carpeta, porque lo cargará en el almacenamiento de blobs más adelante).

    *Si el archivo JSON aparece en el explorador, guarde la página como **product1.json**.*

1. En la página del contenedor de almacenamiento en Azure Portal, en la sección **Almacenamiento de datos** que aparece al lado izquierdo, seleccione **Contenedores**.
1. En la página **Contenedores**, seleccione **&#65291; Contenedor** y agregue un contenedor nuevo denominado **data** con un nivel de acceso público de **Privado (sin acceso anónimo)** .
1. Una vez creado el contenedor **data**, compruebe que aparece en la página **Contenedores**.
1. In the pane on the left side, in the top section, select **Storage browser **. This page provides a browser-based interface that you can use to work with the data in your storage account.
1. En la página del explorador de almacenamiento, seleccione **Contenedores de blobs** y compruebe que aparece el contenedor **data**.
1. Seleccione el contenedor **data** y observe que está vacío.
1. Seleccione **&#65291; Agregar directorio** y lea la información sobre las carpetas antes de crear un directorio denominado **products**.
1. En el explorador de almacenamiento, compruebe que la vista actual muestra el contenido de la carpeta **products** que acaba de crear. Observe que las rutas de navegación que se encuentran en la parte superior de la página reflejen la ruta de acceso **Contenedores de blobs > data > products**.
1. En las rutas de navegación, seleccione **data** para cambiar al contenedor **data** y observe que <u>no</u> contiene ninguna carpeta denominada **products**.

    Folders in blob storage are virtual, and only exist as part of the path of a blob. Since the <bpt id="p1">**</bpt>products<ept id="p1">**</ept> folder contained no blobs, it isn't really there!

1. Utilice el botón **&#10514; Cargar** para abrir el panel **Cargar blob**.
1. In the <bpt id="p1">**</bpt>Upload blob<ept id="p1">**</ept> panel, select the <bpt id="p2">**</bpt>product1.json<ept id="p2">**</ept> file you saved on your local computer previously. Then in the <bpt id="p1">**</bpt>Advanced<ept id="p1">**</ept> section, in the <bpt id="p2">**</bpt>Upload to folder<ept id="p2">**</ept> box, enter <bpt id="p3">**</bpt>product_data<ept id="p3">**</ept> and select the <bpt id="p4">**</bpt>Upload<ept id="p4">**</ept> button.
1. Cierre el panel **Cargar blob** si todavía está abierto y compruebe que se creó una carpeta virtual denominada **product_data** en el contenedor **data**.
1. Seleccione la carpeta **product_data** y compruebe que contiene el blob **product1.json** que cargó.
1. En el lado izquierdo, en la sección **Almacenamiento de datos**, seleccione **Contenedores**.
1. Abra el contenedor **data** y compruebe que aparece la carpeta **product_data** que creó.
1. Select the <bpt id="p1">**</bpt>&amp;#x2027;&amp;#x2027;&amp;#x2027;<ept id="p1">**</ept> icon at the right-end of the folder, and note that it doesn't display any options. Folders in a flat namespace blob container are virtual, and can't be managed.
1. Use el icono **X** que está en la parte superior derecha de la página **data** para cerrarla y vuelva a la página **Contenedores**.

## <a name="explore-azure-data-lake-storage-gen2"></a>Exploración de Azure Data Lake Storage Gen2

Azure Data Lake Store Gen2 support enables you to use hierarchical folders to organize and manage access to blobs. It also enables you to use Azure blob storage to host distributed file systems for common big data analytics platforms.

1. Descargue el archivo JSON [product2.json](https://aka.ms/product2.json?azure-portal=true) desde `https://aka.ms/product2.json` y guárdelo en el equipo en la misma carpeta en la que descargó anteriormente **product1.json** (lo cargará en el almacenamiento de blobs más adelante).
1. En la página de la cuenta de almacenamiento en Azure Portal, en el lado izquierdo, desplácese a la sección **Configuración** y seleccione **Actualización de Data Lake Gen2**.
1. In the ****Data Lake Gen2 upgrade**** page, expand and complete each step to upgrade your storage account to enable hierarchical namespace and support Azure Data Lake Storage Gen 2. This may take some time.
1. Una vez que se complete la actualización, en la sección superior del panel de la izquierda, seleccione **Explorador de almacenamiento** y navegue de vuelta a la raíz del contenedor de blobs **data**, que todavía contiene la carpeta **product_data**.
1. Seleccione la carpeta **product_data** y compruebe que todavía contiene el archivo **product1.json** que cargó anteriormente.
1. Utilice el botón **&#10514; Cargar** para abrir el panel **Cargar blob**.
1. En la página principal de Azure Portal, seleccione **&#65291; Crear un recurso** en la esquina superior izquierda y busque *Cuenta de almacenamiento*.
1. Cierre el panel **Cargar blob** si todavía está abierto y compruebe que la carpeta **product_data** ahora contiene el archivo **product2.json**.
1. En el lado izquierdo, en la sección **Almacenamiento de datos**, seleccione **Contenedores**.
1. Abra el contenedor **data** y compruebe que aparece la carpeta **product_data** que creó.
1. Seleccione el icono **&#x2027;&#x2027;&#x2027;** que aparece en el extremo derecho de la carpeta y observe que, si el espacio de nombres jerárquico está habilitado, puede hacer tareas de configuración en el nivel de carpeta, incluido el cambio de nombre de carpetas y la configuración de permisos.
1. Use el icono **X** que está en la parte superior derecha de la página **data** para cerrarla y vuelva a la página **Contenedores**.

## <a name="explore-azure-files"></a>Explorar Azure Files

Azure Files proporciona una manera de crear recursos compartidos de archivos basados en la nube.

1. En la página del contenedor de almacenamiento en Azure Portal, en la sección **Almacenamiento de datos** que aparece al lado izquierdo, seleccione **Recursos compartidos de archivos**.
1. En la página Recursos compartidos de archivos, seleccione **&#65291; Recurso compartido de archivos** y agregue un recurso compartido de archivos nuevo denominado **files** mediante el nivel **Optimizado para transacciones**.
1. En los **Recursos compartidos de archivos**, abra el recurso compartido de archivos **files** nuevo.
1. Luego, en la página **Cuenta de almacenamiento** resultante, seleccione **Crear**.
1. Cierre el panel **Conectar** y, luego, cierre la página **files** para volver a la página **Recursos compartidos de archivos** de la cuenta de Azure Storage.

## <a name="explore-azure-tables"></a>Exploración de tablas de Azure

Las tablas de Azure proporcionan un almacén de clave-valor para las aplicaciones que necesitan almacenar valores de datos, pero que no necesitan la funcionalidad y la estructura completas de una base de datos relacional.

1. En la página del contenedor de almacenamiento en Azure Portal, en la sección **Almacenamiento de datos** que aparece al lado izquierdo, seleccione **Tablas**.
1. En la página **Tablas**, seleccione **&#65291; Tabla** y cree una tabla denominada **products**.
1. Una vez que se crea la tabla **products**, en la sección superior del panel de la izquierda, seleccione **Explorador de almacenamiento**.
1. En el explorador de almacenamiento, seleccione **Tablas** y compruebe que aparece la tabla **products**.
1. Seleccione la tabla **products**.
1. En la página **product**, seleccione **&#65291; Agregar entidad**.
1. En el panel **Agregar entidad**, escriba estos valores de clave:
    - **PartitionKey**: 1
    - **RowKey**: 1
1. Seleccione **Agregar propiedad** y cree una propiedad con estos valores:

    |Nombre de propiedad | Tipo | Valor |
    | ------------ | ---- | ----- |
    | Nombre | String | Widget |

1. Agregue una segunda propiedad con estos valores:

    |Nombre de propiedad | Tipo | Value |
    | ------------ | ---- | ----- |
    | Precio | Doble | 2,99 |

1. Seleccione **Insertar** para insertar en la tabla una fila para la entidad nueva en la tabla.
1. En el explorador de almacenamiento, compruebe que se agregó una fila a la tabla **products** y que se creó una columna **Timestamp** para indicar la fecha de última modificación de la fila.
1. Agregue otra entidad a la tabla **products** con estas propiedades:

    |Nombre de propiedad | Tipo | Value |
    | ------------ | ---- | ----- |
    | PartitionKey | String | 1 |
    | RowKey | String | 2 |
    | Nombre | String | Kniknak |
    | Precio | Doble | 1,99 |
    | Descontinuado | Boolean | true |

1. Después de insertar la entidad nueva, compruebe que en la tabla se muestra una fila que contiene el producto descontinuado.

    You have manually entered data into the table using the storage browser interface. In a real scenario, application developers can use the Azure Storage Table API to build applications that read and write values to tables, making it a cost effective and scalable solution for NoSQL storage.

> **Sugerencia**: Si ha terminado de explorar Azure Storage, puede eliminar el grupo de recursos que creó en este ejercicio.
