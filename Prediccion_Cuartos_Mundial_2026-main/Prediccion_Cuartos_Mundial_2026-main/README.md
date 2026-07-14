# 🏆 Simulación Predictiva del Mundial 2026 – Fase de Cuartos de Final

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![XGBoost](https://img.shields.io/badge/XGBoost-1.7%2B-green)
![Monte Carlo](https://img.shields.io/badge/Monte%20Carlo-1000%20simulaciones-orange)
![Machine Learning](https://img.shields.io/badge/Machine%20Learning-Predictive%20Modeling-brightgreen)

Este repositorio contiene una **simulación probabilística completa** enfocada en los **Cuartos de Final del Mundial 2026**. El proyecto combina datos históricos, estadísticas actuales de selecciones y técnicas avanzadas de Machine Learning para estimar las probabilidades reales de que cada equipo avance a las semifinales.

---

## 📌 Descripción del Proyecto

El objetivo principal es predecir, de forma probabilística, qué selecciones tienen mayores posibilidades de superar los Cuartos de Final del Mundial 2026. Para ello, se emplea un enfoque en dos etapas:

1. **Modelado de goles** con **XGBoost Regressor** configurado con una **distribución de Poisson**, ideal para modelar eventos de conteo como los goles en un partido de fútbol. Las predicciones se basan en variables como el ranking FIFA, el valor de mercado de las plantillas y la forma reciente de cada equipo.

2. **Simulación de Monte Carlo** con **1000 iteraciones** por cada cruce, lo que permite estabilizar los resultados y obtener probabilidades porcentuales robustas de clasificación a semifinales, considerando incluso escenarios de empate y resolución por penales.

---

## 📊 Arquitectura de Datos

El pipeline se nutre de tres fuentes principales, las cuales son limpiadas, normalizadas y cruzadas para generar un dataset de entrenamiento rico en información:

- **Datos Históricos (`datos_historicos.csv`):** Contiene más de 1300 partidos internacionales con variables como goles, posesión, tarjetas, córneres, etc. Los nombres de los equipos se normalizan (ej. "EEUU" → "Estados Unidos") y se crea una columna `Resultado_Num` para facilitar el análisis.

- **Ranking FIFA (`ranking_fifa.csv`):** Proporciona la puntuación histórica y actual de cada selección. Se toma el ranking más reciente disponible antes de cada partido, mediante una unión `merge_asof` por fecha.

- **Valor de Mercado (`transfermarkt.csv`):** Información del valor total de la plantilla en millones de euros, extraída de Transfermarkt. Actúa como un indicador del talento individual disponible.

Todos estos datos se almacenan en versiones limpias dentro del directorio `/Data/`.

---

## ⚙️ Feature Engineering (Variables del Modelo)

Para capturar la dinámica del fútbol moderno, se derivan múltiples características (features) a partir de los datos históricos:

- **Diferenciales de Nivel:**  
  - `Dif_Ranking`: Diferencia de puntos FIFA entre local y visitante.  
  - `Dif_Valor`: Diferencia en valor de mercado (millones €).

- **Forma Reciente (Momentum):**  
  - Promedio de goles marcados y recibidos en los últimos **5 y 10 partidos** para cada equipo.  
  - Variables: `Forma_L_M5`, `Forma_L_E5`, `Forma_V_M5`, `Forma_V_E5`, y sus análogas para 10 partidos.

- **Racha Competitiva:**  
  - Acumulación de resultados (victoria/derrota/empate) en los últimos 5 y 10 encuentros (`Racha_L_5`, `Racha_V_5`, etc.).

- **Diferencias de forma:**  
  - `Dif_Forma_M5`, `Dif_Forma_E5`, etc., que restan los promedios del local y visitante.

Todas las variables son escaladas con `StandardScaler` antes de ser alimentadas al modelo.

---

## 📈 Entrenamiento y Métricas de Rendimiento

El conjunto de datos se divide en **85% entrenamiento** y **15% prueba**. Los hiperparámetros de XGBoost se ajustaron para lograr un equilibrio entre sesgo y varianza:

python
params = {
    'objective': 'count:poisson',
    'n_estimators': 800,
    'max_depth': 7,
    'learning_rate': 0.03,
    'subsample': 0.85,
    'colsample_bytree': 0.8,
    'random_state': 42
}
Se entrenan dos regresores independientes: uno para los goles del equipo local y otro para los del visitante.

**Métricas obtenidas en el conjunto de prueba:**

| Métrica | Local | Visitante |
| :--- | :--- | :--- |
| **MAE** (Error Absoluto Medio) | 1.086 goles | 0.881 goles |
| **R²** (Coeficiente de Determinación) | 0.191 | 0.122 |
| **Precisión 1X2** (victoria local / empate / victoria visitante) | 60.0% | - |

Estos valores son consistentes con la alta variabilidad del fútbol, donde el modelo captura tendencias generales sin sobreajustarse.

## 🚀 Uso y Salida de Resultados

El notebook está diseñado para ejecutarse en **Google Colab** (aunque también puede adaptarse a un entorno local). El flujo principal es el siguiente:

1. Montar Google Drive para acceder a los datos.
2. Ejecutar las celdas de limpieza y entrenamiento.
3. Definir los emparejamientos de Cuartos de Final (actualmente: Francia vs Marruecos, Noruega vs Inglaterra, España vs Bélgica, Suiza vs Argentina).
4. Ejecutar la simulación de Monte Carlo (1000 repeticiones).
5. Obtener las probabilidades de clasificación a semifinales y guardar los resultados en CSV.

**Archivos generados:**

* `Resultados_Cuartos_2026_<timestamp>.csv`: Resultado de una simulación única (marcador y ganador de cada partido).
* `MonteCarlo_Cuartos_1000sim_<timestamp>.csv`: Probabilidades porcentuales de que cada equipo pase a semifinales.

**Ejemplo de salida (Monte Carlo):**

| País | Probabilidad de Pasar a Semifinales (%) |
| :--- | :--- |
| Inglaterra | 82.8 |
| Francia | 81.1 |
| España | 77.7 |
| Argentina | 74.9 |
| Suiza | 25.1 |
| Bélgica | 22.3 |
| Marruecos | 18.9 |
| Noruega | 17.2 |

## 🛠️ Requisitos del Entorno

Asegúrate de tener Python 3.8 o superior y las siguientes librerías instaladas:

```bash
pip install pandas numpy scikit-learn xgboost joblib
```
## 👨‍💻 Autor

**Rayan Emilio Loja Alvarado**  
Universidad Nacional Toribio Rodríguez de Mendoza
