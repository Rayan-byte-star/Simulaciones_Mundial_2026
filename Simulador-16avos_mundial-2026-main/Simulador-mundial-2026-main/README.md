# 🏆 Simulador Predictivo del Mundial 2026

Este proyecto es una simulación probabilística de la Copa Mundial de la FIFA 2026. Utiliza un modelo de Machine Learning entrenado con datos históricos para estimar el rendimiento de las selecciones, y simulaciones estocásticas para predecir al campeón del torneo.

## 🧠 Arquitectura del Modelo

El sistema predictivo consta de tres etapas principales:
1. **Machine Learning (Random Forest Regressor):** Dos modelos independientes entrenados para predecir los Goles Esperados (xG o Esperanza Matemática) del equipo local y visitante, basados en métricas históricas de rendimiento y posesión.
2. **Capa Estocástica (Regresión de Poisson):** Las predicciones fraccionales del modelo se utilizan como el parámetro $\lambda$ en una distribución de Poisson, permitiendo simular resultados discretos de goles y resolver empates en fases eliminatorias de manera realista.
3. **Simulación de Monte Carlo:** El motor de torneo simula el cuadro final (desde Dieciseisavos hasta la Gran Final) 1,000 veces para calcular la probabilidad porcentual que tiene cada selección de ganar la copa.

## 📂 Requisitos de Datos

Para que el modelo funcione, requiere los siguientes archivos `.csv`. Todos deben estar ubicados dentro de una carpeta llamada `data/`:
* `datos_mundial.csv`
* `datos_historicos.csv`
* `partidos.csv`
* `Grupos_Mundial.csv`
* `ranking_fifa.csv`
* `transfermarkt.csv`

---

## 🚀 Instrucciones de Ejecución para el Evaluador

El código fuente se encuentra en el archivo `RandomForest.ipynb`. Puedes evaluar este proyecto de dos maneras: a través de Google Colab (Recomendado) o en un entorno local.

### Opción A: Ejecución Rápida en Google Colab (Recomendado)
Esta es la forma más directa de probar el código, ya que el *notebook* está configurado para montar Google Drive.

1. Descarga el archivo `RandomForest.ipynb` y la carpeta `data/` de este repositorio.
2. Sube la carpeta `data/` a tu propio Google Drive. Asegúrate de colocarla en una ruta fácil de encontrar, por ejemplo: `Mi unidad/Simulaciones_Mundial/Data/`.
3. Abre Google Colab (colab.research.google.com) y sube el archivo `RandomForest.ipynb`.
4. En el **Bloque 2** del *notebook*, modifica la variable `base_path` para que coincida con la ruta donde guardaste la carpeta `data` en tu Drive.
   ```python
   # Ejemplo:
   base_path = "/content/drive/MyDrive/TU_RUTA_AQUI/data"
