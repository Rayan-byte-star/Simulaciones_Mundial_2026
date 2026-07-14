# 🏆 Simulación Predictiva del Mundial 2026

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![XGBoost](https://img.shields.io/badge/XGBoost-1.7%2B-green)
![Machine Learning](https://img.shields.io/badge/Machine%20Learning-Predictive%20Modeling-orange)

Este repositorio contiene una **simulación probabilística completa** enfocada en las fases eliminatorias del Mundial 2026 (Octavos y Cuartos de Final). El proyecto combina el análisis de datos históricos, estadísticas de equipos y técnicas avanzadas de Machine Learning para proyectar los resultados más probables del torneo.

---

## 📌 Descripción del Proyecto

El objetivo principal de este proyecto es predecir de forma probabilística qué selecciones nacionales tienen mayores posibilidades de avanzar en las fases finales del torneo. 

En lugar de predecir un simple "ganador" o "perdedor", el núcleo analítico utiliza un modelo **XGBoost Regressor** configurado con una **distribución de Poisson**. Esta distribución es estadísticamente ideal para modelar eventos de conteo en intervalos de tiempo fijos, como es el caso de la cantidad de goles anotados en un partido de 90 minutos.

A partir de estas predicciones de goles base, el sistema ejecuta una **Simulación de Monte Carlo con 1000 iteraciones** por cada cruce, lo que permite estabilizar la varianza y calcular las probabilidades porcentuales reales de que cada país avance de ronda, contemplando escenarios de empates y resoluciones por penales.

---

## 📊 Arquitectura de Datos

El pipeline se alimenta de múltiples fuentes que son cruzadas y limpiadas para generar un dataset robusto:

1. **Datos Históricos (`datos_historicos.csv`):** Contiene el historial de partidos internacionales. Se han normalizado los nombres de los países (ej. 'EEUU' a 'Estados Unidos') y se han filtrado registros inconsistentes.
2. **Ranking FIFA (`ranking_fifa.csv`):** Proporciona el nivel competitivo histórico y actual de cada selección. El modelo utiliza el ranking más reciente disponible antes de cada encuentro.
3. **Valor de Mercado (`transfermarkt.csv`):** Extraído de Transfermarkt, aporta la cotización en millones de euros de las plantillas, funcionando como un proxy del talento individual disponible en cada equipo.

---

## ⚙️ Feature Engineering (Variables del Modelo)

Para que el modelo XGBoost pueda capturar los matices del fútbol internacional, se derivaron variables dinámicas avanzadas (Features) calculadas en el momento de la ejecución:

* **Diferenciales de Nivel:** Diferencia exacta de puntos en el Ranking FIFA y brecha en el valor de mercado en millones de euros entre el equipo local y el visitante.
* **Forma Reciente (Momentum):** Promedio de goles marcados y recibidos en los últimos 5 y 10 partidos de cada selección.
* **Racha Competitiva:** Acumulación de resultados positivos, negativos o neutros en el corto plazo (últimos 5 encuentros).

---

## 📈 Entrenamiento y Métricas de Rendimiento

El conjunto de datos se dividió en un 85% para entrenamiento y 15% para pruebas. Los hiperparámetros del modelo XGBoost se ajustaron para evitar el sobreajuste (`learning_rate=0.03`, `max_depth=7`, `n_estimators=800`).

Las métricas obtenidas en el conjunto de prueba (Test Split) validan el enfoque del modelo:

* **Precisión Absoluta (Formato 1X2):** `60.0%` de acierto al predecir si el partido termina en victoria local, empate o victoria visitante.
* **Error Absoluto Medio (MAE):** 
  * Goles Equipo Local: `1.086` goles.
  * Goles Equipo Visitante: `0.881` goles.
* **Coeficiente de Determinación (R²):** `0.191` (Local) / `0.122` (Visitante), lo cual es un estándar aceptable dado el alto nivel de aleatoriedad e imprevisibilidad intrínseco en el fútbol.

---

## 🚀 Uso y Salida de Resultados

El flujo final del notebook permite definir los emparejamientos de Octavos de Final y ejecutar la simulación en cadena. 

Al finalizar, el script exporta los resultados automáticamente a archivos CSV dentro del directorio `/Data/` (ej. `Resultados_Mundial_2026_*.csv` y `MonteCarlo_Mundial2026_1000sim_*.csv`). Estos archivos detallan:
* Los goles proyectados por equipo.
* El ganador simulado del cruce.
* La probabilidad porcentual exacta (de 0% a 100%) de alcanzar los Cuartos de Final.

---

## 🛠️ Requisitos del Entorno

Asegúrate de contar con un entorno de Python 3.8 o superior y las siguientes dependencias instaladas:

* `pandas` - Para la manipulación y limpieza de datos.
* `numpy` - Para operaciones numéricas y cálculos de matrices.
* `scikit-learn` - Para el escalado de datos (StandardScaler) y cálculo de métricas.
* `xgboost` - Algoritmo central de predicción (Gradient Boosting).
* `joblib` - Para guardar y cargar los modelos entrenados de forma eficiente.

---

## 👨‍💻 Autor

**Rayan Emilio Loja Alvarado**  
Universidad Nacional Toribio Rodríguez de Mendoza
