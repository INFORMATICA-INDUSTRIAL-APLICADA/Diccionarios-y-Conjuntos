### Práctica: Optimización de la Red de Carreteras de España**

#### **Objetivo**

El objetivo de esta práctica es doble. Primero, procesar datos geográficos brutos para construir un modelo de red. Segundo, aplicar el **algoritmo de Kruskal** para encontrar la red de carreteras de coste mínimo que conecte todas las capitales de provincia de la España peninsular (Árbol de Recubrimiento Mínimo - MST).

#### **Parte 1: Preprocesamiento de Datos - De Coordenadas a Distancias**

En esta primera fase, actuarás como un ingeniero de datos. Tu materia prima es un fichero con coordenadas y tu producto final es un dataset limpio y listo para ser analizado.

**Fichero de Entrada:** `espana_coordenadas.csv`

Este fichero contiene las coordenadas de las 47 capitales de provincia peninsulares.

*   **Coordenadas:** Están en **metros**, utilizando el sistema de proyección estándar **ETRS89 / UTM Zone 30N (EPSG:25830)**.
*   **`Coord_X` (Easting):** Metros al este desde el meridiano de referencia.
*   **`Coord_Y` (Northing):** Metros al norte desde el ecuador.

##### **1.1. La Fórmula Matemática**

La distancia entre dos puntos en un plano UTM se puede aproximar con la **distancia euclidiana**. Para dos puntos `P1(x1, y1)` y `P2(x2, y2)`, la fórmula es:

$$
d = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2}
$$

##### **1.2. Tu Tarea de Programación (Guion `preparar_datos.py`)**

Deberás crear un script de Python que realice las siguientes tareas:
1.  Leer el fichero `espana_coordenadas.csv` y guardar los datos en una estructura de datos adecuada. Un **diccionario** donde la clave es el nombre de la capital y el valor es una tupla `(x, y)` es ideal.
2.  Generar todas las **combinaciones únicas** de dos ciudades a partir de la lista de capitales.
3.  Para cada par de ciudades, calcular la distancia euclidea entre sus coordenadas.
4.  Crear una lista de aristas, donde cada arista es una tupla en el formato `(ciudad1, ciudad2, distancia_en_km)`. La distancia debe ser un número entero (redondeado) para simplificar.
5.  **Generar un fichero de salida** llamado `espana_distancias.csv`. Este fichero contendrá todas las aristas calculadas, una por línea, en el formato `Ciudad1,Ciudad2,Distancia`. Este fichero será la entrada para la siguiente parte de la práctica.

---

#### **Parte 2: Aplicación del Algoritmo de Kruskal y Visualización**

Ahora, con los datos ya procesados, te pondrás en el rol de un analista de optimización.

**Fichero de Entrada:** `espana_distancias.csv` (el que generaste en la parte anterior).

##### **2.1. Implementación del Algoritmo de Kruskal (Guion `analizar_red.py`)**

Tu segundo script deberá:
1.  Leer el fichero `espana_distancias.csv` para obtener la lista completa de aristas.
2.  Implementar la lógica del **algoritmo de Kruskal** que hemos visto en clase. Esto incluye:
    *   La estructura de datos **Union-Find** (usando un diccionario `parent`).
    *   Las funciones `find(nodo)` (con la optimización de "Path Compression") y `union(nodo1, nodo2)`.
3.  Aplicar el algoritmo para encontrar el Árbol de Recubrimiento Mínimo.
4.  Imprimir por consola los resultados clave:
    *   El **coste total mínimo** de la red (la suma de las distancias de las aristas seleccionadas).
    *   La lista de las **46 carreteras** que forman la red óptima.

##### **2.2. Visualización del Resultado**

Un resultado numérico está bien, pero una imagen es mucho más potente. El último paso es crear una visualización de la red que has calculado.

1.  Necesitarás las coordenadas originales de las ciudades que leíste en la Parte 1. Puedes volver a leer `espana_coordenadas.csv`.
2.  Usando la librería `matplotlib.pyplot`, crea un gráfico que muestre:
    *   Un **diagrama de dispersión (`scatter plot`)** con un punto por cada capital de provincia.
    *   **Etiquetas de texto** junto a cada punto para identificar las ciudades.
    *   Líneas que conecten únicamente las ciudades que forman parte de la **solución óptima** encontrada por tu algoritmo de Kruskal.

**El resultado final debería ser un mapa de España con la red de carreteras más eficiente posible conectando todas las capitales.**
