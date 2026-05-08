<h1 align="center">📊 Segmentación Inteligente de Clientes con Machine Learning</h1>

<p align="center">
Proyecto de Aprendizaje No Supervisado enfocado en segmentación de clientes utilizando técnicas de clustering.
</p>

<hr>

<h2>🎯 Objetivo del Proyecto</h2>

<p>
Identificar grupos de clientes con comportamientos similares mediante técnicas de aprendizaje no supervisado,
permitiendo diseñar estrategias de marketing diferenciadas basadas en datos.
</p>

<hr>

<h2>📂 Dataset</h2>

<ul>
  <li>Más de 1000 registros de clientes</li>
  <li>Variables de ingreso, gasto, frecuencia y canales de compra</li>
  <li>Datos orientados al comportamiento de consumo</li>
</ul>

<hr>

<h2>🧹 Limpieza y Preprocesamiento</h2>

<ul>
  <li>Eliminación de columnas irrelevantes</li>
  <li>Imputación de valores nulos en <b>Income</b> usando mediana</li>
  <li>Detección y eliminación de outliers mediante Z-Score</li>
  <li>Escalado utilizando <b>StandardScaler</b></li>
</ul>

<hr>

<h2>📈 Análisis Exploratorio de Datos (EDA)</h2>

<p>
Durante el análisis exploratorio se identificó alta dispersión en variables de gasto,
además de correlaciones importantes entre variables de consumo.
</p>

<h3>Distribución de Ingresos</h3>
<img src="images/hist_income.png" width="700">

<h3>Matriz de Correlación</h3>
<img src="images/heatmap_corr.png" width="700">

<hr>

<h2>⚙️ Feature Engineering</h2>

<p>
Se crearon nuevas variables para representar mejor el comportamiento del cliente:
</p>

<ul>
  <li><b>Total_Spend:</b> gasto total acumulado</li>
  <li><b>Avg_Ticket:</b> gasto promedio por compra</li>
</ul>

<img src="images/total_spend.png" width="700">

<hr>

<h2>🚨 Tratamiento de Outliers</h2>

<p>
Los outliers fueron identificados utilizando Z-Score y eliminados cuando |Z| &gt; 3.
Esto permitió mejorar la estabilidad del modelo y evitar distorsiones en la distancia euclidiana.
</p>

<h3>Antes del Tratamiento</h3>
<img src="images/boxplot_before.png" width="700">

<h3>Después del Tratamiento</h3>
<img src="images/boxplot_after.png" width="700">

<hr>

<h2>⚖️ Comparación de Escalamiento</h2>

<p>
Se compararon tres escenarios:
</p>

<ul>
  <li>Datos sin escalar</li>
  <li>MinMaxScaler</li>
  <li>StandardScaler</li>
</ul>

<p>
StandardScaler presentó el mejor desempeño según el silhouette score.
</p>

<img src="images/scaling_comparison.png" width="700">

<hr>

<h2>📉 Selección del Número de Clusters</h2>

<p>
El número óptimo de clusters se seleccionó utilizando el método del codo,
identificando un punto de inflexión en K = 3.
</p>

<img src="images/elbow_method.png" width="700">

<hr>

<h2>🌐 DBSCAN</h2>

<p>
Se utilizó DBSCAN para detectar estructuras basadas en densidad y observaciones atípicas.
</p>

<img src="images/kdistance_plot.png" width="700">

<hr>

<h2>🌳 Clustering Jerárquico</h2>

<p>
El dendrograma mostró una estructura consistente con tres grupos principales.
</p>

<img src="images/dendrogram.png" width="700">

<hr>

<h2>📊 Visualización Final de Clusters</h2>

<p>
Se utilizó PCA para reducir dimensionalidad y visualizar los clusters en 2D.
</p>

<img src="images/pca_clusters.png" width="700">

<hr>

<h2>🧠 Segmentación de Clientes</h2>

<table border="1" cellpadding="10">
  <tr>
    <th>Tribu</th>
    <th>Características</th>
    <th>Estrategia</th>
  </tr>
  <tr>
    <td>VIP Omnicanal</td>
    <td>Alto ingreso y alto gasto</td>
    <td>Fidelización premium</td>
  </tr>
  <tr>
    <td>Cazadores de Ofertas Activos</td>
    <td>Alta actividad y respuesta a promociones</td>
    <td>Promociones personalizadas</td>
  </tr>
  <tr>
    <td>Exploradores de Bajo Consumo</td>
    <td>Bajo gasto y alta presencia digital</td>
    <td>Campañas de activación</td>
  </tr>
</table>

<hr>

<h2>💰 Estrategias de Negocio</h2>

<table border="1" cellpadding="10">
  <tr>
    <th>Campaña</th>
    <th>Objetivo</th>
    <th>Acción Recomendada</th>
  </tr>
  <tr>
    <td>Programa VIP Omnicanal</td>
    <td>Fidelización</td>
    <td>Beneficios exclusivos y experiencias premium</td>
  </tr>
  <tr>
    <td>Ofertas Inteligentes</td>
    <td>Incrementar recurrencia</td>
    <td>Cupones y promociones personalizadas</td>
  </tr>
  <tr>
    <td>Conversión Digital</td>
    <td>Aumentar consumo</td>
    <td>Campañas digitales de entrada</td>
  </tr>
</table>

<hr>

<h2>🎯 Conclusiones</h2>

<p>
El proyecto permitió identificar tres tribus de clientes mediante técnicas de clustering.
K-Means con K = 3 fue seleccionado por ofrecer el mejor equilibrio entre desempeño e interpretabilidad.

La segmentación permitió transformar datos en estrategias accionables orientadas a fidelización,
promociones inteligentes y crecimiento del consumo.
</p>

<hr>

<h2>🛠️ Tecnologías Utilizadas</h2>

<ul>
  <li>Python</li>
  <li>Pandas</li>
  <li>Scikit-Learn</li>
  <li>Matplotlib</li>
  <li>Seaborn</li>
  <li>Jupyter Notebook</li>
</ul>
