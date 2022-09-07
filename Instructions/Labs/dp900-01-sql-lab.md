---
lab:
  title: Exploración de Azure SQL Database
  module: Explore relational data in Azure
---

# <a name="explore-azure-sql-database"></a>Exploración de Azure SQL Database

En este ejercicio, aprovisionará un recurso de Azure SQL Database en la suscripción de Azure y, a continuación, usará SQL para consultar las tablas de una base de datos relacional.

Este laboratorio se tarda aproximadamente **15** minutos en completarse.

## <a name="before-you-start"></a>Antes de empezar

Necesitará una [suscripción de Azure](https://azure.microsoft.com/free) en la que tenga acceso de nivel administrativo.

## <a name="provision-an-azure-sql-database-resource"></a>Aprovisionamiento de un recurso de Azure SQL Database

1. In the <bpt id="p1">[</bpt>Azure portal<ept id="p1">](https://portal.azure.com?azure-portal=true)</ept>, select <bpt id="p2">**</bpt>&amp;#65291; Create a resource<ept id="p2">**</ept> from the upper left-hand corner and search for <bpt id="p3">*</bpt>Azure SQL<ept id="p3">*</ept>. Then in the resulting <bpt id="p1">**</bpt>Azure SQL<ept id="p1">**</ept> page, select <bpt id="p2">**</bpt>Create<ept id="p2">**</ept>.

1. Revise las opciones de Azure SQL disponibles y, luego, en el icono **Bases de datos SQL**, asegúrese de que está seleccionado **Base de datos única** y seleccione **Crear**.

    ![Captura de pantalla de Azure Portal en la que se muestra la página Azure SQL.](images//azure-sql-portal.png)

1. Escriba los valores siguientes en la página **Crear base de datos SQL**:
    - **Suscripción**: Seleccione su suscripción a Azure.
    - **Grupo de recursos**: cree un grupo de recursos con el nombre que prefiera.
    - **Nombre de la base de datos**: *AdventureWorks*.
    - <bpt id="p1">**</bpt>Server<ept id="p1">**</ept>:  Select <bpt id="p2">**</bpt>Create new<ept id="p2">**</ept> and create a new server with a unique name in any available location. Use <bpt id="p1">**</bpt>SQL authentication<ept id="p1">**</ept> and specify your name as the server admin login and a suitably complex password (remember the password - you'll need it later!)
    - **¿Quiere usar un grupo elástico de SQL?**: *No*.
    - **Proceso y almacenamiento**: no lo cambie.
    - **Redundancia de almacenamiento de Backup**: seleccione *Locally-redundant backup storage* (Almacenamiento de copia de seguridad con redundancia local).

1. On the <bpt id="p1">**</bpt>Create SQL Database<ept id="p1">**</ept> page, select <bpt id="p2">**</bpt>Next :Networking &gt;<ept id="p2">**</ept>, and on the <bpt id="p3">**</bpt>Networking<ept id="p3">**</ept> page, in the <bpt id="p4">**</bpt>Network connectivity<ept id="p4">**</ept> section, select <bpt id="p5">**</bpt>Public endpoint<ept id="p5">**</ept>. Then select <bpt id="p1">**</bpt>Yes<ept id="p1">**</ept> for both options in the <bpt id="p2">**</bpt>Firewall rules<ept id="p2">**</ept> section to allow access to your database server from Azure services and your current client IP address.

1. Seleccione **Siguiente: Seguridad >** y establezca la opción **Enable Microsoft Defender for SQL** (Habilitar Microsoft Defender para SQL) en **Ahora no**.

1. Seleccione **Siguiente: Configuración adicional >** y, en la pestaña **Configuración adicional**, establezca la opción **Usar datos existentes** en **Ejemplo** (esto creará una base de datos de ejemplo que puede explorar más adelante).

1. Seleccione **Revisar y crear** y, luego, **Crear** para crear la base de datos de Azure SQL.

1. Wait for deployment to complete. Then go to the resource that was deployed, which should look like this:

    ![Captura de pantalla de Azure Portal en la que se muestra la página SQL Database.](images//sql-database-portal.png)

1. En el panel del lado izquierdo de la página, seleccione **Editor de consultas (versión preliminar)** e inicie sesión con el inicio de sesión de administrador y la contraseña que especificó para el servidor.
    
    *Si se muestra un mensaje de error que indica que no se permite la dirección IP del cliente, seleccione el vínculo **Allowlist IP…** (IP de la lista de permitidos…) al final del mensaje para permitir el acceso e intente iniciar sesión de nuevo (antes agregó la dirección IP de cliente de su propio equipo a las reglas de firewall, pero el editor de consultas podría conectarse desde otra dirección, en función de la configuración de red).*
    
    El editor de consultas tiene el aspecto siguiente:
    
    ![Captura de pantalla de Azure Portal en la que se muestra el editor de consultas.](images//query-editor.png)

1. Expanda la carpeta **Tablas** para ver las tablas de la base de datos.

1. En el panel **Consulta 1**, escriba el siguiente código SQL:

    ```sql
    SELECT * FROM SalesLT.Product;
    ```

1. Seleccione **&#9655; Ejecutar** encima de la consulta para ejecutarla y ver los resultados, que deberían incluir todas las columnas de todas las filas de la tabla **SalesLT.Product**, tal como se muestra aquí:

    ![Captura de pantalla de Azure Portal en la que se muestra el editor de consultas con los resultados de la consulta.](images//sql-query-results.png)

1. Reemplace la instrucción SELECT por el código siguiente y, luego, seleccione **&#9655; Ejecutar** para ejecutar la nueva consulta y revisar los resultados (se incluyen solo las columnas **ProductID**, **Name**, **ListPrice** y **ProductCategoryID**):

    ```sql
    SELECT ProductID, Name, ListPrice, ProductCategoryID
    FROM SalesLT.Product;
    ```

1. Ahora pruebe la consulta siguiente, que usa JOIN para obtener el nombre de categoría de la tabla **SalesLT.ProductCategory**:

    ```sql
    SELECT p.ProductID, p.Name AS ProductName,
            c.Name AS Category, p.ListPrice
    FROM SalesLT.Product AS p
    JOIN [SalesLT].[ProductCategory] AS c
        ON p.ProductCategoryID = c.ProductCategoryID;
    ```

1. Cierre el panel del editor de consultas y descarte las modificaciones.

> **Sugerencia**: Si ha terminado de explorar Azure SQL Database, puede eliminar el grupo de recursos que creó en este ejercicio.
