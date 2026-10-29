# Huella Compatible

Sistema recomendador de perros y gatos para adopción responsable.
Taller de Sistemas Recomendadores 2026-1 · Grupo 2: Carlos Sánchez y Cristian Molina.

El sistema empareja adoptantes con animales en adopción según la compatibilidad con su hogar. Es un híbrido en cascada:

1. **Filtro basado en conocimiento:** restricciones duras que salen del cuestionario (vivienda, niños, otras mascotas, presupuesto).
2. **Filtrado basado en contenido:** similitud de Gower ponderada por el usuario sobre 10 dimensiones de compatibilidad (75 %) + coseno TF-IDF sobre la descripción (25 %).
3. **Re-ranking de equidad y diversidad:** MMR + un impulso calibrado para los grupos que el sistema subexpone.

## Estructura

| Ruta | Contenido |
| --- | --- |
| `notebooks/HuellaCompatible_prototipo.ipynb` | Prototipo completo, comentado y ya ejecutado (actividad 2.3 y mediciones de la Parte 3) |
| `data/petfinder_train.csv` | 14.993 fichas reales de PetFinder.my (Kaggle) |
| `data/breed_labels.csv`, `color_labels.csv`, `state_labels.csv` | Tablas de nombres de razas, colores y estados |
| `figuras/` | Gráficas y diagrama de arquitectura usados en el informe |
| `requirements.txt` | Dependencias de Python |

## Cómo ejecutar

```bash
pip install -r requirements.txt
cd notebooks
jupyter notebook HuellaCompatible_prototipo.ipynb
```

Luego *Kernel → Restart & Run All*. La ejecución completa tarda menos de un minuto y regenera las gráficas de `figuras/`.

## Fuente de los datos

- **PetFinder.my Adoption Prediction** (Kaggle, 2019): https://www.kaggle.com/c/petfinder-adoption-prediction
- `petfinder_train.csv` es el conjunto de entrenamiento original de la competencia (14.993 fichas). Se obtuvo uniendo `train.csv` y `dev.csv` de la copia pública que usa AutoGluon (https://automl-mm-bench.s3.amazonaws.com/petfinder_kaggle.zip), sin las columnas de rutas de imágenes.
- Las tablas de etiquetas provienen de la misma competencia (copia en https://github.com/georgian-io/Multimodal-Toolkit).

## Resultados principales

- HR@10 de **30,2 %** en un catálogo de 500 animales (popularidad: 2,8 %; azar: 5,0 %), con cero violaciones de restricciones.
- Un animal recién publicado, sin interacciones, entra en el puesto 1 del perfil compatible.
- La medición de sesgos detectó una subexposición indirecta de los animales de pelaje negro (0,75). El re-ranking calibrado la lleva a 0,95 con 0,1 % de pérdida de relevancia.
