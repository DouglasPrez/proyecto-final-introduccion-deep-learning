# Proyecto final de Introducción a Deep Learning

**Douglas Pérez**  
**Carnet 22003865**

En este proyecto aplico tres arquitecturas de redes neuronales a problemas de videojuegos. En cada notebook explico cómo trabajé los datos, cómo entrené la red y qué aprendí de los resultados.

## Modelos

| Notebook | Modelo | Problema |
| --- | --- | --- |
| [01_MLP.ipynb](01_MLP.ipynb) | MLP en PyTorch | Predecir si gana el equipo azul en League of Legends con datos de los primeros diez minutos. |
| [02_CNN.ipynb](02_CNN.ipynb) | CNN en PyTorch | Clasificar siete entornos de Minecraft a partir de capturas del juego. |
| [03_RNN.ipynb](03_RNN.ipynb) | LSTM y GRU en PyTorch | Pronosticar la ventaja de net worth de Radiant del minuto 20 al 25 en Dota 2. |

## MLP de League of Legends

Usé 9,879 partidas clasificatorias de nivel alto. A partir de las estadísticas al minuto diez, calculé las diferencias de oro, experiencia, bajas, visión y objetivos entre los equipos. Probé dos MLP y elegí el de una hidden layer de 32 unidades por su menor validation loss. [Dataset de Bobbyscience en Kaggle](https://www.kaggle.com/datasets/bobbyscience/league-of-legends-diamond-ranked-games-10-min)

| Modelo | Accuracy en test | Macro F1-score | Test loss |
| --- | ---: | ---: | ---: |
| MLP de una capa | 71.26 % | 0.7125 | 0.5350 |
| Logistic regression | 71.93 % | 0.7193 | 0.5288 |

## CNN de Minecraft

Usé 11,724 capturas de siete entornos. Conservé 10,867 después de retirar duplicados exactos y grupos visualmente similares con etiquetas distintas. Hice el split por grupos y comparé la misma CNN de tres bloques con y sin data augmentation. [Minecraft Vision Dataset, de Nininkkka](https://huggingface.co/datasets/Nininkkka/Minecraft-vision-dataset)

| Modelo elegido | Accuracy en test | Macro F1-score | Test loss |
| --- | ---: | ---: | ---: |
| CNN con augmentation | 98.61 % | 0.9847 | 0.0549 |

En validation, la variante con augmentation obtuvo un macro F1-score de 0.9934 frente a 0.9860 de la CNN base. En test, las confusiones que más se repitieron fueron end → caves, forests → caves y forests → plains.

## Red recurrente de Dota 2

Usé cinco snapshots por partida para pronosticar la ventaja de net worth de Radiant al minuto 25. Conservé 72,424 partidas con la ventana completa y separé train, validation y test por fecha. Comparé una LSTM y una GRU con dos baselines de pronóstico. [Dataset v3 en Zenodo](https://zenodo.org/records/23166182) · [OpenDota](https://www.opendota.com/)

| Modelo | MAE en test | RMSE en test |
| --- | ---: | ---: |
| GRU | 3,694.81 | 4,689.07 |
| Persistencia | 4,143.39 | 5,113.96 |
| Tendencia lineal | 3,963.64 | 5,078.22 |

Elegí la GRU por su MAE en validation.

## Cómo ejecutar los notebooks

Abre el [MLP en Google Colab](https://colab.research.google.com/github/DouglasPrez/proyecto-final-introduccion-deep-learning/blob/main/01_MLP.ipynb) y ejecuta las celdas en orden desde **Entorno de ejecución → Ejecutar todas**. El notebook corre en CPU y descarga la versión 1 del dataset. Si Kaggle pide iniciar sesión, descarga el ZIP desde el enlace del dataset y súbelo al entorno de Colab con el nombre `league-of-legends-v1.zip`.

Abre la [CNN en Google Colab](https://colab.research.google.com/github/DouglasPrez/proyecto-final-introduccion-deep-learning/blob/main/02_CNN.ipynb) y usa **Entorno de ejecución → Ejecutar todas**. El notebook descarga las siete clases desde la revisión indicada de Hugging Face e instala Git LFS en el runtime si hace falta.

Abre la [red recurrente en Google Colab](https://colab.research.google.com/github/DouglasPrez/proyecto-final-introduccion-deep-learning/blob/main/03_RNN.ipynb) y usa **Entorno de ejecución → Ejecutar todas**. La primera corrida descarga el CSV v3 y el paquete de reproducibilidad desde Zenodo, unos 272 MB en total.

Para ejecutarlos en Jupyter, instala las dependencias con Python 3.11 o posterior y abre cada notebook con ese entorno. La CNN también necesita Git LFS.

```bash
python -m pip install jupyter torch torchvision numpy pandas matplotlib pillow scikit-learn
```

## Conclusiones

- En League of Legends, logistic regression obtuvo 0.67 puntos porcentuales más de accuracy que el MLP. Probaría con partidas de otros parches y rangos para ver si esa diferencia se mantiene.
- En Minecraft, el data augmentation mejoró el macro F1-score en validation. Probaría la CNN con capturas de otros mundos y paquetes de texturas para medir qué tan bien generaliza.
- En Dota 2, la GRU redujo el MAE de test un 10.83 % frente a persistencia. La mejora se concentró en partidas con cambios de al menos 2,000 unidades de net worth.
