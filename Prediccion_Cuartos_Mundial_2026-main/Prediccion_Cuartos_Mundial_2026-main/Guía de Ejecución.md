# 🚀 Guía de Ejecución: Simulación Predictiva del Mundial 2026
### Cuartos de Final – Modelo XGBoost + Monte Carlo (1000 simulaciones)

Este documento detalla los pasos necesarios para configurar el entorno, ejecutar correctamente el notebook y obtener las probabilidades de clasificación a semifinales del Mundial 2026.

---

## 📋 Pre-requisitos y Configuración del Entorno

El código está diseñado para ejecutarse en **Google Colab** y utiliza rutas específicas de Google Drive. Antes de ejecutar cualquier celda, realiza los siguientes preparativos:

1. **Estructura de Carpetas en Google Drive:** Crea la siguiente ruta exacta dentro de tu unidad:

   `Mi unidad/Simulaciones_Mundial/Data/`

2. **Archivos Base:** Sube los siguientes archivos CSV originales a la carpeta `Data/`:

   - `datos_historicos.csv` (historial de partidos internacionales)
   - `ranking_fifa.csv` (puntuaciones FIFA históricas)
   - `transfermarkt.csv` (valor de mercado de las selecciones en millones de €)

3. **Montar Drive en Colab:** Al abrir el notebook, ejecuta la celda que contiene:

```python
from google.colab import drive
drive.mount('/content/drive')
```

Sigue las instrucciones para autorizar el acceso a tu Drive.

> **Nota:** Si deseas ejecutar el notebook en un entorno local (fuera de Colab), cambia la variable `base_path` en las celdas correspondientes por `'./Data'` (o la ruta donde tengas los archivos) y comenta las líneas de montaje de Drive.

---

## ⚙️ Pasos para Ejecutar el Notebook

El notebook está organizado en bloques lógicos que deben ejecutarse **en orden secuencial**. No omitas ningún paso, ya que las celdas posteriores dependen de los datos y modelos generados previamente.

### Paso 1: Montaje y Limpieza de Datos

**Celda 1: Montar Google Drive**

Ejecuta la celda de montaje para tener acceso a tus archivos.

**Celda 2: Limpieza de datos históricos**

- Convierte la columna `Fecha` a tipo datetime.
- Normaliza los nombres de los equipos (por ejemplo, "EEUU" → "Estados Unidos").
- Crea la columna `Resultado_Num` a partir de `Resultado_1X2`.
- Elimina filas con valores nulos en columnas clave.
- Guarda el resultado como `datos_historicos_limpio.csv` en la misma carpeta.

**Celda 3: Limpieza del Ranking FIFA**

- Convierte fechas, normaliza nombres y se queda con el ranking más reciente por país.
- Guarda el archivo `ranking_fifa_limpio.csv`.

**Celda 4: Limpieza de Transfermarkt**

- Normaliza nombres y guarda `transfermarkt_limpio.csv`.

> ✅ Al finalizar estas celdas, tendrás tres archivos limpios listos para el pipeline.

---

### Paso 2: Entrenamiento del Modelo XGBoost

**Celda 5: Pipeline completo y entrenamiento**

Esta celda es la más importante y realiza lo siguiente:

- Carga los tres archivos limpios.
- Cruza el ranking FIFA con cada partido (mediante `merge_asof`) para asignar la puntuación más reciente a cada equipo en cada fecha.
- Añade el valor de mercado de cada equipo (rellenando valores faltantes con la mediana).
- Calcula variables dinámicas (features):
  - Promedio de goles marcados y recibidos en los últimos 5 y 10 partidos (forma reciente).
  - Racha de resultados (suma de signos de diferencia de goles) en los últimos 5 y 10 partidos.
  - Diferencias entre local y visitante para todas estas variables.
- Divide los datos en 85% entrenamiento y 15% prueba.
- Escala las features con `StandardScaler`.
- Entrena dos modelos XGBoost (uno para goles locales y otro para goles visitantes) con parámetros optimizados.
- Evalúa el rendimiento mostrando MAE, R² y precisión 1X2.

**Resultado esperado:**

```text
MAE Local:     1.086
MAE Visitante: 0.881
Precisión 1X2: 60.0%
```

---

### Paso 3: Simulación de un Único Escenario de Cuartos

**Celda 6: Simulación única**

- Define los emparejamientos de Cuartos (actualmente: Francia vs Marruecos, Noruega vs Inglaterra, España vs Bélgica, Suiza vs Argentina).
- Para cada partido, utiliza una función `predecir_partido()` que:
  - Estima goles esperados mediante una fórmula basada en ranking y valor de mercado (coeficientes ajustados empíricamente).
  - Simula los goles con distribución de Poisson.
  - Resuelve empates con un modelo de penales (probabilidad en función de ranking y valor).
- Muestra el marcador y el ganador.
- Guarda los resultados en un CSV con timestamp.

**Salida:**

Archivo `Resultados_Cuartos_2026_<timestamp>.csv` en la carpeta `Data/`.

---

### Paso 4: Simulación de Monte Carlo (1000 iteraciones)

**Celda 7: Monte Carlo**

- Repite la simulación de los cuatro partidos **1000 veces**.
- En cada iteración se registra el ganador de cada cruce.
- Se calcula la frecuencia con la que cada equipo avanza a semifinales.
- Se genera un DataFrame con las probabilidades porcentuales y se guarda como `MonteCarlo_Cuartos_1000sim_<timestamp>.csv`.

**Resultado esperado:**

```text
      País  Probabilidad_Pasar_a_Semifinales_%
Inglaterra                                82.8
Francia                                   81.1
España                                    77.7
Argentina                                 74.9
Suiza                                     25.1
Bélgica                                   22.3
Marruecos                                 18.9
Noruega                                   17.2
```

---

## 💾 Guardar los Modelos Entrenados (Opcional)

Si deseas reutilizar los modelos sin tener que reentrenarlos cada vez, puedes añadir el siguiente código al final de la celda de entrenamiento (Paso 2):

```python
import joblib

joblib.dump(reg_local, f'{base_path}/modelo_local.pkl')
joblib.dump(reg_visit, f'{base_path}/modelo_visitante.pkl')
joblib.dump(scaler, f'{base_path}/scaler.pkl')
```

Luego, en ejecuciones futuras, puedes cargarlos con:

```python
reg_local = joblib.load(f'{base_path}/modelo_local.pkl')
reg_visit = joblib.load(f'{base_path}/modelo_visitante.pkl')
scaler = joblib.load(f'{base_path}/scaler.pkl')
```

---

## 🔧 Posibles Errores y Soluciones

| Error | Causa | Solución |
|-------|-------|----------|
| `FileNotFoundError` | Los archivos CSV no están en la ruta esperada. | Verifica que hayas subido los archivos originales y que la carpeta `Data/` esté en `Mi unidad/Simulaciones_Mundial/`. |
| `KeyError` en nombres de columnas | El archivo `datos_historicos.csv` tiene nombres distintos a los esperados. | Revisa las columnas y ajusta los nombres en el código de limpieza si es necesario. |
| `ImportError: No module named 'xgboost'` | XGBoost no está instalado en el entorno. | Ejecuta `!pip install xgboost` en una celda antes del entrenamiento. |
| La simulación tarda mucho | La celda de Monte Carlo puede demorar entre 20 y 40 segundos. | Es normal; el tiempo depende de la capacidad de la máquina. |

---

## 📊 Interpretación de los Resultados

- Los archivos CSV generados contienen todos los datos necesarios para análisis posteriores o visualizaciones.
- La tabla de probabilidades indica, por ejemplo, que Inglaterra tiene un **82.8%** de probabilidades de superar a Noruega en Cuartos, según el modelo.
- Recuerda que estas son **probabilidades**, no certezas. El modelo captura tendencias, pero no puede predecir el azar absoluto del fútbol.

---

## 📝 Personalización de Emparejamientos

Si deseas cambiar los equipos o los cruces, modifica la lista `cuartos_equipos` en las celdas de simulación (Paso 3 y Paso 4). Asegúrate de que los nombres coincidan exactamente con los que aparecen en los archivos limpios (por ejemplo, `"Inglaterra"` o `"España"`).

---

# 👨‍💻 Autor

**Rayan Emilio Loja Alvarado**  
Universidad Nacional Toribio Rodríguez de Mendoza
