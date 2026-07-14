<div align="center">
  <h1>🏆 Simulaciones Mundial 2026</h1>
  <p><i>Análisis predictivo y resultados basados en modelos estadísticos (xG)</i></p>

  ![Formato](https://img.shields.io/badge/Formato-CSV-success?style=flat-square&logo=microsoftexcel)
  ![Data Analysis](https://img.shields.io/badge/Análisis-Estadístico-blue?style=flat-square)
  ![Licencia MIT](https://img.shields.io/badge/Licencia-MIT-orange?style=flat-square)
</div>

---

Este repositorio contiene los resultados de **tres predicciones independientes** del torneo de la Copa Mundial de Fútbol 2026. Cada predicción se genera a partir de modelos estadísticos que estiman el rendimiento de las selecciones basándose en **goles esperados (xG)**, historial de enfrentamientos y factores tácticos. 

Los archivos `.csv` incluyen los marcadores, los equipos que avanzan de ronda y, en simulaciones detalladas, especificaciones sobre el método de clasificación (tiempo regular o tanda de penales).

## 📋 Índice
- [Archivos del Repositorio](#-archivos-del-repositorio)
- [Metodología](#-metodología)
- [Resultados por Modelo](#-resultados-por-modelo)
  - [Predicción de Octavos (2026-07-06)](#1️⃣-predicción-de-octavos-2026-07-06)
  - [Predicción de Cuartos (2026-07-09)](#2️⃣-predicción-de-cuartos-2026-07-09)
  - [Predicción de 16avos (Torneo Completo)](#3️⃣-predicción-de-16avos-torneo-completo)
- [Tablas Resumen (Todos los Partidos)](#-tablas-resumen-todos-los-partidos)
- [Uso de los Datos](#-uso-de-los-datos)
- [Contribuciones](#-contribuciones)

---

## 📁 Archivos del Repositorio

| Archivo | Descripción | Fases Incluidas |
|---------|-------------|-----------------|
| 📄 `Resultados_Cuartos_2026_20260709_1223 (1).csv` | Resultados correspondientes a la predicción del **9 de julio de 2026**. | Cuartos de final |
| 📄 `Resultados_Mundial_2026_20260706_0105.csv` | Resultados correspondientes a la predicción del **6 de julio de 2026**. | Octavos y Cuartos |
| 📄 `Resultados_16avos.csv` | Predicción **completa** con variables avanzadas (xG local y visitante) y detalles de desempate. | Dieciseisavos a Final |

---

## 🧠 Metodología

Para el cálculo de los resultados, los modelos implementan:
- **xG (Goles Esperados):** Estimación de probabilidad de gol por partido, calculada a partir de volumen de disparos, posesión de balón y métricas de eficacia ofensiva/defensiva.
- **Motor de Simulación:** Evolución de las llaves del torneo mediante simulaciones de Monte Carlo y regresión logística.
- **Criterios de Desempate:** Resolución por penales cuando el xG o las probabilidades generan un empate en el marcador tras el tiempo regular.

*Nota: Los datos de entrada provienen de estadísticas oficiales de selecciones y enfrentamientos previos.*

---

## 📊 Resultados por Modelo

### 1️⃣ Predicción de Octavos (2026-07-06)
*(Archivo: `Resultados_Mundial_2026_20260706_0105.csv`)*

**Octavos de final**
| Local | Visitante | Marcador | 🏆 Ganador |
|:---|:---|:---:|:---|
| Francia | Paraguay | 2 – 0 | **Francia** |
| Marruecos | Canadá | 3 – 1 | **Marruecos** |
| Noruega | Brasil | 2 – 1 | **Noruega** |
| Inglaterra | México | 1 – 1 *(penales)* | **Inglaterra** |
| España | Portugal | 2 – 1 | **España** |
| Bélgica | Estados Unidos | 3 – 2 | **Bélgica** |
| Argentina | Egipto | 2 – 0 | **Argentina** |
| Colombia | Suiza | 1 – 1 *(penales)* | **Colombia** |

**Cuartos de final**
| Local | Visitante | Marcador | 🏆 Ganador |
|:---|:---|:---:|:---|
| Francia | Marruecos | 1 – 0 | **Francia** |
| Noruega | Inglaterra | 1 – 2 | **Inglaterra** |
| España | Bélgica | 2 – 1 | **España** |
| Argentina | Colombia | 3 – 1 | **Argentina** |

> **📌 Semifinalistas:** Francia, Inglaterra, España y Argentina.

---

### 2️⃣ Predicción de Cuartos (2026-07-09)
*(Archivo: `Resultados_Cuartos_2026_20260709_1223 (1).csv`)*

**Cuartos de final**
| Local | Visitante | Marcador | 🏆 Ganador |
|:---|:---|:---:|:---|
| Francia | Marruecos | 2 – 1 | **Francia** |
| Noruega | Inglaterra | 4 – 5 | **Noruega** * |
| España | Bélgica | 2 – 1 | **España** |
| Suiza | Argentina | 0 – 2 | **Argentina** |

*\* Nota: El marcador inusual de 4-5 refleja una posible resolución atípica o tanda de penales con avance de Noruega según esta ejecución.*

> **📌 Semifinalistas:** Francia, Noruega, España y Argentina.

---

### 3️⃣ Predicción de 16avos (Torneo Completo)
*(Archivo: `Resultados_16avos.csv`)*

Esta ejecución es la más detallada y abarca todas las fases eliminatorias.

**Dieciseisavos de Final**
| Local | Visitante | Marcador | xG (L - V) | 🏆 Avanza | Clasificación |
|:---|:---|:---:|:---:|:---|:---|
| Sudáfrica | Canadá | **1 – 0** | 2.08 - 0.87 | **Sudáfrica** | Tiempo regular |
| Brasil | Japón | **1 – 2** | 1.44 - 1.15 | **Japón** | Tiempo regular |
| Alemania | Paraguay | **7 – 1** | 2.36 - 0.58 | **Alemania** | Tiempo regular |
| Países Bajos | Marruecos | **1 – 0** | 1.53 - 0.90 | **Países Bajos** | Tiempo regular |
| Costa de Marfil | Noruega | **4 – 1** | 2.06 - 1.01 | **Costa de Marfil**| Tiempo regular |
| Francia | Suecia | **1 – 0** | 1.43 - 0.81 | **Francia** | Tiempo regular |
| México | República Checa | **3 – 0** | 1.69 - 0.80 | **México** | Tiempo regular |
| Argentina | Corea del Sur | **1 – 1** | 1.63 - 0.92 | **Corea del Sur** | Penales |
| Bélgica | Catar | **1 – 1** | 2.22 - 0.83 | **Catar** | Penales |
| EE. UU. | Bosnia-Herzegovina| **2 – 2** | 1.19 - 0.96 | **EE. UU.** | Penales |
| España | Haití | **1 – 1** | 2.55 - 0.73 | **España** | Penales |
| Inglaterra | Escocia | **5 – 0** | 2.81 - 0.69 | **Inglaterra** | Tiempo regular |
| Suiza | Curazao | **2 – 0** | 2.09 - 0.97 | **Suiza** | Tiempo regular |
| Portugal | Turquía | **6 – 1** | 2.15 - 1.12 | **Portugal** | Tiempo regular |
| Colombia | Australia | **6 – 0** | 1.50 - 1.07 | **Colombia** | Tiempo regular |
| Uruguay | Ecuador | **1 – 0** | 1.61 - 1.03 | **Uruguay** | Tiempo regular |

**Cuartos de final**
| Local | Visitante | Marcador | 🏆 Ganador |
|:---|:---|:---:|:---|
| Sudáfrica | Países Bajos | 1 – 2 | **Países Bajos** |
| Costa de Marfil | México | 4 – 1 | **Costa de Marfil** |
| Catar | España | 0 – 0 *(penales)* | **España** |
| Portugal | Uruguay | 2 – 0 | **Portugal** |

**Semifinales**
| Local | Visitante | Marcador | 🏆 Ganador |
|:---|:---|:---:|:---|
| Países Bajos | Costa de Marfil | 2 – 2 *(penales)* | **Países Bajos** |
| España | Portugal | 4 – 1 | **España** |

**Gran Final**
| Local | Visitante | Marcador | 🏆 Campeón |
|:---|:---|:---:|:---|
| Países Bajos | España | 0 – 1 | 🥇 **España** |

> 🥈 **Subcampeón:** Países Bajos

---

## 📈 Tablas Resumen (Todos los Partidos)

### 🏆 Ganadores por Partido

| Predicción | Fase | Enfrentamiento | Equipo Ganador |
|:---|:---|:---|:---|
| **Octavos** | Octavos | Francia vs Paraguay | **Francia** |
| **Octavos** | Octavos | Marruecos vs Canadá | **Marruecos** |
| **Octavos** | Octavos | Noruega vs Brasil | **Noruega** |
| **Octavos** | Octavos | Inglaterra vs México | **Inglaterra** |
| **Octavos** | Octavos | España vs Portugal | **España** |
| **Octavos** | Octavos | Bélgica vs Estados Unidos | **Bélgica** |
| **Octavos** | Octavos | Argentina vs Egipto | **Argentina** |
| **Octavos** | Octavos | Colombia vs Suiza | **Colombia** |
| **Octavos** | Cuartos | Francia vs Marruecos | **Francia** |
| **Octavos** | Cuartos | Noruega vs Inglaterra | **Inglaterra** |
| **Octavos** | Cuartos | España vs Bélgica | **España** |
| **Octavos** | Cuartos | Argentina vs Colombia | **Argentina** |
| **Cuartos** | Cuartos | Francia vs Marruecos | **Francia** |
| **Cuartos** | Cuartos | Noruega vs Inglaterra | **Noruega** |
| **Cuartos** | Cuartos | España vs Bélgica | **España** |
| **Cuartos** | Cuartos | Suiza vs Argentina | **Argentina** |
| **16avos** | Cuartos | Sudáfrica vs Países Bajos | **Países Bajos** |
| **16avos** | Cuartos | Costa de Marfil vs México | **Costa de Marfil** |
| **16avos** | Cuartos | Catar vs España | **España** |
| **16avos** | Cuartos | Portugal vs Uruguay | **Portugal** |
| **16avos** | Semis | Países Bajos vs Costa de Marfil | **Países Bajos** |
| **16avos** | Semis | España vs Portugal | **España** |
| **16avos** | Final | Países Bajos vs España | **España** |

### ⚽ Marcadores por Partido

| Predicción | Fase | Enfrentamiento | Marcador |
|:---|:---|:---|:---:|
| **Octavos** | Octavos | Francia vs Paraguay | **2 – 0** |
| **Octavos** | Octavos | Marruecos vs Canadá | **3 – 1** |
| **Octavos** | Octavos | Noruega vs Brasil | **2 – 1** |
| **Octavos** | Octavos | Inglaterra vs México | **1 – 1** *(penales)* |
| **Octavos** | Octavos | España vs Portugal | **2 – 1** |
| **Octavos** | Octavos | Bélgica vs Estados Unidos | **3 – 2** |
| **Octavos** | Octavos | Argentina vs Egipto | **2 – 0** |
| **Octavos** | Octavos | Colombia vs Suiza | **1 – 1** *(penales)* |
| **Octavos** | Cuartos | Francia vs Marruecos | **1 – 0** |
| **Octavos** | Cuartos | Noruega vs Inglaterra | **1 – 2** |
| **Octavos** | Cuartos | España vs Bélgica | **2 – 1** |
| **Octavos** | Cuartos | Argentina vs Colombia | **3 – 1** |
| **Cuartos** | Cuartos | Francia vs Marruecos | **2 – 1** |
| **Cuartos** | Cuartos | Noruega vs Inglaterra | **4 – 5** |
| **Cuartos** | Cuartos | España vs Bélgica | **2 – 1** |
| **Cuartos** | Cuartos | Suiza vs Argentina | **0 – 2** |
| **16avos** | Cuartos | Sudáfrica vs Países Bajos | **1 – 2** |
| **16avos** | Cuartos | Costa de Marfil vs México | **4 – 1** |
| **16avos** | Cuartos | Catar vs España | **0 – 0** *(penales)* |
| **16avos** | Cuartos | Portugal vs Uruguay | **2 – 0** |
| **16avos** | Semis | Países Bajos vs Costa de Marfil | **2 – 2** *(penales)* |
| **16avos** | Semis | España vs Portugal | **4 – 1** |
| **16avos** | Final | Países Bajos vs España | **0 – 1** |

---

## 💻 Uso de los Datos

Los archivos CSV pueden abrirse con Excel o Google Sheets, pero están especialmente optimizados para ser procesados con herramientas de análisis de datos. 

Si deseas cruzar los marcadores predichos con los `xG` para evaluar la consistencia usando **Python (pandas)**:

```python
import pandas as pd

# Cargar la predicción de 16avos
df = pd.read_csv("Resultados_16avos.csv")

# Filtrar los partidos que se decidieron por penales
partidos_penales = df[df['Detalle'].str.contains("penales", na=False)]
print(partidos_penales[['Fase', 'Local', 'Visitante', 'Avanza']])

```
## 👥 Integrantes del Equipo

* Salón Ynga, Dangelo Emanuel
* Tuesta Chiquizuta, María Carmen
* Ramos Ocampo, Kevin
* Chichipe Huamán, Jhenuar
* **Loja Alvarado, Rayan Emilio**
* Ynga Vargas, Roberto José
