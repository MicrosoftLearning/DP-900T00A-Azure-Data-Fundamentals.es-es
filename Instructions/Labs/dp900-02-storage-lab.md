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
1. En la página principal de Azure Portal, seleccione **&#65291; Crear un recurso** en la esquina superior izquierda y busque *Cuenta de almacenamiento*. Luego, en la página **Cuenta de almacenamiento** resultante, seleccione **Crear**.
1. Escriba los valores siguientes en la página **Crear una cuenta de almacenamiento**:
    - **Suscripción**: Seleccione su suscripción a Azure.
    - **Grupo de recursos**: cree un grupo de recursos con el nombre que prefiera.
    - **Nombre de la cuenta de almacenamiento**: escriba un nombre único para la cuenta de almacenamiento con números y letras minúsculas.
    - **Región**: seleccione cualquier ubicación disponible.
    - **Rendimiento**: *Estándar*
    - **Redundancia**: *almacenamiento con redundancia local (LRS)*

1. Seleccione **Siguiente: Opciones avanzadas >** y vea las opciones de configuración avanzada. En concreto, tenga en cuenta que es así donde puede habilitar el espacio de nombres jerárquico para admitir Azure Data Lake Storage Gen2. Deje esta opción **<u>sin seleccionar</u>** (la habilitará más adelante) y seleccione **Siguiente: Redes >** para conocer las opciones de redes correspondientes a la cuenta de almacenamiento.
1. Seleccione **Siguiente: Protección de datos >** y, luego, en la sección **Recuperación**, <u>anule la </u>selección de todas las opciones **Habilitar eliminación temporal…** . Estas opciones conservan los archivos eliminados para su posterior recuperación, pero pueden causar problemas más adelante cuando se habilite el espacio de nombres jerárquico.
1. Continúe por el resto de las páginas **Siguiente >** sin cambiar la configuración predeterminada y, luego, en la página **Revisar y crear**, espere la validación de sus selecciones y seleccione **Crear** para crear una cuenta de Azure Storage.
1. Espere a que la implementación finalice. Luego, vaya al recurso que se implementó.

## <a name="explore-blob-storage"></a>Exploración de almacenamiento de blobs

Ahora que tiene una cuenta de Azure Storage, puede crear un contenedor para los datos de blobs.

1. Descargue el archivo JSON [product1.json](https://aka.ms/product1.json?azure-portal=true) desde `https://aka.ms/product1.json` y guárdelo en el equipo (puede guardarlo en cualquier carpeta, porque lo cargará en el almacenamiento de blobs más adelante).

    *Si el archivo JSON aparece en el explorador, guarde la página como **product1.json**.*

1. En la página del contenedor de almacenamiento en Azure Portal, en la sección **Almacenamiento de datos** que aparece al lado izquierdo, seleccione **Contenedores**.
1. En la página **Contenedores**, seleccione **&#65291; Contenedor** y agregue un contenedor nuevo denominado **data** con un nivel de acceso público de **Privado (sin acceso anónimo)** .
1. Una vez creado el contenedor **data**, compruebe que aparece en la página **Contenedores**.
1. En la sección superior del panel de la izquierda, seleccione **Explorador de almacenamiento**. En esta página, se proporciona una interfaz basada en explorador que puede utilizar para trabajar con los datos de la cuenta de almacenamiento.
1. En la página del explorador de almacenamiento, seleccione **Contenedores de blobs** y compruebe que aparece el contenedor **data**.
1. Seleccione el contenedor **data** y observe que está vacío.
1. Seleccione **&#65291; Agregar directorio** y lea la información sobre las carpetas antes de crear un directorio denominado **products**.
1. En el explorador de almacenamiento, compruebe que la vista actual muestra el contenido de la carpeta **products** que acaba de crear. Observe que las rutas de navegación que se encuentran en la parte superior de la página reflejen la ruta de acceso **Contenedores de blobs > data > products**.
1. En las rutas de navegación, seleccione **data** para cambiar al contenedor **data** y observe que <u>no</u> contiene ninguna carpeta denominada **products**.

    Las carpetas del almacenamiento de blobs son virtuales y solo existen como parte de la ruta de acceso de un blob. Como la carpeta **products** no contiene ningún blob, en realidad no existe.

1. Utilice el botón **&#10514; Cargar** para abrir el panel **Cargar blob**.
1. En el panel **Cargar blob**, seleccione el archivo **product1.json** que guardó anteriormente en el equipo local. Luego, en la sección **Opciones avanzadas**, en el cuadro **Cargar en carpeta**, escriba **product_data** y seleccione el botón **Cargar**.
1. Cierre el panel **Cargar blob** si todavía está abierto y compruebe que se creó una carpeta virtual denominada **product_data** en el contenedor **data**.
1. Seleccione la carpeta **product_data** y compruebe que contiene el blob **product1.json** que cargó.
1. En el lado izquierdo, en la sección **Almacenamiento de datos**, seleccione **Contenedores**.
1. Abra el contenedor **data** y compruebe que aparece la carpeta **product_data** que creó.
1. Seleccione el icono **&#x2027;&#x2027;&#x2027;** que aparece en el extremo derecho de la carpeta y observe que no muestra ninguna opción. Las carpetas de un contenedor de blobs de espacio de nombres plano son virtuales y no se pueden administrar.
1. Use el icono **X** que está en la parte superior derecha de la página **data** para cerrarla y vuelva a la página **Contenedores**.

## <a name="explore-azure-data-lake-storage-gen2"></a>Exploración de Azure Data Lake Storage Gen2

La compatibilidad con Azure Data Lake Store Gen2 le permite usar carpetas jerárquicas para organizar y administrar el acceso a los blobs. También le permite utilizar Azure Blob Storage para hospedar sistemas de archivos distribuidos para plataformas comunes de análisis de macrodatos.

1. Descargue el archivo JSON [product2.json](https://aka.ms/product2.json?azure-portal=true) desde `https://aka.ms/product2.json` y guárdelo en el equipo en la misma carpeta en la que descargó anteriormente **product1.json** (lo cargará en el almacenamiento de blobs más adelante).
1. En la página de la cuenta de almacenamiento en Azure Portal, en el lado izquierdo, desplácese a la sección **Configuración** y seleccione **Actualización de Data Lake Gen2**.
1. En la página **Actualización de Data Lake Gen2**, expanda y complete cada paso para actualizar la cuenta de almacenamiento a fin de habilitar el espacio de nombres jerárquico y admitir Azure Data Lake Storage Gen2. Esto puede llevar algo de tiempo.
1. Una vez que se complete la actualización, en la sección superior del panel de la izquierda, seleccione **Explorador de almacenamiento** y navegue de vuelta a la raíz del contenedor de blobs **data**, que todavía contiene la carpeta **product_data**.
1. Seleccione la carpeta **product_data** y compruebe que todavía contiene el archivo **product1.json** que cargó anteriormente.
1. Utilice el botón **&#10514; Cargar** para abrir el panel **Cargar blob**.
1. En el panel **Cargar blob**, seleccione el archivo **product2.json** que guardó en el equipo local. Luego, seleccione el botón **Cargar**.
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
1. En la parte superior de la página, seleccione **Conectar**. Luego, en el panel **Conectar**, observe que hay pestañas para los sistemas operativos comunes (Windows, Linux y macOS) que contienen scripts que puede ejecutar para conectarse a la carpeta compartida desde un equipo cliente.
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

    |Nombre de propiedad | Tipo | Value |
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

    Escribió los datos en la tabla con la interfaz del explorador de almacenamiento. En un escenario real, los desarrolladores de aplicaciones pueden la Azure Storage Table API para compilar aplicaciones que leen y escriben valores en tablas, lo que la hace una solución rentable y escalable para el almacenamiento NoSQL.

> **Sugerencia**: Si ha terminado de explorar Azure Storage, puede eliminar el grupo de recursos que creó en este ejercicio.
