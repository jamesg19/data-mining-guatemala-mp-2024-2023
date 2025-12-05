# data-mining-guatemala-mp-2024-2023
Data Mining Project (USAC). Analysis of Criminal Acts in Guatemala (emphasis MP) using R, Apriori, FP-Growth, and K-Means. The objective is to discover patterns and generate concrete proposals for improvement.



# Documentación Técnica — Proyecto *Project Phase-1*

**Autor:** James Gramajo  
**Lenguaje:** R (versión 4.5.1)  
**Entorno de desarrollo:** RStudio  
**Tipo de proyecto:** Análisis de asociación con el algoritmo *Apriori*  
**Salida:** HTML Notebook  

---

## 1. Descripción General

El presente documento describe la configuración técnica y ejecución del proyecto **Project Phase-1**, desarrollado en **RStudio** con **R versión 4.5.1**.  
El objetivo principal es aplicar el algoritmo de **reglas de asociación (Apriori)** al conjunto de datos *mp-sindicados-2024.xlsx*, para descubrir relaciones significativas entre variables categóricas.

---

## 2. Requisitos del Entorno

### 2.1 Versión del Lenguaje
- **R:** 4.5.1  
  Es importante mantener esta versión o una compatible (≥ 4.3) para asegurar la compatibilidad con las librerías utilizadas.

### 2.2 Software Requerido
- **RStudio** (versión ≥ 2023.09)
  - IDE recomendado para ejecución, depuración y visualización.
- **Microsoft Excel o LibreOffice**  
  - Requerido solo para validar el archivo de datos de entrada.

### 2.3 Librerías R necesarias
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

#### Instalación:
```r
install.packages("arules")
install.packages("readxl")
install.packages("fim4r")
install.packages("ggplot2")
install.packages("ggalt")
```
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

