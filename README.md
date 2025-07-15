# ARS2025-PointCloudPipeline

Un pipeline modular para procesamiento, análisis y reconstrucción de nubes de puntos 3D desarrollado como parte del proyecto ARS2025 del Tecnológico de Monterrey.

## Descripción del Proyecto

Este repositorio contiene una suite de 6 scripts especializados en Python que implementan un pipeline completo para el procesamiento de nubes de puntos 3D. El proyecto combina las capacidades de Blender para visualización y análisis geométrico con Open3D para algoritmos avanzados de procesamiento de nubes de puntos.

### Objetivos Principales

- Automatizar el análisis de densidad en objetos 3D
- Implementar filtrado volumétrico preciso
- Aplicar técnicas de segmentación y clustering
- Reconstruir mallas a partir de nubes de puntos
- Realizar alineación automática entre diferentes nubes 3D

## Scripts del Pipeline

### 1. `code1_blender.py` - Análisis de Densidad
Detecta automáticamente el objeto más denso de la escena de Blender y analiza su geometría, extrayendo propiedades como volumen, centro de masa y distribución espacial.

### 2. `code2_densest_region.py` - Detección de Región Densa
Identifica la región con mayor densidad en la escena combinada y marca esa zona con una esfera transparente para visualización y análisis posterior.

### 3. `code3_filter_volume.py` - Filtrado Volumétrico
Filtra puntos fuera de un volumen definido (esfera o cilindro) y exporta los datos resultantes, permitiendo el aislamiento de regiones de interés específicas.

### 4. `code4_segment_dbscan.py` - Segmentación DBSCAN
Aplica segmentación con DBSCAN para agrupar estructuras 3D de forma automática, identificando clústeres y eliminando ruido de la nube de puntos.

### 5. `code5_poisson_mesh.py` - Reconstrucción de Malla
Reconstruye una malla 3D a partir de una nube de puntos utilizando Poisson Surface Reconstruction, generando superficies suaves y continuas.

### 6. `code6_alineacion.py` - Alineación Automática
Alinea automáticamente dos nubes 3D usando técnicas como RANSAC, FPFH e ICP, con parámetros optimizados para diferentes tipos de geometrías.

## Requisitos Técnicos

### Dependencias Principales
- **Python** 3.8 o superior
- **Open3D** 0.17.0 o superior
- **Blender** 3.0 o superior (con API Python)
- **NumPy** 1.21.0 o superior
- **Matplotlib** 3.5.0 o superior
- **Scikit-learn** 1.0.0 o superior

### Dependencias Adicionales
```
scipy
argparse
json
```

## Instalación

### 1. Clonar el Repositorio
```bash
git clone https://github.com/usuario/ARS2025-PointCloudPipeline.git
cd ARS2025-PointCloudPipeline
```

### 2. Instalar Dependencias
```bash
pip install open3d numpy matplotlib scikit-learn scipy
```

### 3. Verificar Instalación de Blender
Asegúrate de tener Blender instalado y accesible desde la línea de comandos:
```bash
blender --version
```

## Uso Básico

### Ejecución de Scripts Individuales

**Script 1 - Análisis en Blender:**
```bash
blender --background scene.blend --python code1_blender.py
```

**Scripts 2-6 - Procesamiento con Python:**
```bash
python code2_densest_region.py --input data.ply --output result.ply
python code3_filter_volume.py --input cloud.ply --shape sphere --radius 5.0
python code4_segment_dbscan.py --input cloud.ply --eps 0.5 --min_samples 10
python code5_poisson_mesh.py --input segmented.ply --depth 9
python code6_alineacion.py --source cloud1.ply --target cloud2.ply
```

### Pipeline Completo
Para ejecutar el pipeline completo en secuencia:
```bash
# 1. Análisis de densidad
blender --background scene.blend --python code1_blender.py

# 2. Detección de región densa
python code2_densest_region.py --input raw_cloud.ply --output dense_region.ply

# 3. Filtrado volumétrico
python code3_filter_volume.py --input dense_region.ply --output filtered.ply

# 4. Segmentación
python code4_segment_dbscan.py --input filtered.ply --output segmented.ply

# 5. Reconstrucción de malla
python code5_poisson_mesh.py --input segmented.ply --output mesh.ply

# 6. Alineación (opcional)
python code6_alineacion.py --source mesh.ply --target reference.ply --output aligned.ply
```

## Archivos de Salida

Cada script genera archivos de salida específicos:
- **Archivos PLY**: Nubes de puntos y mallas procesadas
- **Archivos JSON**: Reportes y metadatos del procesamiento
- **Archivos PNG**: Visualizaciones y gráficas de análisis
- **Archivos TXT**: Matrices de transformación y parámetros

## Autor

**Alfonso Solís**  
Tecnológico de Monterrey  
Proyecto ARS2025

## Licencia

Este proyecto está licenciado bajo la Licencia MIT - consulta el archivo [LICENSE](LICENSE) para más detalles.

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

**Versión**: 1.0.0  
**Última actualización**: Julio 2025
