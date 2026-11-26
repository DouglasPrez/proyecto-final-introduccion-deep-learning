# Proyecto final de Introducción a Deep Learning

**Douglas Pérez**  
**Carnet 22003865**

En este proyecto aplico tres arquitecturas de redes neuronales a problemas de videojuegos. En cada notebook explico cómo trabajé los datos, cómo entrené la red y qué aprendí de los resultados.

## Modelos

| Notebook | Modelo | Problema |
| --- | --- | --- |
| [01_MLP.ipynb](01_MLP.ipynb) | MLP en PyTorch | Predecir si gana el equipo azul en League of Legends con datos de los primeros diez minutos. |
| [02_CNN.ipynb](02_CNN.ipynb) | CNN en PyTorch | Clasificar siete entornos de Minecraft a partir de capturas del juego. |
| `03_RNN.ipynb` | Red recurrente | Pendiente. |

## MLP de League of Legends

Usé 9,879 partidas clasificatorias de nivel alto. A partir de las estadísticas al minuto diez, calculé las diferencias de oro, experiencia, bajas, visión y objetivos entre los equipos. Probé dos redes y elegí la de una capa oculta de 32 neuronas por su menor pérdida en validación. [Dataset de Bobbyscience en Kaggle](https://www.kaggle.com/datasets/bobbyscience/league-of-legends-diamond-ranked-games-10-min)

| Modelo | Exactitud en prueba | F1 macro | Pérdida en prueba |
| --- | ---: | ---: | ---: |
| MLP de una capa | 71.26 % | 0.7125 | 0.5350 |
| Regresión logística | 71.93 % | 0.7193 | 0.5288 |

En este conjunto de prueba, la regresión logística obtuvo 0.67 puntos porcentuales más de exactitud. Me quedo con esa comparación porque muestra cuánto aporta el MLP frente a una opción más sencilla. Para saber si el resultado se mantiene, probaría con partidas de otros parches y rangos.

## CNN de Minecraft

Usé 11,724 capturas de siete entornos. Conservé 10,867 después de retirar duplicados exactos y grupos visualmente similares con etiquetas distintas. Hice el split por grupos y comparé la misma CNN de tres bloques con y sin data augmentation. [Minecraft Vision Dataset, de Nininkkka](https://huggingface.co/datasets/Nininkkka/Minecraft-vision-dataset)

| Modelo elegido | Accuracy en test | Macro F1-score | Test loss |
| --- | ---: | ---: | ---: |
| CNN con augmentation | 98.61 % | 0.9847 | 0.0549 |

En validation, la variante con augmentation obtuvo un macro F1-score de 0.9934 frente a 0.9860 de la CNN base. En test, las confusiones que más se repitieron fueron end → caves, forests → caves y forests → plains.

## Cómo ejecutar los notebooks

Abre el [MLP en Google Colab](https://colab.research.google.com/github/DouglasPrez/proyecto-final-introduccion-deep-learning/blob/main/01_MLP.ipynb) y ejecuta las celdas en orden desde **Entorno de ejecución → Ejecutar todas**. El notebook corre en CPU y descarga la versión 1 del dataset. Si Kaggle pide iniciar sesión, descarga el ZIP desde el enlace del dataset y súbelo al entorno de Colab con el nombre `league-of-legends-v1.zip`.

Abre la [CNN en Google Colab](https://colab.research.google.com/github/DouglasPrez/proyecto-final-introduccion-deep-learning/blob/main/02_CNN.ipynb) y usa **Entorno de ejecución → Ejecutar todas**. El notebook descarga las siete clases desde la revisión indicada de Hugging Face e instala Git LFS en el runtime si hace falta.

Para ejecutarlos en Jupyter, instala las dependencias en un entorno de Python y abre cada notebook con ese entorno. La CNN también necesita Git LFS.

```bash
python -m pip install jupyter torch torchvision numpy pandas matplotlib pillow scikit-learn
```
