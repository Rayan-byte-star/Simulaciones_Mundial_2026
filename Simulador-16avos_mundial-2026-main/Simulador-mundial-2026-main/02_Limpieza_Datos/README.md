# 02_Limpieza_Datos

Esta carpeta contiene los procesos de tratamiento de los datasets crudos (`/Data/`).

## Contenido:
- **`02_Limpieza.ipynb`**: Notebook principal que realiza:
    - Eliminación de registros con valores nulos en variables clave.
    - Normalización de nombres de equipos (mapeo a un estándar único).
    - Imputación de valores faltantes mediante la media estadística.
    - Generación de los archivos de entrenamiento limpios.
