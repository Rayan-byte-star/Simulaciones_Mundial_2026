# Metodología del Modelo Predictivo

## 1. Introducción
El presente proyecto emplea un enfoque de aprendizaje supervisado y simulación estocástica para modelar los resultados de la Copa Mundial de la FIFA 2026. El objetivo es determinar la probabilidad de éxito de las selecciones participantes mediante un motor de inferencia multivariante.

## 2. Pipeline de Procesamiento de Datos
El flujo de trabajo se divide en cuatro etapas críticas:

1.  **Ingesta y Normalización:** Se consolidan diversas fuentes de datos (históricos de partidos, estadísticas de rendimiento y valor de mercado). Se aplica un proceso de normalización de cadenas para armonizar los nombres de las selecciones bajo un estándar único.
2.  **Ingeniería de Características:** Se construyen vectores de entrada basados en métricas acumuladas (xG, posesión, eficacia en remates). Estas variables capturan el estado de forma actual de cada equipo tanto en su faceta local como visitante.
3.  **Modelado Predictivo:**
    * **Algoritmo:** Se implementó un `RandomForestRegressor` para la estimación de goles. 
    * **Configuración:** Se utilizaron dos estimadores independientes para cada equipo en un enfrentamiento (Local vs. Visitante), configurados con 200 árboles de decisión y una profundidad máxima de 10 para garantizar la generalización del modelo y evitar el sobreajuste (*overfitting*).
4.  **Capa Estocástica y Simulación:**
    * Las salidas del modelo representan la esperanza matemática ($\lambda$) de goles.
    * Para incorporar la incertidumbre propia de la disciplina deportiva, se utiliza una **Distribución de Poisson**, generando resultados discretos que permiten resolver empates mediante reglas de avance en fases eliminatorias.

## 3. Simulación de Monte Carlo
Para obtener una métrica probabilística robusta, el motor de torneo se ejecuta en 1,000 iteraciones independientes. Este enfoque permite observar la distribución de frecuencias de los campeones, proporcionando una estimación estadística de la probabilidad de victoria para cada equipo, mitigando el sesgo de una única predicción determinista.

## 4. Validación del Modelo
El rendimiento del modelo fue evaluado mediante métricas de error estándar:
* **MAE (Mean Absolute Error):** Cuantifica la magnitud promedio de los errores en las predicciones.
* **RMSE (Root Mean Squared Error):** Penaliza los errores de mayor magnitud.
* **R² (Coeficiente de determinación):** Evalúa la capacidad del modelo para explicar la varianza en los goles observados.
