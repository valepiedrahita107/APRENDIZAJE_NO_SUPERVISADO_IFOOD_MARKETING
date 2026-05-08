# Proyecto 2: Aprendizaje No Supervisado

## Segmentación de clientes con iFood Marketing Data
### Juan Pablo Clavijo y Valeria Piedrahita.

Este proyecto desarrolla un flujo completo de aprendizaje no supervisado para identificar tribus de clientes y traducir los resultados técnicos en acciones de marketing. El análisis parte de la limpieza del dataset, continúa con el laboratorio de escalamiento y la comparación de algoritmos, y finaliza con el perfilamiento comercial de los clusters.

**Dataset:** iFood Marketing Data  
**Fuente:** Kaggle  
**Técnicas utilizadas:** EDA, escalamiento, K-Means, clustering jerárquico, DBSCAN, PCA y perfilamiento de clusters.  
**Modelo final seleccionado:** K-Means con K = 3.

---



## Fase inicial: Selección del dataset

Se seleccionó el dataset **iFood Marketing Data** porque cumple con los requisitos del proyecto: contiene más de 1,000 registros, incluye múltiples variables numéricas continuas y está orientado a comportamiento de clientes, compras, campañas y canales de venta.

Este dataset es especialmente adecuado para clustering, ya que permite segmentar clientes según ingreso, recencia, gasto por categoría, frecuencia de compra, uso de canales y respuesta a campañas. Estas variables facilitan la construcción de tribus comerciales accionables.

---



# FASE 1 - PASO 1: EDA Y LIMPIEZA

# Objetivo: preparar la base para clustering de clientes


---

# Markdown Cell 4

En la etapa de exploración y limpieza se revisó la estructura general del dataset iFood Marketing Data, compuesto por clientes y variables asociadas a ingresos, recencia, gasto por categoría de producto, frecuencia de compra y canales de compra. Se identificaron valores nulos y se trataron mediante imputación con la mediana en variables numéricas, evitando perder registros de clientes. También se eliminaron columnas sin valor analítico, como variables constantes o posibles identificadores, ya que no contribuyen a diferenciar patrones de comportamiento.

El análisis de distribuciones mostró que varias variables de gasto presentan sesgo a la derecha, lo cual es esperable en datos comerciales: muchos clientes compran poco y pocos clientes concentran altos niveles de consumo. Estos valores extremos no se eliminaron automáticamente porque pueden representar clientes de alto valor para el negocio. Esta base limpia queda preparada para el laboratorio de escalamiento, donde se evaluará cómo la escala de las variables afecta los resultados de clustering.

---



## Identificación de valores nulos

---



Esto quiere decir que la base de datos no cuenta con valores nulos, pero de igual manera haremos un tratamiento extra de los valores nulos, dado que aunque los datos de entrenamiento estén perfectos hoy, los datos que lleguen mañana desde una API o un formulario de usuario podrían traer huecos. Si el modelo no sabe qué hacer con un nulo, el sistema arrojará un error y dejará de funcionar.

---



### Interpretación de la imputación de valores nulos

Después de imputar los valores faltantes, la base queda lista para continuar el análisis sin perder clientes por registros incompletos. Desde una perspectiva de negocio, esta decisión conserva información valiosa y evita que el modelo de clustering se entrene con datos incompletos.

---

# Markdown Cell 8

### Detectar columnas constantes o identificadores sin valor analítico.

---

# Markdown Cell 9

El análisis de valores faltantes evidencia que la única variable con datos ausentes es Income, lo cual se debe probablemente a la no respuesta de algunos individuos, clasificándose como un caso de datos faltantes no completamente aleatorios (MNAR). Dado que esta variable es numérica y relevante para el análisis, se optó por imputar sus valores utilizando la mediana, con el fin de preservar la distribución original y reducir el impacto de posibles valores atípicos. Esta estrategia permite mantener la integridad del conjunto de datos y garantizar la correcta aplicación de técnicas de modelado que requieren datos completos

---

# Markdown Cell 10

### Conclusión de limpieza

La limpieza inicial permitió preparar el dataset para el análisis no supervisado. Los valores faltantes fueron tratados mediante imputación con la mediana, una medida robusta frente a valores extremos. También se eliminaron columnas sin valor analítico, como identificadores o variables constantes, porque no describen comportamiento real del cliente y podrían distorsionar las distancias usadas por los algoritmos de clustering.

---

# Markdown Cell 11

### Análisis de distribuciones de variables numéricas

Creamos histogramas para observar la forma de cada distribución.Los histogramas nos ayudan a detectar sesgos, concentración y valores extremos.

---

# Markdown Cell 12

El análisis exploratorio de las variables numéricas evidencia distintos comportamientos distribucionales dentro del conjunto de datos. La variable *Income* presenta una distribución aproximadamente normal, lo que indica una concentración equilibrada de los ingresos sin presencia significativa de valores atípicos, haciéndola adecuada para métodos estadísticos paramétricos. Por su parte, *Recency* muestra una distribución cercana a uniforme, sugiriendo una dispersión homogénea en el tiempo desde la última compra y una limitada capacidad discriminativa de manera individual. En contraste, las variables de gasto (*MntWines*, *MntFruits*, *MntMeatProducts*, entre otras) exhiben una marcada asimetría positiva, con alta concentración de valores bajos y una cola larga de valores elevados, lo que evidencia la presencia de outliers y una alta variabilidad en el comportamiento de consumo. De forma similar, las variables de frecuencia de compra (*NumWebPurchases*, *NumCatalogPurchases*) corresponden a datos discretos con sesgo a la derecha, mientras que *NumStorePurchases* presenta una distribución más centrada y estable. Finalmente, *NumWebVisitsMonth* muestra una distribución unimodal con ligera asimetría, lo que sugiere un comportamiento moderadamente concentrado en valores medios. En conjunto, estos patrones indican la necesidad de aplicar transformaciones en variables sesgadas, así como técnicas robustas de análisis, debido a la heterogeneidad y no normalidad predominante en el dataset.


---

# Markdown Cell 13

### Detección visual de outliers con boxplots
 Los boxplots permiten ver valores extremos de forma rápida. Un outlier no siempre es un error: puede ser un cliente VIP o un caso raro.

---

# Markdown Cell 14

### INTERPRETACIÓN:
El análisis de los diagramas de caja evidencia diferencias importantes en la dispersión, simetría y presencia de valores atípicos entre las variables. *Income* presenta una distribución relativamente simétrica, con una mediana centrada y sin una cantidad significativa de outliers, lo que confirma su estabilidad y coherencia con una distribución aproximadamente normal. De manera similar, *Recency* muestra una dispersión amplia pero equilibrada, sin sesgos marcados ni valores extremos relevantes. En contraste, las variables de gasto (*MntWines*, *MntFruits*, *MntMeatProducts*, *MntFishProducts*, *MntSweetProducts* y *MntGoldProds*) exhiben una marcada asimetría positiva, evidenciada por cajas concentradas en valores bajos, bigotes extendidos hacia la derecha y una alta presencia de outliers superiores, lo que indica una fuerte heterogeneidad en el comportamiento de consumo y la existencia de pocos individuos con niveles de gasto significativamente altos. Por su parte, las variables de frecuencia de compra (*NumWebPurchases* y *NumCatalogPurchases*) también presentan sesgo a la derecha y algunos valores atípicos, característicos de datos de conteo con baja frecuencia en la mayoría de observaciones. En contraste, *NumStorePurchases* muestra una distribución más compacta y centrada, con menor variabilidad relativa y ausencia de outliers extremos, lo que sugiere un comportamiento más consistente. Finalmente, *NumWebVisitsMonth* presenta ligera asimetría positiva con algunos valores atípicos altos, indicando variabilidad moderada en la interacción digital. En conjunto, los boxplots confirman la predominancia de distribuciones no normales, la presencia de valores atípicos y la necesidad de aplicar transformaciones o métodos robustos para un análisis estadístico más preciso.


---

# Markdown Cell 15


###  Matriz de correlación


Calculamos la correlación entre variables numéricas. La correlación mide si dos variables se mueven juntas.

---

# Markdown Cell 16

Si MntWines y MntMeatProducts tienen alta correlación, puede existir una tribu de clientes que compran productos premium en varias categorías.

Si NumWebVisitsMonth tiene baja relación con compras, podría haber clientes que navegan mucho pero compran poco. Eso es útil para campañas de conversión.

---

# Markdown Cell 17

En la etapa de exploración y limpieza se revisó la estructura general del dataset iFood Marketing Data, compuesto por clientes y variables asociadas a ingresos, recencia, gasto por categoría de producto, frecuencia de compra y canales de compra. Se identificaron valores nulos y se trataron mediante imputación con la mediana en variables numéricas, evitando perder registros de clientes. También se eliminaron columnas sin valor analítico, como variables constantes o posibles identificadores, ya que no contribuyen a diferenciar patrones de comportamiento.

El análisis de distribuciones mostró que varias variables de gasto presentan sesgo a la derecha, lo cual es esperable en datos comerciales: muchos clientes compran poco y pocos clientes concentran altos niveles de consumo. Estos valores extremos no se eliminaron automáticamente porque pueden representar clientes de alto valor para el negocio. Esta base limpia queda preparada para el laboratorio de escalamiento, donde se evaluará cómo la escala de las variables afecta los resultados de clustering.

---

# Markdown Cell 18

## FASE 2: LABORATORIO DE ESCALAMIENTO.

---

# Markdown Cell 19

 Creamos una lista de variables candidatas. Elegimos variables que representan comportamiento real del cliente:
 ingresos, recencia, gasto por categoría y canales de compra.

---

# Markdown Cell 20

# 5. Función auxiliar para correr K-Means
   Esta función entrena K-Means sobre una versión del dataset y devuelve métricas útiles para comparar escenarios.
    
    Parámetros:
    
    X_data: dataset numérico, escalado o no escalado.
    scenario_name: nombre del escenario para identificar resultados.
    k: número provisional de clusters.

---

# Markdown Cell 21

El Score de Silueta mide si los clientes están bien asignados a su cluster.

Valores aproximados:

-Cerca de 1	Clusters muy bien separados

-Cerca de 0	Clusters se mezclan

-Menor que 0	Posibles asignaciones incorrectas

Se compararon tres escenarios de escalamiento. Los datos crudos obtuvieron el mayor Score de Silueta, con 0.5335; sin embargo, este resultado puede estar dominado por variables de gran escala como ingresos y montos de compra.

Entre los escenarios escalados, StandardScaler obtuvo mejor desempeño que MinMaxScaler, con un Score de Silueta de 0.2071 frente a 0.1523. Por esta razón, se seleccionó StandardScaler para continuar el proyecto, ya que permite comparar variables en distintas unidades y facilita interpretar los centroides como valores por encima o por debajo del promedio.

---

# Markdown Cell 22

### Función para visualizar clusters en 2D usando PCA
Esta función reduce los datos a 2 dimensiones usando PCA y grafica los clusters con colores diferentes.
    
No usamos PCA para entrenar K-Means aquí. Lo usamos solo para visualizar.

---

# Markdown Cell 23

La proyección de los clusters mediante PCA evidencia un impacto significativo del escalado en la estructura y separabilidad de los grupos generados por K-Means. En el caso de los datos crudos, se observa una separación artificial dominada por la magnitud de las variables, donde los clusters se organizan principalmente a lo largo del primer componente principal, indicando que variables con mayor escala (como ingresos o montos de gasto) están sesgando la formación de grupos. Al aplicar *StandardScaler*, la distribución de los datos se centra y estandariza, lo que permite una separación más equilibrada y compacta de los clusters, reduciendo la influencia de las escalas originales y facilitando la identificación de patrones estructurales más consistentes entre observaciones. Por otro lado, con *MinMaxScaler*, aunque los datos se restringen a un rango uniforme, se observa mayor solapamiento entre clusters, lo que sugiere una menor capacidad de discriminación en comparación con la estandarización. En conjunto, estos resultados indican que el uso de escalado, particularmente mediante estandarización, es fundamental para mejorar la calidad de los agrupamientos en K-Means, ya que favorece la formación de clusters más definidos y representativos de la estructura subyacente de los datos.


---

# Markdown Cell 24

## COMPARACIÓN DE CENTROIDES.

---

# Markdown Cell 25

En los datos crudos, los centroides estarán en unidades reales:

Income en dinero.

Recency en días.

MntWines en gasto.

NumWebPurchases en número de compras.

Pero en StandardScaler y MinMaxScaler, los centroides ya están transformados. Sirven para comparar patrones relativos, no valores reales de negocio.

---

# Markdown Cell 26

## Heatmap de centroides para StandardScaler y MinMaxScaler

---

# Markdown Cell 27

## Conclusión del laboratorio de escalamiento

Se comparó el desempeño de K-Means con tres versiones del dataset: datos crudos, datos transformados con StandardScaler y datos transformados con MinMaxScaler. El escenario con datos crudos obtuvo el mayor Score de Silueta, con un valor de 0.5335. Sin embargo, este resultado debe interpretarse con cautela, ya que K-Means utiliza distancias y las variables con mayor escala, como ingresos y montos de compra, pueden dominar el agrupamiento.

Desde una perspectiva de negocio, usar los datos crudos podría llevar a construir tribus basadas principalmente en poder adquisitivo o gasto total, dejando en segundo plano variables importantes como recencia, frecuencia de compra, canales de compra y visitas web. Por esta razón, aunque la silueta de los datos crudos es mayor, no se considera la mejor opción metodológica para continuar el proyecto.

Entre los escenarios escalados, StandardScaler obtuvo un Score de Silueta de 0.2071, superior al resultado de MinMaxScaler, que fue 0.1523. Además, StandardScaler permite interpretar los centroides en términos de desviaciones respecto al promedio, lo cual facilita el perfilamiento posterior de las tribus.

Por lo tanto, se selecciona StandardScaler como método de escalamiento para el resto del proyecto, ya que ofrece un balance adecuado entre rigor técnico, comparabilidad entre variables y utilidad interpretativa para la estrategia de marketing.

---

# Markdown Cell 28

### ESCALADOR ELEGIDO

---

# Markdown Cell 29

### K-MEANS: MÉTODO DEL CODO Y SCORE DE SILUETA

---

# Markdown Cell 30

### CLUSTERING JERÁRQUICO: DENDROGRAMA

---

# Markdown Cell 31

### CLUSTERING JERÁRQUICO CON 3 CLUSTERS

---

# Markdown Cell 32

### DBSCAN: GRÁFICO DE K-DISTANCIAS

---

# Markdown Cell 33

El gráfico de K-distancias presenta la distancia al vecino número 20 para cada observación, ordenada de menor a mayor, y se utiliza como herramienta para determinar el valor adecuado del parámetro *eps* en el algoritmo DBSCAN. En la figura se observa un comportamiento inicialmente suave y creciente, donde la mayoría de los puntos mantienen distancias relativamente bajas y homogéneas, lo que indica regiones densas del conjunto de datos. Sin embargo, hacia el extremo derecho se identifica un incremento abrupto en la pendiente (punto de “codo”), a partir del cual las distancias aumentan significativamente, evidenciando la transición hacia observaciones más aisladas o potencialmente consideradas ruido. Este punto de inflexión sugiere un valor óptimo de *eps*, ya que separa las zonas densas de aquellas con baja densidad. En este caso, el cambio pronunciado parece ubicarse aproximadamente entre valores de distancia cercanos a 4 y 5, lo que indica que un *eps* dentro de este rango permitiría identificar clusters coherentes, minimizando la inclusión de ruido excesivo o la fragmentación de los grupos.


---

# Markdown Cell 34

### DBSCAN: PRUEBA DE VALORES EPS

---

# Markdown Cell 35

# TABLA FINAL

---

# Markdown Cell 36

La presencia de valores faltantes (missing values) en el conjunto de datos puede deberse a múltiples factores, incluyendo errores en la recolección de información, datos no aplicables a ciertos individuos, fallos técnicos o procesos de transformación previos. Estos valores representan una ausencia de información que, dependiendo de su naturaleza (MCAR, MAR o MNAR), puede introducir sesgos o afectar la calidad del análisis. Por esta razón, es fundamental identificar su origen y aplicar estrategias adecuadas de tratamiento, como eliminación o imputación, con el fin de garantizar la validez de los resultados obtenidos.

---

# Markdown Cell 37

# FASE 3: PERFILAMIENTO DE TRIBUS

---

# Markdown Cell 38

## VARIABLES DE NEGOCIO PARA PERFILAMIENTO

---

# Markdown Cell 39

### VARIABLES DE NEGOCIO PARA PERFILAMIENTO

---

# Markdown Cell 40

Se realizó la creación de variables derivadas orientadas a capturar el comportamiento de consumo, frecuencia de compra y respuesta a campañas, optimizando el procesamiento mediante funciones reutilizables que garantizan consistencia, manejo de valores faltantes y prevención de errores computacionales.

---

# Markdown Cell 41

### PERFIL RELATIVO VS PROMEDIO GENERAL

---

# Markdown Cell 42

### HEATMAP DEL PERFIL RELATIVO

---

# Markdown Cell 43

## TABLA RESUMEN DE TRIBUS

---

# Markdown Cell 44

### RANKINGS PARA INTERPRETAR TRIBUS

---

# Markdown Cell 45


### GRÁFICOS DE PERFILAMIENTO DE TRIBUS


---

# Markdown Cell 46


### ASIGNACIÓN DE NOMBRES COMERCIALES


---

# Markdown Cell 47

## Bautizo de tribus

Con base en los promedios reales de cada cluster, se asignaron nombres comerciales que permiten explicar los segmentos en lenguaje de negocio. El objetivo de este paso es transformar etiquetas técnicas como `Cluster 0`, `Cluster 1` y `Cluster 2` en tribus comprensibles y accionables para una estrategia de marketing.

El **Cluster 0** fue denominado **“Los Exploradores de Bajo Consumo”**. Este grupo representa el 46.44% de la base, con 1024 clientes. Presenta el menor ingreso promedio, 34595.08, el menor gasto total promedio, 96.71, y solo 5.88 compras promedio. Sin embargo, registra 6.42 visitas web mensuales, lo que sugiere interacción digital con baja conversión.

El **Cluster 1** fue denominado **“Los VIP Omnicanal”**. Este grupo representa el 25.80% de la base, con 569 clientes. Es la tribu de mayor valor económico, con ingreso promedio de 76078.09, gasto total promedio de 1419.42, ticket promedio de 74.96 y 19.73 compras promedio. Además, combina compras en tienda, catálogo y web.

El **Cluster 2** fue denominado **“Los Cazadores de Ofertas Activos”**. Este grupo representa el 27.76% de la base, con 612 clientes. Tiene ingreso medio-alto, gasto total promedio de 704.83 y 17.11 compras promedio. Destaca por el mayor número de compras con descuento, 3.73, y el mayor promedio de compras web, 6.36.

---

# Markdown Cell 48


### TABLA DE CAMPAÑAS DE MARKETING

---

# Markdown Cell 49

## Estrategia de marketing

Las campañas propuestas se diseñan a partir de las diferencias reales observadas entre las tribus. Cada recomendación conecta un hallazgo numérico con una decisión comercial concreta.

### Campaña 1: Programa VIP Omnicanal

**Tribu objetivo:** Los VIP Omnicanal.

**Hallazgo numérico:** Esta tribu tiene el mayor ingreso promedio, 76078.09, el mayor gasto total promedio, 1419.42, el mayor ticket promedio, 74.96, y la mayor frecuencia de compra, con 19.73 compras promedio.

**Acción recomendada:** Crear un programa premium con beneficios exclusivos, bundles de alto valor, preventas, recompensas por compras multicanal y atención preferencial.

**Justificación de negocio:** Esta tribu ya genera alto valor para la empresa. Por ello, la prioridad no debe ser aplicar descuentos agresivos, sino fortalecer la fidelización, la retención, la recompra y el aumento del ticket promedio.

### Campaña 2: Ofertas Inteligentes para Clientes Activos

**Tribu objetivo:** Los Cazadores de Ofertas Activos.

**Hallazgo numérico:** Esta tribu tiene 17.11 compras promedio, gasto total de 704.83 y el mayor número de compras con descuento, 3.73. También registra el mayor promedio de compras web, con 6.36.

**Acción recomendada:** Diseñar promociones personalizadas, cupones por tiempo limitado, bundles con descuento y campañas digitales segmentadas según categorías de mayor consumo.

**Justificación de negocio:** Estos clientes compran con frecuencia y muestran sensibilidad a promociones. Una estrategia de descuentos controlados puede aumentar frecuencia y gasto sin sacrificar excesivamente el margen.

### Campaña 3: Conversión de Exploradores Digitales

**Tribu objetivo:** Los Exploradores de Bajo Consumo.

**Hallazgo numérico:** Esta tribu representa el 46.44% de la base, pero tiene el menor gasto total promedio, 96.71, y solo 5.88 compras promedio. A pesar de ello, registra 6.42 visitas web mensuales.

**Acción recomendada:** Implementar campañas digitales de conversión con recomendaciones simples, descuentos de primera compra, envío gratis condicionado a compra mínima y recordatorios personalizados.

**Justificación de negocio:** Aunque esta tribu tiene bajo gasto, su tamaño y su actividad web indican una oportunidad de conversión. La empresa puede generar crecimiento si convierte una parte de estos visitantes en compradores recurrentes.

---

# Markdown Cell 50

## Conclusiones finales

El proyecto permitió identificar tres tribus de clientes a partir de variables de ingreso, gasto, recencia, frecuencia y canales de compra. El proceso incluyó limpieza, análisis exploratorio, laboratorio de escalamiento y comparación de algoritmos de clustering.

El modelo seleccionado fue **K-Means con K = 3**, porque ofreció el mejor equilibrio entre desempeño técnico, tamaños de cluster adecuados e interpretabilidad comercial. Aunque K = 2 obtuvo una silueta más alta, se consideró una segmentación demasiado general para diseñar tres campañas diferenciadas.

La segmentación final diferenció tres grupos relevantes: **Los Exploradores de Bajo Consumo**, **Los VIP Omnicanal** y **Los Cazadores de Ofertas Activos**. Cada tribu presenta patrones distintos de ingreso, gasto, frecuencia, canal de compra y respuesta comercial.

Desde el punto de vista de negocio, los resultados permiten pasar de una estrategia homogénea a una estrategia segmentada: fidelizar a los clientes VIP, incentivar con promociones controladas a los cazadores de ofertas y convertir a los exploradores digitales de bajo consumo en compradores más frecuentes.

---
