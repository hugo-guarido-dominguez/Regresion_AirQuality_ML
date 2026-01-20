# Air Quality Madrid — Predicción de Benceno (C6H6) con Regresión Simple y Múltiple

Análisis de datos de calidad del aire en Madrid para **modelar y predecir la concentración de benceno (C6H6)** a partir de contaminantes, señales de sensores (serie PT08) y variables meteorológicas. El repositorio incluye dos notebooks: uno centrado en **regresión lineal simple** (un predictor) y otro en **regresión múltiple** (varios predictores + modelos lineales y Random Forest).

## Objetivo
- Identificar **qué atributo predice de forma más directa** la concentración de benceno (C6H6).
- Comparar modelos y cuantificar rendimiento con métricas estándar de regresión.
- Interpretar la contribución de variables (correlaciones, coeficientes e importancias).

---

## Dataset
Conjunto de mediciones horarias (≈9.3k registros) con variables como:
- Contaminantes: `CO(GT)`, `NMHC(GT)`, `NOx(GT)`, `NO2(GT)`, `C6H6(GT)` (objetivo)
- Sensores: `PT08.S1(CO)`, `PT08.S2(NMHC)`, `PT08.S3(NOx)`, `PT08.S4(NO2)`, `PT08.S5(O3)`
- Meteorología: `T`, `RH`, `AH`
- Tiempo: `Date`, `Time` (en el análisis múltiple se deriva `Hour`)

---

## Estructura del repositorio
- `PW2_E1_GRUPO[3C6].ipynb` — Regresión simple: selección de predictores y comparación de modelos 1D.
- `PW2_E2_GROUP[3C6].ipynb` — Regresión múltiple: modelos lineales, Ridge y Random Forest + análisis de importancia.
- `RESEARCH_PWI_GROUP[3C6].pdf` — Informe con metodología, resultados y conclusiones.
- `air_quality.csv` — Fichero CSV con datos.

---

## Metodología

### 1) Exploración y limpieza
- Revisión de distribuciones, correlaciones y relaciones con el objetivo.
- Tratamiento del código de error **-200** (marcador de medición inválida): sustitución por `NaN`.
- Eliminación de columnas sin información (p. ej., columnas “Unnamed” vacías).
- Filtrado de filas con valores incompletos para entrenar con un dataset consistente.

### 2) Regresión lineal simple (Notebook E1)
- Selección de candidatos por **correlación** con `C6H6(GT)`.
- Entrenamiento de un modelo por predictor (train/test **80/20** con semilla fija).
- Evaluación comparativa con:
  - **R²**, **MAE**, **MSE**, **RMSE**
- Visualizaciones: dispersión + recta de regresión y análisis del ajuste.

**Resultado destacado:** el predictor individual más informativo es **`PT08.S2(NMHC)`**, con un ajuste muy alto (R² ~ 0.97) frente al resto de candidatos.

### 3) Regresión múltiple (Notebook E2)
- Construcción de un conjunto de variables con contaminantes, sensores, meteorología y `Hour`.
- Modelos entrenados y comparados:
  - **Regresión lineal múltiple**
  - **Ridge**
  - **Random Forest Regressor** (mejor configuración encontrada sin búsqueda exhaustiva)
- Evaluación con las mismas métricas (R², MAE, MSE, RMSE).
- Interpretación del mejor modelo:
  - Valores reales vs predichos
  - Residuos
  - **Importancia de variables** (Random Forest)

**Resultado destacado:** **Random Forest** logra el mejor rendimiento (R² ~ 0.999). La variable dominante en la predicción vuelve a ser **`PT08.S2(NMHC)`**, seguida a distancia por señales como `PT08.S4(NO2)` y `CO(GT)`.

---