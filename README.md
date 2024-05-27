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
![image](https://github.com/demonory/PAAD_G17/assets/129700342/9e87d5b7-dae7-4eee-b600-3418d3d4dec7)

Las pruebas de conexión a las fuentes de datos, limpieza y transformación por medio de ETL, implementación del modelo relacional y visualización se han realizado por el momento con base en la siguiente arquitectura de BI:

![image](https://github.com/demonory/PAAD_G17/assets/129700342/3f3de50d-8014-46ff-9ade-93dc7bb15fec)

Para la parte de pruebas se conecta Power BI Desktop directamente a las fuentes de Frubana, fuentes externas y al archivo csv resultante después de aplicar los modelos de segmentación utilizando Python. En este caso se utiliza Power Query como herramienta de ETL, Modelos semánticos de Power BI como herramienta de DWH o bodega, y Power BI Service como herramienta de visualización, como se puede observar en el diseño por el momento nada sale de la suite de Power BI.

Siguiendo esta arquitectura se obtuvieron las primeras visualizaciones observadas en el MockUp presentado.

![image](https://github.com/demonory/PAAD_G17/assets/129700342/741adf33-c8ca-4725-ba3a-341e499b6dc3)

Alternativas para el problema de clasificación y agrupamiento.

Para solucionar el problema de clasificación y agrupamiento, se realizará una evaluación de los métodos más comunes de clustering:

*   K-means
*   Clustering jerárquico
*   DBScan
*   Gaussian Mixture Model (GMM)
*   K-medioides

Estos métodos cuentan con la ventaja de ser de fácil implementación, además de permitir una caracterización inicial de los proveedores, el cual es el objetivo de esta primera etapa. Como desventaja, necesitan una recalibración frecuente tanto con la entrada de nuevos proveedores como con la actualización de información histórica de los proveedores actuales. Sin embargo, con esta primera aproximación se puede generar un grupo de categorías y características fijas, y posteriormente utilizar métodos más robustos de clasificación (como redes neuronales), que permitan clasificar con mayor detalle cada uno de los métodos planteados.

También cabe destacar que en esta primera etapa, al ser una base de datos de un tamaño pequeño (141 proveedores con 6 características), no se considera necesario realizar una reducción de dimensionalidad de las características. En caso de que en el futuro se agreguen más características, se recomienda primero realizar un algoritmo de reducción de dimensionalidad como PCA y luego realizar dicho agrupamiento, aunque esto puede reducir la interpretabilidad de los factores que contribuyen al agrupamiento buscado.

Verificación de supuestos y preparación de datos para algoritmos de clustering

Los algoritmos de clustering no requieren una preparación rigurosa de la base de datos, en este caso sólo requieren que se utilicen variables numéricas (para el cálculo de distancias), así como que no se cuente con información faltante. A continuación, se presenta la base de datos de los 140 proveedores disponibles, junto con las variables utilizadas para la clasificación. Como puede observarse, no se cuentan con datos faltantes o fuera de formato, por lo que puede procederse con la clasificación.


query_1 = """
SELECT a.*, b.Diff as

 'DiffPromedio'
FROM (
      SELECT supplier_id, 
      COUNT(*) AS CountDeCompras, 
      SUM(total_price) AS SumTotalPrice, 
      AVG(total_price) AS AvgTotalPrice, 
      MIN(total_price) AS MinTotalPrice, 
      MAX(total_price) AS MaxTotalPrice, 
      AVG(peso_prom) AS AvgPesoProm, 
      AVG(DATEDIFF(DAY,CAST(purchase_date AS DATE), CAST(received_date AS DATE))) AS AvgTiemSumi
      FROM base_compras
      GROUP BY supplier_id
) a
LEFT JOIN (
      SELECT supplier_id, AVG((price_diff)) as 'Diff'
      FROM (
              SELECT supplier_id, purchase_date, product_id, purchase_unit_price, unit_price, 
              (purchase_unit_price - unit_price)/unit_price AS price_diff
              FROM base_compras
              INNER JOIN sipsa
              ON base_compras.product_id = sipsa.product_id
              AND base_compras.purchase_date = sipsa.report_date
              ) AS subquery
      GROUP BY supplier_id
      ) b
ON a.supplier_id = b.supplier_id
"""

df_final = run_query(query_1)

Reemplazo de valores nulos
df_final.fillna(0, inplace=True)

print(df_final.head())

Agrupamiento utilizando K-means

El algoritmo de K-means es un método iterativo que busca dividir un conjunto de datos en K grupos diferentes de acuerdo con sus características. Es especialmente útil para identificar patrones y estructuras en los datos que no son inmediatamente evidentes. 

Definición de parámetros y ejecución del modelo K-means

from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA

Definición de características y estandarización de datos
X = df_final[['CountDeCompras', 'SumTotalPrice', 'AvgTotalPrice', 'MinTotalPrice', 'MaxTotalPrice', 'AvgPesoProm', 'AvgTiemSumi', 'DiffPromedio']]
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

Reducción de dimensionalidad para visualización
pca = PCA(n_components=2)
X_pca = pca.fit_transform(X_scaled)
K-means con 3 clusters
kmeans = KMeans(n_clusters=3, random_state=42)
clusters = kmeans.fit_predict(X_scaled)
Agregar resultados al dataframe
df_final['Cluster'] = clusters

Visualización de clusters
plt.figure(figsize=(10, 7))
plt.scatter(X_pca[:, 0], X_pca[:, 1], c=clusters, cmap='viridis', marker='o')
plt.title('Clusters de proveedores - K-means')
plt.xlabel('Componente Principal 1')
plt.ylabel('Componente Principal 2')
plt.grid(True)
plt.show()
Silhouette Score
silhouette_avg = silhouette_score(X_scaled, clusters)
print(f"Silhouette Score: {silhouette_avg}")
Interpretación de resultados K-means

El análisis K-means ha revelado 3 grupos distintos de proveedores. La visualización PCA ayuda a comprender la separación de los clusters en un espacio reducido. El Silhouette Score indica que la estructura de los clusters es moderadamente buena, con un valor positivo que sugiere que los puntos están bien asignados a sus clusters respectivos.
Agrupamiento utilizando Clustering jerárquico

El clustering jerárquico agrupa los datos en una jerarquía o árbol. Este método puede ser aglomerativo (comienza con puntos individuales y los agrupa) o divisivo (comienza con un solo grupo y lo divide).
Definición de parámetros y ejecución del modelo de clustering jerárquico
Clustering jerárquico
linked = linkage(X_scaled, method='ward')
Dendrograma
plt.figure(figsize=(15, 10))
dendrogram(linked, orientation='top', distance_sort='descending', show_leaf_counts=True)
plt.title('Dendrograma - Clustering jerárquico')
plt.show()
Asignación de clusters
hc = AgglomerativeClustering(n_clusters=3, affinity='euclidean', linkage='ward')
hc_clusters = hc.fit_predict(X_scaled)
Agregar resultados al dataframe
df_final['HC_Cluster'] = hc_clusters
Visualización de clusters
plt.figure(figsize=(10, 7))
plt.scatter(X_pca[:, 0], X_pca[:, 1], c=hc_clusters, cmap='rainbow', marker='o')
plt.title('Clusters de proveedores - Clustering jerárquico')
plt.xlabel('Componente Principal 1')
plt.ylabel('Componente Principal 2')
plt.grid(True)
plt.show()
Silhouette Score
silhouette_avg_hc = silhouette_score(X_scaled, hc_clusters)
print(f"Silhouette Score: {silhouette_avg_hc}")
Interpretación de resultados Clustering jerárquico

El clustering jerárquico también revela 3 grupos de proveedores, con una estructura que se puede visualizar a través del dendrograma. El Silhouette Score es ligeramente inferior al de K-means, sugiriendo que la definición de los clusters es algo menos clara en este caso.
Agrupamiento utilizando DBSCAN

DBSCAN es un método basado en densidad que identifica clusters de alta densidad en el espacio de datos. Este método es útil para descubrir clusters de forma arbitraria y es robusto a outliers.
Definición de parámetros y ejecución del modelo DBSCAN
DBSCAN
dbscan = DBSCAN(eps=0.5, min_samples=5)
dbscan_clusters = dbscan.fit_predict(X_scaled)
Agregar resultados al dataframe
df_final['DBSCAN_Cluster'] = dbscan_clusters
Visualización de clusters
plt.figure(figsize=(10, 7))
plt.scatter(X_pca[:, 0], X_pca[:, 1], c=dbscan_clusters, cmap='plasma', marker='o')
plt.title('Clusters de proveedores - DBSCAN')
plt.xlabel('Componente Principal 1')
plt.ylabel('Componente Principal 2')
plt.grid(True)
plt.show()
Silhouette Score
silhouette_avg_dbscan = silhouette_score(X_scaled, dbscan_clusters)
print(f"Silhouette Score: {silhouette_avg_dbscan}")
Interpretación de resultados DBSCAN

DBSCAN identifica un número variable de clusters basados en la densidad de los puntos en el espacio de datos. El Silhouette Score puede ser menor debido a la naturaleza de este método, que no necesariamente busca la estructura global del clustering pero es útil para identificar outliers y clusters densos específicos.

Conclusión y recomendaciones

A través de la implementación de diferentes métodos de clustering, se pueden observar distintas agrupaciones de proveedores, cada uno con sus ventajas y desventajas:

K-means: Identifica claramente 3 clusters con un Silhouette Score moderadamente bueno.
Clustering jerárquico: Proporciona una estructura jerárquica que puede ser útil para entender las relaciones entre los proveedores.
DBSCAN: Es útil para identificar clusters de alta densidad y outliers, aunque puede resultar en un menor Silhouette Score.

Se recomienda realizar un análisis más profundo de las características específicas de cada cluster para definir estrategias de negocio. Además, con la integración de estos resultados en un sistema de BI, Frubana puede tomar decisiones más informadas respecto a la selección y gestión de sus proveedores.



