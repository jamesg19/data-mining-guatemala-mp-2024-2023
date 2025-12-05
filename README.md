# data-mining-guatemala-mp-2024-2023
Data Mining Project (USAC). Analysis of Criminal Acts in Guatemala (emphasis MP) using R, Apriori, FP-Growth, and K-Means, Decision Trees, Random forest and neural network. The objective is to discover patterns and generate concrete proposals for improvement.



# Documentación Técnica — Proyecto *Project Phase 1 & 2*

**Autor:** James Gramajo  
**Lenguaje:** R (versión 4.5.1)  Python (versión 3.11)
**Entorno de desarrollo:** RStudio( Notebook R), Dataspell (Jupyter Notebook)  
**Fecha de creación:** Noviembre 2025 
**Tipo de proyecto:** Mineria de datos *Decision Trees*, *Random forest* and *neural network*.  
**Salida:** HTML Notebook  

---

## 1. Descripción General

El presente documento describe la configuración técnica y ejecución del proyecto **Project Phase-2**, desarrollado en **RStudio** con **R versión 4.5.1** y **DataSpell** **Python**.  
El objetivo principal es aplicar el algoritmo de **Decision Trees**, **Random forest** and **Neural network** al conjunto de datos *mp-sindicados-2024-2023.xlsx*, para descubrir relaciones significativas entre variables categóricas.

---

## 2. Requisitos del Entorno

### 2.1 Versión del Lenguaje
- **R:** 4.5.1  
  Es importante mantener esta versión o una compatible (≥ 4.3) para asegurar la compatibilidad con las librerías utilizadas.
- **Python:** 3.11
  Asegura compatibilidad con las librerías de análisis de datos y machine learning utilizadas en el proyecto.


### 2.2 Software Requerido
- **RStudio** (versión ≥ 2023.09)
  - IDE recomendado para ejecución, depuración y visualización.
- **DataSpell** (versión ≥ 2024.1)
  - IDE recomendado para ejecución, depuración y visualización de notebooks Python.
- **Rtools** (para Windows)
  - Necesario para compilar paquetes R que contienen código en C/C++.
- **Microsoft Excel o LibreOffice**  
  - Requerido solo para validar el archivo de datos de entrada.

### 2.3.1 Librerías R necesarias
| Librería | Descripción | Uso principal |
|-----------|-------------|---------------|
| `arules` | Implementa algoritmos de minería de reglas de asociación, incluyendo *Apriori*. | Generar reglas de asociación. |
| `readxl` | Permite leer archivos Excel (.xlsx, .xls). | Importar el dataset. |
| `fim4r` | Permite leer archivos Excel (.xlsx, .xls). | Acelerar el proceso de minería de reglas de asociación y el descubrimiento de conjuntos de ítems frecuentes con alta eficiencia.|
| `ggplot2` | Sistema avanzado de visualización de datos basado en la gramática de gráficos. | Crear gráficos de dispersión, análisis de clusters y visualizaciones estadísticas. |
| `ggalt` | Extiende ggplot2 con funciones adicionales para resaltar o delinear grupos. | Usado para `geom_encircle()`, que resalta los límites visuales de los clusters. |
| `rpart` | Biblioteca para crear modelos de árboles de decisión. | Clasificar o predecir valores mediante árboles recursivos. |
| `rpart.plot` | Herramienta para visualizar árboles generados con rpart. | Mostrar estructuras de árboles de forma clara y comprensible. |
| `randomForest` | Implementa el algoritmo de bosques aleatorios. | Mejorar precisión combinando múltiples árboles para clasificación o regresión. |

#### 2.3.2 Instalación:
```r
install.packages("arules")
install.packages("readxl")
install.packages("fim4r")
install.packages("ggplot2")
install.packages("ggalt")
install.packages("rpart")
install.packages("rpart.plot")
install.packages("randomForest")
```
### 2.4.1 Librerías Python necesarias
| Librería | Descripción | Uso principal |
|----------|-------------|---------------|
| `pandas` (pd) | Librería para manipulación y análisis de datos | Estructuras de datos (DataFrame, Series) y operaciones de limpieza, transformación, análisis y visualización de datos |
| `numpy` (np) | Librería fundamental para computación científica | Arrays multidimensionales, operaciones matemáticas, álgebra lineal y funciones estadísticas |
| `tensorflow` (tf) | Framework de código abierto para machine learning | Creación, entrenamiento y despliegue de modelos de aprendizaje automático y deep learning |
| `tensorflow.keras.models.Sequential` | API para crear modelos de redes neuronales secuenciales | Construcción de modelos de deep learning capa por capa en orden lineal |
| `tensorflow.keras.layers.Dense` | Tipo de capa en redes neuronales | Capas completamente conectadas donde cada neurona recibe entrada de todas las neuronas de la capa anterior |
| `tensorflow.keras.utils.to_categorical` | Función de utilidad para preprocesamiento | Convertir etiquetas de clase en vectores one-hot para problemas de clasificación multiclase |
| `sklearn.preprocessing.StandardScaler` | Clase para normalización de datos | Estandarización de características (media=0, desviación estándar=1) para mejorar el rendimiento de algoritmos |
| `sklearn.preprocessing.LabelEncoder` | Clase para codificación de etiquetas | Convertir etiquetas categóricas en valores numéricos para modelos que requieren entrada numérica |
| `sklearn.model_selection.train_test_split` | Función para división de datos | Dividir conjuntos de datos en subconjuntos de entrenamiento y prueba para validación de modelos |


#### 2.4.2 Instalación:
```bash
# Instalar pandas
pip install pandas

# Instalar numpy
pip install numpy

# Instalar scikit-learn
pip install scikit-learn

# Instalar TensorFlow
pip install tensorflow

```

### 2.4.3 Notas sobre las librerías

### pandas (`pd`)
- **Instalación**: `pip install pandas`
- **Alternativas**: `polars` (más rápido), `modin` (escalable)
- **Casos de uso**: Análisis exploratorio de datos, limpieza de datos, agregaciones, merging/joining de datasets

### numpy (`np`)
- **Instalación**: `pip install numpy`
- **Fundamental**: Base para muchas otras librerías científicas de Python
- **Rendimiento**: Implementado en C, muy eficiente para operaciones numéricas

### tensorflow (`tf`)
- **Instalación**: `pip install tensorflow` (CPU) o `pip install tensorflow-gpu` (GPU)
- **Alternativas**: `pytorch`, `jax`
- **Ecosistema**: Incluye Keras como API de alto nivel, TensorFlow Lite, TensorFlow.js

### scikit-learn (sklearn)
- **Instalación**: `pip install scikit-learn`
- **Modulos principales**:
  - `sklearn.preprocessing`: Preprocesamiento y normalización de datos
  - `sklearn.model_selection`: Validación cruzada, división de datos, búsqueda de hiperparámetros
  - `sklearn.metrics`: Métricas de evaluación de modelos
  - `sklearn.ensemble`: Algoritmos ensemble (Random Forest, Gradient Boosting)

# 3 Cargar archivo (asegurarse que sea la ruta correcta si descargas el repo no deberia variar)
```r
data_sindicados_unida <- read_excel("Data\mp-sindicados-2024-2023.xlsx")
data_agraviados_unida <- read_excel("Data\mp-agraviados-2024-2023.xlsx")
```

## 3.1 Ver las primeras filas para inspección rápida
```r
head(data_sindicados_unida)
head(data_agraviados_unida)
```

## 4. Diccionario de Variables Utilizadas en Clusters

El siguiente diccionario permite interpretar las variables utilizadas para el análisis de *clusters* y reglas de asociación.

---

### Variable: `g_edad_de0_80ymas`

| Código | Rango de Edad |
|--------|----------------|
| 1 | 0–4 |
| 2 | 5–9 |
| 3 | 10–14 |
| 4 | 15–19 |
| 5 | 20–24 |
| 6 | 25–29 |
| 7 | 30–34 |
| 8 | 35–39 |
| 9 | 40–44 |
| 10 | 45–49 |
| 11 | 50–54 |
| 12 | 55–59 |
| 13 | 60–64 |
| 14 | 65–69 |
| 15 | 70–74 |
| 16 | 75–79 |

---

### Variable: `Mes_hecho`

| Código | Mes |
|--------|------|
| 1 | Enero |
| 2 | Febrero |
| 3 | Marzo |
| 4 | Abril |
| 5 | Mayo |
| 6 | Junio |
| 7 | Julio |
| 8 | Agosto |
| 9 | Septiembre |
| 10 | Octubre |
| 11 | Noviembre |
| 12 | Diciembre |
| 99 | Ignorado |

---

### Variable: `Depto_ocu_hecho`

| Código | Departamento |
|--------|---------------|
| 1 | Guatemala |
| 2 | El Progreso |
| 3 | Sacatepéquez |
| 4 | Chimaltenango |
| 5 | Escuintla |
| 6 | Santa Rosa |
| 7 | Sololá |
| 8 | Totonicapán |
| 9 | Quetzaltenango |
| 10 | Suchitepéquez |
| 11 | Retalhuleu |
| 12 | San Marcos |
| 13 | Huehuetenango |
| 14 | Quiché |
| 15 | Baja Verapaz |
| 16 | Alta Verapaz |
| 17 | Petén |
| 18 | Izabal |
| 19 | Zacapa |
| 20 | Chiquimula |
| 21 | Jalapa |
| 22 | Jutiapa |
| 23 | Extranjero |
| 99 | Ignorado |

---

### Variable: `Sex`

| Código | Sexo |
|--------|------|
| 1 | Hombre |
| 2 | Mujer |
| 9 | Ignorado |

