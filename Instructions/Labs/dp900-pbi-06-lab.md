---
lab:
  title: "Exploración de los aspectos básicos de la visualización de datos con Power\_BI"
  module: Explore fundamentals of data visualization
---

# <a name="explore-fundamentals-of-data-visualization-with-power-bi"></a>Exploración de los aspectos básicos de la visualización de datos con Power BI

En este ejercicio usará Microsoft Power BI Desktop para crear un modelo de datos y un informe que contenga visualizaciones de datos interactivas.

Este laboratorio se tarda aproximadamente **20** minutos en completarse.

## <a name="before-you-start"></a>Antes de empezar

Necesitará una [suscripción de Azure](https://azure.microsoft.com/free) en la que tenga acceso de nivel administrativo.

### <a name="install-power-bi-desktop"></a>Instalar Power BI Desktop

Si Microsoft Power BI Desktop no está instalado aún en el equipo de Windows, puede descargarlo e instalarlo de forma gratuita.

1. Descargue el instalador de Power BI Desktop de [https://aka.ms/power-bi-desktop](https://aka.ms/power-bi-desktop?azure-portal=true).
1. When the file has downloaded, open it, and use the setup wizard to install Power BI Desktop on your computer. This insatllation may take a few minutes.

## <a name="import-data"></a>Importar datos

1. Open Power BI Desktop. The application interface should look similar to this:

    ![Captura de pantalla en la que se muestra la pantalla de inicio de Power BI Desktop.](images/power-bi-start.png)

    Ahora está listo para importar los datos del informe.

1. En la pantalla de inicio de sesión de Power BI Desktop, seleccione **Obtener datos** y, a continuación, en la lista de orígenes de datos, seleccione **Web** y, a continuación, **Conectar**.

    ![Captura de pantalla en la que se muestra cómo seleccionar el origen de datos web en Power BI.](images/web-source.png)

1. En el cuadro de diálogo **De web**, escriba la siguiente dirección URL y seleccione **Aceptar**:

    ```
    https://github.com/MicrosoftLearning/DP-900T00A-Azure-Data-Fundamentals/raw/master/power-bi/customers.csv
    ```

1. En el cuadro de diálogo Acceder a contenido web, seleccione **Conectar**.

1. Verify that the URL opens a dataset containing customer data, as shown below. Then select <bpt id="p1">**</bpt>Load<ept id="p1">**</ept> to load the data into the data model for your report.

    ![Captura de pantalla en la que se muestra un conjunto de datos de un cliente mostrados en Power BI.](images/customers.png)

1. En la ventana principal de Power BI Desktop, en el menú Datos, seleccione **Obtener datos** y luego **Web**:

    ![Captura de pantalla en la que se muestra el menú Obtener datos en Power BI.](images/get-data.png)

1. En el cuadro de diálogo **De web**, escriba la siguiente dirección URL y seleccione **Aceptar**:

    ```
    https://github.com/MicrosoftLearning/DP-900T00A-Azure-Data-Fundamentals/raw/master/power-bi/products.csv
    ```

1. En el cuadro de diálogo, seleccione **Cargar** para cargar los datos del producto de este archivo en el modelo de datos.

1. Repita los tres pasos anteriores para importar un tercer conjunto de datos que contenga datos de pedido de la siguiente dirección URL:

    ```
    https://github.com/MicrosoftLearning/DP-900T00A-Azure-Data-Fundamentals/raw/master/power-bi/orders.csv
    ```

## <a name="explore-a-data-model"></a>Exploración de un modelo de datos

Las tres tablas de datos que ha importado se han cargado en un modelo de datos, que ahora explorará y refinará.

1. In Power BI Desktop, on the left-side edge, select the <bpt id="p1">**</bpt>Model<ept id="p1">**</ept> tab, and then arrange the tables in the model so you can see them. You can hide the panes on the right side by using the <bpt id="p1">**</bpt><ph id="ph1">&gt;&gt;</ph><ept id="p1">**</ept> icons:

    ![Captura de pantalla en la que se muestra la pestaña Modelo en Power BI.](images/model-tab.png)

1. En la tabla **orders** (pedidos), seleccione el campo **Revenue** (Ingresos) y, en el panel **Propiedades**, establezca su propiedad **Formato** en **Moneda**:

    ![Captura de pantalla en la que se muestra cómo establecer el formato Ingresos en Moneda en Power BI.](images/revenue-currency.png)

    Este paso garantizará que los valores de ingresos se muestren como moneda en las visualizaciones de informes.

1. In the products table, right-click the <bpt id="p1">**</bpt>Category<ept id="p1">**</ept> field (or open its <bpt id="p2">**</bpt><ph id="ph1">&amp;vellip;</ph><ept id="p2">**</ept> menu) and select <bpt id="p3">**</bpt>Create hierarchy<ept id="p3">**</ept>. This step creates a hierarchy named <bpt id="p1">**</bpt>Category Hierarchy<ept id="p1">**</ept>. You may need to expand or scroll in the <bpt id="p1">**</bpt>products<ept id="p1">**</ept> table to see this - you can also see it in the <bpt id="p2">**</bpt>Fields<ept id="p2">**</ept> pane:

    ![Captura de pantalla en la que se muestra cómo agregar Jerarquía de categoría en Power BI.](images/category-hierarchy.png)

1. In the products table, right-click the <bpt id="p1">**</bpt>ProductName<ept id="p1">**</ept> field (or open its <bpt id="p2">**</bpt><ph id="ph1">&amp;vellip;</ph><ept id="p2">**</ept> menu) and select <bpt id="p3">**</bpt>Add to hierarchy<ept id="p3">**</ept><ph id="ph2"> &gt; </ph><bpt id="p4">**</bpt>Category Hierarchy<ept id="p4">**</ept>. This adds the <bpt id="p1">**</bpt>ProductName<ept id="p1">**</ept> field to the hierarchy you created previously.
1. In the <bpt id="p1">**</bpt>Fields<ept id="p1">**</ept> pane, right-click <bpt id="p2">**</bpt>Category Hierarchy<ept id="p2">**</ept> (or open its <bpt id="p3">**</bpt>...<ept id="p3">**</ept> menu) and select <bpt id="p4">**</bpt>Rename<ept id="p4">**</ept>. Then rename the hierarchy to <bpt id="p1">**</bpt>Categorized Product<ept id="p1">**</ept>.

    ![Captura de pantalla en la que se muestra cómo cambiar el nombre de la jerarquía en Power BI.](images/rename-hierarchy.png)

1. En el borde izquierdo, seleccione la pestaña **Datos** y, a continuación, en el panel **Campos**, seleccione la tabla **customers** (clientes).
1. Seleccione el encabezado de columna **Ciudad** y, a continuación, establezca su propiedad **Categoría de datos** en **Ciudad**:

    ![Captura de pantalla en la que se muestra cómo establecer una categoría de datos en Power BI.](images/data-category.png)

    Este paso garantizará que los valores de esta columna se interpreten como nombres de ciudad, lo que puede ser útil si piensa incluir visualizaciones de mapa.

## <a name="create-a-report"></a>Creación de un informe

Now you're almost ready to create a report. First you need to check some settings to ensure all visualizations are enabled.

1. On the <bpt id="p1">**</bpt>File<ept id="p1">**</ept> menu, select <bpt id="p2">**</bpt>Options and Settings<ept id="p2">**</ept>. Then select <bpt id="p1">**</bpt>Options<ept id="p1">**</ept>, and in the <bpt id="p2">**</bpt>Security<ept id="p2">**</ept> section, ensure that <bpt id="p3">**</bpt>Use Map and Filled Map visuals<ept id="p3">**</ept> is enabled and select <bpt id="p4">**</bpt>OK<ept id="p4">**</ept>.

    ![Captura de pantalla en la que se muestra cómo establecer la propiedad Usar objetos visuales de mapa y de mapa rellenado en Power BI.](images/set-options.png)

    Esta configuración garantiza que pueda incluir visualizaciones de mapas en los informes.

1. En el borde izquierdo, seleccione la pestaña **Informe** y fíjese en la interfaz de diseño del informe.

    ![Captura de pantalla en la que se muestra la pestaña Informe en Power BI.](images/report-tab.png)

1. In the ribbon, above the report design surface, select <bpt id="p1">**</bpt>Text Box<ept id="p1">**</ept> and add a text box containing the text <bpt id="p2">**</bpt>Sales Report<ept id="p2">**</ept> to the report. Format the text to make it bold with a font size of 32.

    ![Captura de pantalla en la que se muestra cómo agregar un cuadro de texto en Power BI.](images/text-box.png)

1. Cuando el archivo se haya descargado, ábralo e instale Power BI Desktop en el equipo mediante el asistente para instalación.

    ![Captura de pantalla en la que se muestra cómo agregar una tabla de productos clasificados a un informe en Power BI.](images/categorized-products-table.png)

1. Esta instalación puede tardar unos minutos.

    The revenue is formatted as currency, as you specified in the model. However, you didn't specify the number of decimal places, so the values include fractional amounts. It won't matter for the visualizations you're going to create, but you could go back to the <bpt id="p1">**</bpt>Model<ept id="p1">**</ept> or <bpt id="p2">**</bpt>Data<ept id="p2">**</ept> tab and change the decimal places if you wish.

    ![Captura de pantalla en la que se muestra una tabla de productos clasificados con ingresos en un informe.](images/revenue-column.png)

1. With the table still selected, in the <bpt id="p1">**</bpt>Visualizations<ept id="p1">**</ept> pane, select the <bpt id="p2">**</bpt>Stacked column chart<ept id="p2">**</ept> visualization. The table is changed to a column chart showing revenue by category.

    ![Captura de pantalla en la que se muestra un gráfico de columnas apiladas de productos clasificados con ingresos en un informe.](images/stacked-column-chart.png)

1. Abra Power BI Desktop.

    ![Captura de pantalla en la que se muestra un gráfico de columnas explorado en profundidad para ver los productos de una categoría.](images/drill-down.png)

1. La interfaz de la aplicación debe tener un aspecto similar al siguiente:
1. Select a blank area of the report, and then in the <bpt id="p1">**</bpt>Fields<ept id="p1">**</ept> pane, select the <bpt id="p2">**</bpt>Quantity<ept id="p2">**</ept> field in the <bpt id="p3">**</bpt>orders<ept id="p3">**</ept> table and the <bpt id="p4">**</bpt>Category<ept id="p4">**</ept> field in the <bpt id="p5">**</bpt>products<ept id="p5">**</ept> table. This step results in another column chart showing sales quantity by product category.
1. Con el nuevo gráfico de columnas seleccionado, en el panel **Visualizaciones**, seleccione **Gráfico circular** y, a continuación, cambie el tamaño del gráfico y colóquelo junto al gráfico de columnas de ingresos por categoría.

    ![Captura de pantalla en la que se muestra un gráfico circular en el que se muestra la cantidad de ventas por categoría.](images/category-pie-chart.png)

1. Select a blank area of the report, and then in the <bpt id="p1">**</bpt>Fields<ept id="p1">**</ept> pane, select the <bpt id="p2">**</bpt>City<ept id="p2">**</ept> field in the <bpt id="p3">**</bpt>customers<ept id="p3">**</ept> table and then select the <bpt id="p4">**</bpt>Revenue<ept id="p4">**</ept> field in the <bpt id="p5">**</bpt>orders<ept id="p5">**</ept> table. This results in a map showing sales revenue by city. Rearrange and resize the visualizations as needed:

    ![Captura de pantalla en la que se muestra un mapa en el que se muestran los ingresos por ciudad.](images/revenue-map.png)

1. In the map, note that you can drag, double-click, use a mouse-wheel, or pinch and drag on a touch screen to interact. Then select a specific city, and note that the other visualizations in the report are modified to highlight the data for the selected city.

    ![Captura de pantalla en la que se muestra un mapa en el que se muestran los ingresos por ciudad resaltando los datos de la ciudad seleccionada.](images/selected-data.png)

1. On the <bpt id="p1">**</bpt>File<ept id="p1">**</ept> menu, select <bpt id="p2">**</bpt>Save<ept id="p2">**</ept>. Then save the file with an appropriate .pbix file name. You can open the file and explore data modeling and visualization further at your leisure.

Si tiene una suscripción a un [servicio Power BI](https://www.powerbi.com/?azure-portal=true), puede iniciar sesión en su cuenta y publicar el informe en un área de trabajo de Power BI. 
