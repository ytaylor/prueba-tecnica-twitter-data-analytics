# Prueba técnica de empresa de visualización y reporting

## Introducción
Esta prueba técnica está diseñada para avalar tus habilidades como Analista de datos, poniendo énfasis en la calidad del código, el nivel de experiencia usando las tecnologías requeridas:


## Tecnologías requeridas: 

Visual Studio Code, Python, Power BI/ Tableau y cualquier otra similar.

## Prueba técnica

Presenta información sobre un feed de Twitter sobre la COVID-19. Descarga el conjunto de datos sin procesar desde este enlace. El conjunto de datos de Twitter se recopiló a través del flujo de datos e incluye metadatos de tweets de varios idiomas. Puedes realizar cualquier tipo de análisis de este conjunto de datos. El objetivo es generar información útil sobre este tema. Prepara una presentación breve con los resultados de tu análisis. La presentación debe presentar algunas ideas, dirigidas a una audiencia no técnica.


Analiza también algunos detalles técnicos; por ejemplo, cómo preparaste los datos e implementaste el análisis, describe los modelos que utilizaste o tus opciones de configuración.



## Fases seguidas 

### Fase  Extracción, limpieza y transformación de datos. 

#### Exploracion

Esta es la fase de "hacerse amigas de los datos". El objetivo aquí no es hacer gráficos definitivos, sino entender qué información tenemos, si está sucia, qué tipos de datos hay y qué historias potenciales se esconden en el archivo COVID.csv.

- El dataset tiene 60,160 tweets (filas) y 22 columnas. Es una muestra robusta, idónea para sacar patrones.
- Las columnas de coordenadas (Lat y Long) están vacías casi al 100% (solo hay 19 tweets con coordenadas de 60,000). Conclusión de negocio: Queda totalmente descartado hacer mapas de calor precisos por coordenadas en Power BI; nos enfocaremos en la columna de texto libre
- El 50% de los tweets del dataset tienen 0 retweets y 0 likes, pero el máximo de retweets es de 23,832. Conclusión: Hay unos pocos usuarios con un impacto masivo (Outliers/Influencers). Debemos tratarlos por separado en el reporte.
- La conversación se divide principalmente entre Inglés (33,174 tweets) y Español (15,814 tweets). Los demás idiomas (italiano, francés...) se quedan muy por detrás. Conclusión: Podríamos segmentar el dashboard final en estos dos grandes idiomas para que las métricas no se diluyan.
- El 90.1% de las cuentas no están verificadas. La conversación sobre el COVID-19 en este set de datos está empujada por la ciudadanía general, no por grandes corporaciones o instituciones verificadas.

### Limpieza
Una vez que conocemos los datos y sus defectos, es hora de usar Python en VS Code para limpiarlos y dejarlos listos para Power BI o Tableau".
-  Las columnas de coordenadas geográficas (`Lat` y `Long`) contaban con un 99.9% de valores nulos (solo 19 registros válidos de 60,160). Fueron eliminadas para aligerar la carga del modelo en la herramienta de reporting y evitar falsas expectativas visuales.
- Los valores faltantes en variables categóricas como `Tweet Location` e `Tweet Language` fueron imputados con la etiqueta `"Desconocido"` para mantener la integridad de los registros sin alterar las métricas globales.
-  La columna `Tweet Posted Time (UTC)` se transformó a formato *datetime*. A partir de ella, se crearon dos variables derivadas de alto valor analítico: `Año-Mes` (para análisis de tendencias) y `Hora del Día` (para identificar picos de interacción).
- La columna original `Tweet Location` presentaba un formato de texto libre altamente caótico (mezclando ciudades, modismos, faltas de ortografía e idiomas). Se implementó un algoritmo de mapeo por palabras clave para agrupar y estandarizar las ubicaciones en variables geográficas limpias reconocibles por herramientas de Business Intelligence.
- Para profundizar en el análisis de comportamiento y perfil de usuario, se generaron tres nuevas dimensiones a partir de los datos crudos:
    - **`Categoria_Influencia`:** Segmentación basada en el volumen de seguidores para aislar el impacto de cuentas institucionales/celebridades del ciudadano común.
    - **`Tiene_Mencion` y `Tiene_Enlace`:** Indicadores booleanos extraídos mediante análisis de texto en `Tweet Content` para medir el nivel de interacción y el uso de fuentes externas.
    - **`Dispositivo_Limpio`:** Estandarización de las cientos de fuentes de la columna `Client` en 5 categorías tecnológicas clave para analizar el perfil de consumo técnico de la audiencia.


 ## 🎨 Fase Prototipado y Visualización (Seaborn & Tableau)

El proceso de diseño visual se dividió en dos etapas: validación técnica mediante código y despliegue interactivo para la audiencia final.

### 1. Prototipado Técnico (Python)
Utilizando las librerías `matplotlib` y `seaborn`, se generaron gráficos analíticos preliminares para verificar la consistencia de las variables calculadas:
* Un análisis de frecuencias demostró que los mercados de **Estados Unidos (4,058 tuits)** y **España (2,528 tuits)** lideran el volumen dentro del segmento identificado.
* Un gráfico de barras de interacciones confirmó la hipótesis de concentración: las cuentas categorizadas como `"Celebridad (>50k)"` absorben un promedio cercano a 30 retweets por publicación, validando el aislamiento de estos usuarios en el reporte final.

### 2. Implementación de Negocio (Tableau)
El entregable final se estructuró en Tableau aprovechando las capacidades nativas de análisis espacial e interactividad:
* **Asignación Geográfica:** La variable `Pais_Estandarizado` se configuró con roles geoespaciales para renderizar un mapa coroplético dinámico.
* **Interactividad de Filtros:** Se programaron acciones de filtrado cruzado; seleccionar cualquier región del mapa recalculado redefine las métricas de interacción y tipos de dispositivos en tiempo real.
* **Diseño Ejecutivo:** Se priorizó el uso de tipografías limpias y paletas de colores corporativas coherentes (evitando saturación visual), facilitando la lectura a tomadores de decisiones sin perfil técnico.   


## ## 🎯 Conclusiones Generales del Proyecto y Valor de Negocio

El análisis avanzado del feed de Twitter sobre la COVID-19 permitió transformar más de 60,000 registros de datos crudos en tres pilares de conocimiento estratégico para tomadores de decisiones:

1. **Geografía de la Conversación (Dónde):** Tras un exhaustivo proceso de ingeniería de datos para mitigar el caos de texto libre en la ubicación, se identificó un claro duopolio en la conversación digital, liderado por **Estados Unidos** y **España**. Esto permite concentrar los esfuerzos de análisis de sentimiento y campañas mediáticas en zonas geográficas de alto impacto bien delimitadas.
2. **Patrón Temporal de Consumo (Cuándo):** La actividad en la red demostró un comportamiento cíclico predecible, situando el punto de máxima audiencia a las **18:00 UTC**. Este hallazgo proporciona una métrica accionable para optimizar los tiempos de publicación de información sensible o campañas institucionales.
3. **Segmentación Tecnológica (Quién):** El análisis cruzado reveló una marcada divergencia cultural en el acceso a la información; mientras el usuario de habla inglesa interactúa predominantemente desde plataformas **Apple iOS**, el usuario de habla hispana lo hace desde **Android**. Esta conclusión obliga a que cualquier estrategia de visualización o despliegue técnico posterior priorice un enfoque responsive enfocado en la experiencia mobile-first.