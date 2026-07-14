# 🚀 Guía de Ejecución: Simulación Predictiva del Mundial 2026
### Modelo XGBoost + Simulación de Eliminatorias + Monte Carlo (1000 simulaciones)

Esta guía explica cómo ejecutar correctamente el notebook **"Simulación Predictiva del Mundial 2026"**, el cual entrena un modelo basado en **XGBoost**, simula los partidos de las fases eliminatorias del Mundial 2026 y estima las probabilidades de clasificación mediante **Monte Carlo**.

---

# 📋 Requisitos Previos

El notebook está diseñado para ejecutarse en **Google Colab**, utilizando archivos almacenados en **Google Drive**.

Antes de ejecutar el código, realiza los siguientes pasos.

## 1. Crear la estructura de carpetas

Dentro de Google Drive crea la siguiente carpeta:

```
Mi unidad/
│
└── Simulaciones_Mundial/
      └── Data/
```

---

## 2. Colocar los archivos necesarios

Dentro de la carpeta **Data** deben encontrarse los siguientes archivos originales:

- `datos_historicos.csv`
- `ranking_fifa.csv`
- `transfermarkt.csv`

Estos archivos serán utilizados durante todo el pipeline.

---

## 3. Montar Google Drive

Antes de ejecutar cualquier celda del notebook monta Google Drive:

```python
from google.colab import drive
drive.mount('/content/drive')
```

Una vez autorizado el acceso, el notebook podrá leer y guardar archivos automáticamente.

---

# ⚙️ Orden Correcto de Ejecución

El notebook está dividido en **7 bloques principales**.

Es importante ejecutarlos **en el mismo orden**.

---

# Paso 1. Limpieza de Datos Históricos

**Celda 1**

Esta primera celda realiza el preprocesamiento del archivo:

```
datos_historicos.csv
```

Durante este proceso:

- convierte las fechas al formato datetime;
- normaliza los nombres de los países;
- transforma el resultado del partido en variables numéricas;
- elimina registros con datos faltantes;
- corrige inconsistencias del conjunto histórico.

Finalmente guarda un nuevo archivo limpio.

Resultado esperado:

```
datos_historicos_limpio.csv
```

---

# Paso 2. Limpieza del Ranking FIFA

**Celda 2**

Se procesa el archivo:

```
ranking_fifa.csv
```

Las operaciones realizadas son:

- conversión de fechas;
- normalización de nombres;
- eliminación de registros duplicados;
- conservación únicamente del ranking más reciente de cada selección.

Archivo generado:

```
ranking_fifa_limpio.csv
```

---

# Paso 3. Limpieza de Transfermarkt

**Celda 3**

Se procesa el archivo:

```
transfermarkt.csv
```

El código:

- normaliza los nombres de las selecciones;
- elimina espacios innecesarios;
- corrige diferencias entre nombres utilizados por FIFA y Transfermarkt.

Archivo generado:

```
transfermarkt_limpio.csv
```

---

# Paso 4. Pipeline Completo y Entrenamiento del Modelo

**Celda 4**

Esta es la parte principal del proyecto.

Aquí se construye todo el modelo predictivo.

Las tareas realizadas son:

## Carga de información

Se cargan:

- datos históricos limpios;
- ranking FIFA limpio;
- valores de mercado.

---

## Integración de datos

Se unen las bases utilizando:

- fechas;
- selección local;
- selección visitante.

Para ello se utiliza principalmente:

```
merge_asof()
```

---

## Construcción de Variables Predictoras

El notebook calcula automáticamente numerosas características (features), entre ellas:

- promedio de goles anotados;
- promedio de goles recibidos;
- forma reciente;
- rendimiento de los últimos partidos;
- diferencia de ranking FIFA;
- diferencia del valor de mercado;
- estadísticas recientes de ambos equipos.

Estas variables representan la fortaleza actual de cada selección.

---

## Preparación del modelo

Posteriormente se realiza:

- separación entrenamiento/prueba;
- normalización mediante StandardScaler;
- creación del conjunto de entrenamiento.

---

## Entrenamiento

Se entrenan dos modelos independientes de **XGBoost Regressor**:

- Modelo para goles del equipo local.
- Modelo para goles del equipo visitante.

---

## Evaluación

El notebook muestra indicadores como:

- MAE
- R²
- Precisión del resultado 1X2

Una salida típica es similar a:

```
MAE Local: 1.08

MAE Visitante: 0.88

Precisión 1X2: 60%
```

Al finalizar esta celda ya existe un modelo completamente entrenado.

---

# Paso 5. Simulación de la Fase Eliminatoria

**Celda 5**

Aquí comienza la simulación del Mundial.

El notebook:

- carga los modelos entrenados;
- define los emparejamientos;
- estima los goles esperados;
- genera resultados mediante una distribución de Poisson;
- resuelve empates mediante una simulación de penales.

Para cada partido se obtiene:

- marcador;
- ganador;
- equipo clasificado.

La simulación representa una posible edición completa del Mundial.

---

# Paso 6. Guardado de Resultados

**Celda 6**

Esta celda es independiente.

Su función consiste únicamente en guardar los resultados obtenidos durante la simulación.

Se genera automáticamente un archivo CSV con fecha y hora.

Ejemplo:

```
Resultados_Mundial_2026_20260709_103522.csv
```

Este archivo puede utilizarse posteriormente para análisis estadísticos o visualizaciones.

---

# Paso 7. Simulación Monte Carlo

**Celda 7**

Esta es la última etapa del notebook.

Se ejecutan **1000 simulaciones independientes** del torneo.

En cada iteración:

- se vuelven a simular todos los partidos;
- se registra el ganador de cada encuentro;
- se contabilizan las clasificaciones.

Al finalizar:

- se calcula la frecuencia de clasificación;
- se estiman las probabilidades porcentuales;
- se construye una tabla resumen.

Ejemplo:

| País | Probabilidad (%) |
|------|-----------------:|
| Francia | 82.4 |
| Inglaterra | 80.6 |
| Argentina | 77.9 |
| España | 75.3 |

Finalmente el resultado se guarda automáticamente como:

```
MonteCarlo_1000Simulaciones_<timestamp>.csv
```

---

# 💾 Archivos Generados

Al finalizar correctamente el notebook deberán existir archivos similares a:

```
datos_historicos_limpio.csv

ranking_fifa_limpio.csv

transfermarkt_limpio.csv

Resultados_Mundial_2026_<timestamp>.csv

MonteCarlo_1000Simulaciones_<timestamp>.csv
```

---

# 🔧 Problemas Frecuentes

| Error | Posible causa | Solución |
|--------|---------------|----------|
| FileNotFoundError | Los archivos no están en la carpeta Data | Verificar la ruta en Google Drive |
| KeyError | El nombre de alguna columna cambió | Revisar los encabezados del CSV |
| ImportError de xgboost | La librería no está instalada | Ejecutar `!pip install xgboost` |
| ModuleNotFoundError de joblib | Joblib no está instalado | Ejecutar `!pip install joblib` |
| La simulación tarda varios segundos | Monte Carlo ejecuta 1000 iteraciones | Es completamente normal |

---

# 📊 Interpretación de Resultados

El modelo no intenta adivinar exactamente el resultado de un partido.

Su objetivo es estimar probabilidades.

Por ejemplo:

```
Argentina
Probabilidad de clasificar: 78%
```

significa que, de las **1000 simulaciones realizadas**, Argentina avanzó aproximadamente **780 veces**.

Mientras mayor sea la probabilidad, mayor es la confianza estadística del modelo.

---

# 🔄 Personalización

Puedes modificar fácilmente:

- los cruces;
- los equipos participantes;
- el número de simulaciones;
- los parámetros del modelo XGBoost;
- las variables utilizadas como características (features).

Después de realizar cualquier modificación importante se recomienda volver a entrenar el modelo para mantener la consistencia de las predicciones.

---

# 👨‍💻 Recomendaciones

Para obtener resultados reproducibles se recomienda:

- ejecutar todas las celdas en orden;
- no modificar los archivos limpios manualmente;
- mantener la misma estructura de carpetas;
- guardar los modelos entrenados si se realizarán múltiples simulaciones.

---

# 🏆 Resultado Final

Al finalizar correctamente el notebook habrás obtenido:

- un conjunto de datos completamente limpio;
- un modelo XGBoost entrenado;
- una simulación completa del Mundial 2026;
- los resultados exportados en formato CSV;
- una simulación Monte Carlo con las probabilidades de clasificación de cada selección.
