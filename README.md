**CARACTERIZACIÓN Y SEGMENTACIÓN DE PROVEEDORES POR MEDIO DE MODELOS RELACIONALES Y DE CLUSTERING**

**Planteamiento requerimientos de negocio**

El entregable propuesto a Frubana consta de dos elementos:

Un elemento de visualización y caracterización de cada uno de los proveedores de acuerdo con la información disponible.
Un elemento de agrupamiento y clasificación de proveedores de acuerdo con la información disponible

Para el elemento de visualización y caracterización, resulta conveniente hacer uso de métodos descriptivos en dos escalas: Una escala absoluta y una escala temporal. La estimación de promedios, portafolios de productos y acumulado de pedidos permite realizar una caracterización de las capacidades productivas del proveedor, mientras que una evaluación a nivel temporal permite evidenciar la evolución de indicadores estratégicos que evalúen la relación comercial con los distintos proveedores.

Las métricas seleccionadas para evaluar con un corte transversal son el volumen de pedidos atendidos, el volumen de productos ofrecidos, el volumen de productos con perfiles de descomposición distintos (importante para análisis de temas de logística de almacenamiento), así como el pedido máximo y el pedido mínimo histórico que puede suplir dicho proveedor.

La métrica seleccionada para evidenciar la evolución histórica de la relación con el proveedor, así como el impacto económico de la selección de proveedores, es la diferencia porcentual del precio de compra por unidad con el precio por unidad reportado por el SIPSA (Sistema de Información de Precios y Abastecimiento del Sector Agropecuario). Dicha información se construyó realizando una estandarización de los nombres de producto y los nombres reportados en las bases de Frubana, y posteriormente realizando un cruce por fecha de compra y nombre entre ambas bases. Para esta métrica, cabe destacar que para ciertos productos nativos de las regiones (como el níspero o el agraz), no se encuentra información en el SIPSA, por lo que se estimó una diferencia de 0 entre el valor de mercado y el valor del proveedor. Sin embargo, para el desarrollo final se propone una búsqueda más completa en otras bases de precios con el fin de obtener la información completa.

En el caso de las métricas de agrupamiento y clasificación, al no existir una clasificación en Frubana, se hará uso de modelos predictivos de tipo clustering, con el fin de observar agrupaciones entre los datos por similitud, así como distinguir las principales características que definen a estos grupos en función de la información existente dentro de las bases de datos. Ya que corresponde a una primera aproximación de los grupos, se realizará con un corte transversal, y en el caso de la métrica de diferencia porcentual de precios se hará un promedio del corte histórico hasta la fecha. En este caso puntual debe tenerse cuidado con el análisis de dicho valor para los productos de los que no se posee información al interior del SIPSA, ya que puede sugerirse seguir con un proveedor por desinformación mas no por conveniencia para Frubana.
Alternativa para el problema descriptivo, visualización y caracterización de proveedores con la información disponible.

Para solucionar el problema descriptivo se propone un proyecto de BI (Business Intelligence) pequeño, el cual contiene los siguientes componentes:

- Fuentes de información disponibles Frubana:  Archivos csv de Compras, Productos y del Tiempo promedio de vida.
- Fuentes de información complementaria: Archivos csv DANE, y archivos planos del SIPSA (Sistema de Información de Precios y Abastecimiento del Sector Agropecuario).
- Herramienta de ETL: Se realiza un proceso de extracción, transformación y carga con el fin de limpiar, transformar e integrar los archivos fuente en una bodega de datos.
- Bodega de datos: Con base en el diseño de un modelo relacional tipo estrella, se materializan las tablas que conforman el modelo relacional con el fin de tener la información centralizada y que sean base para cálculos y métricas.
-Reporte Inteligente: Se generan cálculos, medidas, visualizaciones, comparaciones y datos descriptivos en un reporte inteligente que permite al usuario interactuar con la información.

La implementación de un proyecto de Business Intelligence (BI) conlleva una serie de ventajas significativas para Frubana. En primer lugar, proporciona una visión integral y en tiempo real de los datos empresariales, lo que permite una toma de decisiones más informada y estratégica. Al centralizar y organizar la información de diversas fuentes, el BI facilita la identificación de tendencias, patrones y oportunidades de mejora, lo que puede resultar en una mayor eficiencia operativa y una ventaja competitiva en el mercado. Además, al generar reportes inteligentes y análisis detallados, el BI ayuda a optimizar los procesos internos y a identificar áreas de inversión rentable.

**Modelo Relacional de Compras Fruba:**

El modelo relacional de Compras de Frubana contiene una estructura organizada de datos que representa las relaciones entre diferentes entidades dentro de la empresa, como proveedores, productos, Georreferencia, Tiempo y transacciones del negocio como compras, además fue complementado con información del DANE para ayudar a describir la locación y con los precios del SIPSA para poder generar las métricas de comparación de precios.

Se diseñó el siguiente modelo:

