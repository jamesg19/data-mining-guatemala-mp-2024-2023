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

#### Instalación:
```r
install.packages("arules")
install.packages("readxl")
```
# Cargar archivo (asegurarse que sea la ruta correcta si descargas el repo no deberia variar)

data_sindicados_unida <- read_excel("Data\mp-sindicados-2024-2023.xlsx")

# Ver las primeras filas para inspección rápida

head(data_sindicados_unida)
