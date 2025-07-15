# ARS2025-PointCloudPipeline 🔬

## Descripción del Proyecto

Este repositorio contiene un pipeline completo para el procesamiento, segmentación y alineación de nubes de puntos 3D desarrollado como parte del proyecto **ARS2025** del Tecnológico de Monterrey. El pipeline incluye 6 scripts especializados que cubren desde el análisis de densidad hasta la reconstrucción de mallas y alineación inteligente de nubes de puntos.

### Características Principales

- 🎯 **Análisis automático de densidad** en objetos 3D
- 🔍 **Detección de regiones densas** combinando múltiples objetos
- 🎛️ **Filtrado volumétrico** con geometrías cilíndricas y esféricas  
- 📊 **Segmentación con DBSCAN** y colorización de clústeres
- 🏗️ **Reconstrucción de mallas** usando Poisson Surface Reconstruction
- 🔄 **Alineación inteligente** con FPFH + RANSAC + ICP adaptativo

---

## 📦 Requisitos y Dependencias

### Dependencias de Python
```
open3d >= 0.17.0
numpy >= 1.21.0
matplotlib >= 3.5.0
scikit-learn >= 1.0.0
scipy >= 1.7.0
json
argparse
```

### Dependencias de Blender
```
bpy (Blender Python API)
bmesh
mathutils
```

### Versiones Recomendadas
- **Python**: 3.8 - 3.11
- **Blender**: 3.0 o superior
- **Open3D**: 0.17.0 o superior

---

## 🛠️ Instalación

### 1. Clonar el Repositorio
```bash
git clone https://github.com/usuario/ARS2025-PointCloudPipeline.git
cd ARS2025-PointCloudPipeline
```

### 2. Instalar Dependencias
```bash
pip install -r requirements.txt
```

### 3. Verificar Instalación de Blender
```bash
blender --version
```

---

## ▶️ Uso de los Scripts

### 1. `code1_blender.py` - Análisis de Densidad en Blender

Analiza automáticamente el objeto MESH más denso en la escena de Blender y extrae metadatos críticos.

```bash
blender --background your_scene.blend --python code1_blender.py
```

**Salida:**
- `mesh_metadata.json`: Metadatos del objeto (bounding box, centro, volumen, densidad)
- Reporte de análisis en consola

### 2. `code2_densest_region.py` - Detección de Región Densa

Detecta la región más densa combinando dos objetos 3D y coloca una esfera visualizadora.

```bash
python code2_densest_region.py --cloud cloud.ply --mesh mesh.ply --output densest_region.ply
```

**Parámetros:**
- `--cloud`: Ruta de la nube de puntos
- `--mesh`: Ruta de la malla 3D
- `--output`: Archivo de salida

**Salida:**
- Nube con esfera visualizadora en región densa
- Reporte JSON con coordenadas y densidad

### 3. `code3_filter_volume.py` - Filtrado Volumétrico

Filtra una nube 3D dentro de un volumen definido (cilindro o esfera).

```bash
python code3_filter_volume.py --input cloud.ply --output filtered.ply --shape cylinder --radius 5.0 --height 10.0
```

**Parámetros:**
- `--input`: Nube de puntos de entrada
- `--output`: Nube filtrada de salida
- `--shape`: Forma del filtro (`cylinder` o `sphere`)
- `--radius`: Radio del volumen
- `--height`: Altura (solo para cilindro)

**Salida:**
- `filtered.ply`: Nube filtrada
- `filter_report.json`: Reporte del proceso de filtrado

### 4. `code4_segment_dbscan.py` - Segmentación con DBSCAN

Segmenta nubes de puntos usando DBSCAN, colorea clústeres y elimina ruido.

```bash
python code4_segment_dbscan.py --input cloud.ply --output segmented.ply --eps 0.5 --min_samples 10
```

**Parámetros:**
- `--input`: Nube de puntos de entrada
- `--output`: Nube segmentada de salida
- `--eps`: Distancia máxima entre puntos vecinos
- `--min_samples`: Mínimo de puntos para formar un clúster

**Salida:**
- `segmented.ply`: Nube segmentada y coloreada
- `clustering_report.json`: Estadísticas de segmentación
- `clusters_visualization.png`: Visualización de clústeres

### 5. `code5_poisson_mesh.py` - Reconstrucción de Malla

Reconstruye una malla 3D desde una nube de puntos segmentada usando Poisson Surface Reconstruction.

```bash
python code5_poisson_mesh.py --input segmented.ply --output mesh.ply --depth 9 --density_threshold 0.1
```

**Parámetros:**
- `--input`: Nube de puntos segmentada
- `--output`: Malla reconstruida
- `--depth`: Profundidad del octree para Poisson
- `--density_threshold`: Umbral de densidad para filtrado

**Salida:**
- `mesh.ply`: Malla reconstruida
- `reconstruction_report.json`: Métricas de reconstrucción
- `mesh_visualization.png`: Visualización de la malla

### 6. `code6_alineacion.py` - Alineación Inteligente

Realiza alineación entre nubes 3D usando FPFH + RANSAC + ICP adaptativo con ML.

```bash
python code6_alineacion.py --source source.ply --target target.ply --output aligned.ply
```

**Parámetros:**
- `--source`: Nube de puntos fuente
- `--target`: Nube de puntos objetivo
- `--output`: Nube alineada de salida

**Salida:**
- `aligned.ply`: Nube alineada
- `alignment_metrics.json`: Métricas de alineación
- `alignment_visualization.pdf`: Visualización comparativa
- `transformation_matrix.txt`: Matriz de transformación

---

## 📤 Resultados Esperados

### Archivos de Salida por Script

| Script | Archivos de Salida | Descripción |
|--------|-------------------|-------------|
| `code1_blender.py` | `mesh_metadata.json` | Metadatos del objeto más denso |
| `code2_densest_region.py` | `densest_region.ply`, `density_report.json` | Región densa y reporte |
| `code3_filter_volume.py` | `filtered.ply`, `filter_report.json` | Nube filtrada y reporte |
| `code4_segment_dbscan.py` | `segmented.ply`, `clustering_report.json`, `clusters_visualization.png` | Segmentación y visualización |
| `code5_poisson_mesh.py` | `mesh.ply`, `reconstruction_report.json`, `mesh_visualization.png` | Malla y métricas |
| `code6_alineacion.py` | `aligned.ply`, `alignment_metrics.json`, `alignment_visualization.pdf` | Alineación completa |

### Métricas y Visualizaciones

- **Reportes JSON**: Contienen métricas cuantitativas de cada proceso
- **Visualizaciones PNG/PDF**: Gráficas comparativas y análisis visual
- **Archivos PLY**: Nubes de puntos y mallas procesadas
- **Matrices de transformación**: Para reproducibilidad en alineación

---

## 🧪 Pipeline Completo de Ejemplo

```bash
# 1. Análisis de densidad en Blender
blender --background scene.blend --python code1_blender.py

# 2. Detección de región densa
python code2_densest_region.py --cloud raw_cloud.ply --mesh mesh.ply --output dense_region.ply

# 3. Filtrado volumétrico
python code3_filter_volume.py --input dense_region.ply --output filtered.ply --shape cylinder --radius 3.0 --height 8.0

# 4. Segmentación DBSCAN
python code4_segment_dbscan.py --input filtered.ply --output segmented.ply --eps 0.3 --min_samples 15

# 5. Reconstrucción de malla
python code5_poisson_mesh.py --input segmented.ply --output reconstructed.ply --depth 10

# 6. Alineación con otra nube
python code6_alineacion.py --source reconstructed.ply --target reference.ply --output final_aligned.ply
```

---

## 👨‍🔬 Uso Académico

Este pipeline ha sido desarrollado como parte del proyecto **ARS2025** del Tecnológico de Monterrey, enfocado en el procesamiento avanzado de nubes de puntos 3D para aplicaciones de investigación académica. 

### Reportes Técnicos Asociados

Cada script está basado en un reporte técnico numerado que documenta la metodología, algoritmos y resultados experimentales:

1. **Reporte 1**: Análisis de densidad automático en objetos 3D
2. **Reporte 2**: Detección de regiones críticas en nubes de puntos
3. **Reporte 3**: Filtrado volumétrico avanzado
4. **Reporte 4**: Segmentación con clustering DBSCAN
5. **Reporte 5**: Reconstrucción de superficies con Poisson
6. **Reporte 6**: Alineación inteligente con machine learning

### Aplicaciones de Investigación

- Análisis de estructuras arquitectónicas
- Procesamiento de datos LiDAR
- Reconstrucción 3D de objetos complejos
- Análisis de deformaciones estructurales
- Modelado geométrico avanzado

---

## 👤 Autor

**Alfonso Solís**  
📧 a01705893@tec.mx  
🏫 Tecnológico de Monterrey  
🔬 Proyecto ARS2025

---

## 📄 Licencia

```
MIT License

Copyright (c) 2025 Alfonso Solís - Tecnológico de Monterrey

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

## 🔗 Referencias y Soporte

Para más información sobre el proyecto ARS2025 o soporte técnico, contacta al autor o consulta la documentación técnica asociada.

**Versión**: 1.0.0  
**Última actualización**: Julio 2025
