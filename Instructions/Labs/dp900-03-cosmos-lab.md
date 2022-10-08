---
lab:
  title: "Exploración de Azure Cosmos\_DB"
  module: Explore fundamentals of Azure Cosmos DB
---
# <a name="explore-azure-cosmos-db"></a>Exploración de Azure Cosmos DB

En este ejercicio aprovisionará una base de datos de Azure Cosmos DB en su suscripción de Azure y explorará las distintas formas en que puede usarla para almacenar datos no relacionales.

Este laboratorio se tarda aproximadamente **15** minutos en completarse.

## <a name="before-you-start"></a>Antes de empezar

Necesitará una [suscripción de Azure](https://azure.microsoft.com/free) en la que tenga acceso de nivel administrativo.

## <a name="create-a-cosmos-db-account"></a>Creación de una cuenta de Cosmos DB

Para usar Cosmos DB, debe aprovisionar una cuenta de Cosmos DB en su suscripción de Azure. En este ejercicio, aprovisionará una cuenta de Cosmos DB que usa la API de núcleo (SQL).

1. En Azure Portal, seleccione **+ Crear un recurso** en la parte superior izquierda y busque *Azure Cosmos DB*.  En los resultados, seleccione **Azure Cosmos DB** y luego **Crear**.
1. En el mosaico **Núcleo (SQL): Recomendado**, seleccione **Crear**.
1. Escriba los detalles siguientes y seleccione **Revisar y crear**: 
    - **Suscripción**: si usa un espacio aislado, seleccione la opción *Concierge Subscription* (Suscripción de Concierge). En caso contrario, seleccione su suscripción de Azure.
    - **Grupo de recursos**: si usa un espacio aislado, seleccione el grupo de recursos existente (que tendrá un nombre como *learn-xxxx…* ). De lo contrario, cree un grupo de recursos con el nombre que prefiera.
    - **Nombre de cuenta**: escriba un nombre único
    - **Ubicación**: elija cualquier ubicación recomendada.
    - **Capacity mode** (Modo de capacidad): rendimiento aprovisionado
    - **Apply Free-Tier Discount** (Aplicar descuento de nivel Gratis): seleccione Aplicar si está disponible
    - **Limit total account throughput** (Limitar el rendimiento total de la cuenta): no seleccionado
1. Una vez validada la configuración, seleccione **Crear**.
1. Espere a que la implementación finalice. A continuación, vaya al recurso implementado.

## <a name="create-a-sample-database"></a>Crear una base de datos de ejemplo

*Durante este procedimiento, cierre las sugerencias que se muestran en el portal*.

1. En la página de la nueva cuenta de Cosmos DB, en el panel de la izquierda, seleccione **Explorador de datos**.
1. En la página **Explorador de datos**, seleccione **Inicio rápido**.
1. En la pestaña **Nuevo contenedor**, revise la configuración rellenada previamente para la base de datos de ejemplo y, a continuación, seleccione **Aceptar**.
1. Observe el estado en el panel de la parte inferior de la pantalla hasta que se haya creado la base de datos **SampleDB** y su contenedor **SampleContainer** (esta acción puede tardar unos minutos).

## <a name="view-and-create-items"></a>Visualización y creación de elementos

1. En la página del Explorador de datos, expanda la base de datos **SampleDB** y el contenedor **SampleContainer** y seleccione **Elementos** para ver una lista de los elementos del contenedor. Los elementos representan direcciones, cada una con un identificador único y otras propiedades.
1. Seleccione cualquiera de los elementos de la lista para ver una representación JSON de los datos del elemento.
1. En la parte superior de la página, seleccione **Nuevo elemento** para crear un nuevo elemento en blanco.
1. Modifique el JSON del nuevo elemento como se muestra a continuación y, posteriormente, seleccione **Guardar**.

    ```json
    {
        "address": "1 Any St.",
        "id": "123456789"
    }
    ```

1. Después de guardar el nuevo elemento, observe que las propiedades de metadatos adicionales se agregan automáticamente.

## <a name="query-the-database"></a>Consulta de la base de datos

1. En la página del **Explorador de datos**, seleccione el icono **Nueva consulta de SQL**.
1. En el editor de consultas SQL, revise la consulta predeterminada (`SELECT * FROM c`) y use el botón `SELECT * FROM c` para ejecutarla.
1. Revise los resultados, que incluyen la representación JSON completa de todos los elementos.
1. Modifique la consulta del siguiente modo:

    ```sql
    SELECT c.id, c.address
    FROM c
    WHERE CONTAINS(c.address, "Any St.")
    ```

1. Use el botón **Ejecutar consulta** para ejecutar la consulta revisada y analizar los resultados, lo que incluye las entidades JSON para cualquier elemento con un campo de **dirección** que contenga el texto "Any St.".
1. Cierre el editor de consultas SQL y descarte los cambios.

    Ha visto cómo crear y consultar entidades JSON en una base de datos de Cosmos DB mediante la interfaz del explorador de datos de Azure Portal. En un escenario real, un desarrollador de aplicaciones utilizará uno de los muchos kits de desarrollo de software (SDK) específicos del lenguaje de programación para llamar a la API de núcleo (SQL) y trabajar con datos en la base de datos.

> **Sugerencia**: Si ha terminado de explorar Azure Cosmos DB, puede eliminar el grupo de recursos que creó en este ejercicio.
