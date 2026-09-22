# Optimización de la Red Troncal de Transporte Eléctrico de Alta Tensión en España

## Contexto del Proyecto: Escenario Académico de Planificación (REE)

### Distinción Fundamental: Red Real en Malla vs. Estructura en Árbol de la Práctica

Es fundamental que distingas entre la realidad física de la red y el modelo matemático **muy simplificado** que resolveremos:

* **La Red Real de Transporte (Topología en Malla):** En la vida real, las redes de alta tensión (400 kV / 220 kV) son **estructuras fuertemente malladas** (llenas de bucles y caminos alternativos). Esto es obligatorio por ley para cumplir criterios estrictos de seguridad ($N-1$, $N-2$), permitir que las ondas electromagnéticas y los flujos de potencia se distribuyan simultáneamente por múltiples líneas paralelas según las leyes de Kirchhoff, y asegurar que la desconexión de una línea nunca aísle a ninguna provincia ni central de generación.
* **El Modelo de la Práctica (Estructura en Árbol / Árbol de Recubrimiento Mínimo):** En esta práctica académica nos situamos en una fase preliminar de planificación desde cero (*greenfield*), donde buscamos una **estructura lineal/acíclica (un árbol)**: la red estrictamente mínima donde entre cada par de subestaciones existe **un único camino posible** y no hay ningún bucle redundante. 

> **Propósito docente:** Aunque una red eléctrica real jamás opera en árbol por motivos obvios de vulnerabilidad, el Árbol de Recubrimiento Mínimo (MST) proporciona a los ingenieros la **cota inferior económica y el "esqueleto base" de coste mínimo** a partir del cual se proyectan posteriormente las líneas de refuerzo y mallado. Además, permite estudiar la aplicación práctica de algoritmos voraces y estructuras de datos eficientes sobre problemas de conectividad territorial.

---

### El Desafío de la Red Troncal

En este escenario de planificación, se proyecta la interconexión de las 47 subestaciones de cabecera provincial peninsulares mediante líneas aéreas de 400 kV bajo dos premisas técnicas:

1. **Restricción técnica de longitud de vano (AC):** En líneas de corriente alterna a 400 kV, los enlaces directos de gran longitud presentan caídas de tensión inasumibles y sobretensiones en vacío por efecto capacitivo (*efecto Ferranti*), requiriendo complejas subestaciones intermedias de compensación de reactiva. Por ello, solo se consideran técnicamente viables aquellos enlaces directos entre subestaciones con una distancia $d \le 200\text{ km}$.
2. **Minimización del kilometraje de tendido:** Se busca determinar el árbol de coste mínimo que garantice la conectividad eléctrica de todas las provincias con el menor número de kilómetros de conductor posible (Árbol de Recubrimiento Mínimo).

---

## Datos de Partida

El diseño parte del fichero `espana_coordenadas.csv`, que recoge la posición de las 47 subestaciones eléctricas provinciales:

* **Sistema de referencia:** Coordenadas proyectadas en el sistema oficial del IGN: **ETRS89 / UTM Huso 30N (EPSG:25830)** en metros (`Coord_X` y `Coord_Y`).
* Las líneas aéreas de alta tensión discurren en línea recta campo a través entre subestaciones; por tanto, las distancias de los vanos se aproximan mediante la distancia euclidiana en el plano proyectado, expresando los resultados en **kilómetros enteros** (redondeados).

---

## Requisitos de la Solución

El software debe estructurarse en dos fases diferenciadas, demostrando un uso riguroso y eficiente de las colecciones estándar de Python (**diccionarios y conjuntos**):

### Fase 1: Modelado de Líneas de Transporte Técnicamente Viables
* **Indexación espacial (Diccionarios):** Procesar `espana_coordenadas.csv` y almacenar la información en un diccionario que permita la consulta directa de las coordenadas de cada subestación a partir del nombre de la provincia.
* **Filtrado por viabilidad técnica:** Evaluar las distancias entre todos los pares posibles de subestaciones y descartar aquellos enlaces que superen el límite técnico admisible de 200 km.
* **Eliminación de redundancias (Conjuntos):** Una línea eléctrica es físicamente bidireccional (el enlace `Valladolid – Palencia` es idéntico a `Palencia – Valladolid`). Es imperativo utilizar **conjuntos (`set`)** para almacenar los enlaces únicos y evitar líneas duplicadas en sentido inverso.
* **Persistencia intermedia:** Exportar el catálogo de líneas viables al archivo `espana_distancias.csv` con un formato adecuado.

### Fase 2: Optimización de la Red Troncal (Kruskal) y Cartografía
* **Optimización con Kruskal:** A partir de `espana_distancias.csv`, implementar el **algoritmo de Kruskal** para seleccionar el subconjunto óptimo de líneas que garantice la interconexión completa de la red nacional.
* **Métricas de la red:** Mostrar por consola la relación de líneas de alta tensión seleccionadas y el **kilometraje total mínimo de tendido eléctrico**.
* **Visualización cartográfica:** Empleando `matplotlib`, generar un mapa de la península que muestre la ubicación de las 47 subestaciones provinciales y trace las líneas eléctricas que conforman la red troncal óptima seleccionada.

