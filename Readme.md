# Clasificación de daños asociados al gusano cogollero en cultivos de maíz mediante arquitecturas ResNet.

**Autores:** Antonio Juan Capistran-Abundez, Vitervo López-Caballero, Noé Alejandro Castro-Sánchez, Juan Reyes-Reyes, Marisol Cervantes-Bobadilla, Hugo Estrada-Esquivel and Lucia Morales-Morales

***
# Contenido

* [1. Descripción general](#1-descripción-general)
* [2. Contexto de la investigación](#2-contexto-de-la-investigación)
* [3. Descripción del conjunto de datos](#3-descripción-del-conjunto-de-datos)
* [4. Preprocesamiento y aumento de imágenes](#4-preprocesamiento-y-aumento-de-imágenes)
* [5. Arquitecturas de aprendizaje profundo](#5-arquitecturas-de-aprendizaje-profundo)
* [6. Métricas de evaluación](#6-métricas-de-evaluación)
* [7. Resultados experimentales](#6--de-evaluación)


# 1. Descripción general

Este repositorio proporciona los recursos asociados y el flujo de trabajo experimental del estudio académico:

«Identificación de daños causados ​​por el gusano cogollero en cultivos de maíz a partir de imágenes de campo capturadas mediante teléfonos inteligentes y drones: una evaluación comparativa de arquitecturas ResNet».

El estudio evalúa cuatro arquitecturas de redes neuronales convolucionales residuales (ResNet-18, ResNet-34, ResNet-50 y ResNet-101), empleando aprendizaje por transferencia (*transfer learning*) a partir de ImageNet, para clasificar imágenes de cultivos de maíz en dos categorías:

Plaga (1): Imágenes de maíz que presentan daños visibles asociados al gusano cogollero (*Spodoptera frugiperda*).

Sano (0): Imágenes de maíz sin daños visibles asociados a dicha plaga.

El objetivo es determinar qué arquitectura ResNet ofrece el mejor rendimiento de clasificación bajo un protocolo experimental común.

# 2. Contexto de la investigación

El gusano cogollero (*Spodoptera frugiperda*) representa una de las principales amenazas fitosanitarias para la producción de maíz. La identificación temprana de los daños que provoca es fundamental para facilitar el monitoreo de los cultivos y la toma de decisiones agrícolas.

Esta investigación explora técnicas de aprendizaje profundo (*deep learning*) aplicadas a imágenes obtenidas en condiciones reales de campo mediante dispositivos accesibles, tales como un teléfono inteligente y un dron DJI Mini 3.

Área de estudio: Jojutla, Tlaltizapán, Tlaquiltenango y Xochitepec, Morelos, México.

# 3. Descripción del conjunto de datos

El conjunto de datos experimental consta de 2500 imágenes de campo distribuidas equitativamente entre dos clases.

| Clase | Etiqueta | Imagénes |
| :--- | :---: | ---: |
| Plaga | 1 | 1,250 |
| Sana | 0 | 1,250 |
| **Total** | | **2,500** |

**Partición del conjunto de datos**

El conjunto de datos se dividió mediante muestreo estratificado:

| Subconjunto | Porcentaje | Imagénes |
| :--- | :---: | ---: |
| Entrenamiento | 70% | 1,750 |
| Validación | 15% | 375 |
| Prueba | 15% | 375 |
| **Total** | 100% | **2,500** |

# 4. Preprocesamiento y aumento de imágenes

Todas las imágenes se redimensionaron a 224 × 224 píxeles para proporcionar una resolución de entrada uniforme a las arquitecturas preentrenadas.

La normalización de las imágenes se realizó utilizando las medias y desviaciones estándar de los canales de ImageNet.

El aumento de datos se aplicó exclusivamente al subconjunto de entrenamiento con el fin de incrementar la variabilidad visual y reducir el sobreajuste.

Las transformaciones incluyeron: Recorte aleatorio y redimensionamiento, Rotación, Traslación, Escalado, Inversión horizontal, Ajuste de brillo, Ajuste de contraste, Ajuste de saturación y tono, Desenfoque gaussiano.

# 5. Arquitecturas de aprendizaje profundo

Se evaluaron cuatro arquitecturas ResNet preentrenadas:

| Arquitectura | Dimensiones de las características
| :--- | :---: |
| ResNet-18 | 512 |
| ResNet-34 | 512 |
| ResNet-50 | 2048 |
| ResNet-101 | 2048 |

Todas las arquitecturas utilizaron pesos previamente entrenados de ImageNet y un cabezal de clasificación binaria.

**Cabezal de clasificación**

La capa de clasificación original, diseñada para 1.000 clases, fue sustituida por un cabezal de clasificación binaria compuesto por:

Una capa lineal (512 unidades).

Activación ReLU.

Dropout (0,30).

Una capa de salida lineal (1 unidad).

Función sigmoide para la estimación de probabilidades durante la inferencia.

Los vectores de características provenientes de los distintos modelos base (backbones) se proyectaron hacia una representación común de 512 dimensiones.

**Configuración del entrenamiento**

Las cuatro arquitecturas se entrenaron utilizando un protocolo experimental idéntico.

| Parámetro | Configuración
| :--- | :---: |
| Pesos preentrenados | ImageNet |
| Resolución de entrada | 224 × 224 |
| Tarea de clasificación | Binaria |
| Función de pérdida| BCEWithLogitsLoss |
| Optimizador| Adam |
| Número máximo de épocas| 30 |
| Paciencia para la detención temprana (early stopping)| 5 |
| Dropout| 0,30 |
| Umbral de decisión| 0,50 |

# 6. Métricas de evaluación

Los modelos se evaluaron utilizando el subconjunto de prueba independiente.

Se consideraron las siguientes métricas:

* Exactitud.

* Precisión.

* Sensibilidad (exhaustividad).

* Especificidad.

* Puntuación F1.

* Área bajo la curva ROC (AUC).

* Matriz de confusión.

* Curva ROC.

La clase «plaga» (etiqueta 1) se definió como la clase positiva.

# 7. Resultados experimentales

La siguiente tabla resume el rendimiento de clasificación obtenido por las cuatro arquitecturas.

| Métrica | ResNet-18 | ResNet-34 | ResNet-50 | ResNet-101
| :--- | :---: | ---: | ---: | ---: |
| Exactitud (%) | 97.33 | **98.13** | 97.33 | 97.60 |
| Precisión (%) | 98.38 | 96.88 | 95.35 | **97.34** |
| Sensibilidad (%) | **99.47** | **99.47** | **99.47** | 97.86 |
| Especificidad (%) | 95.21 | 96.81 | 95.21 | **97.34** |
| Puntuación de F1 (%) | 97.38 | **98.15** | 97.38 | 97.60 |
| Área bajo la curva AUC (%) | 0.9939 | **0.9964** | 0.9944 | 0.9957 |

**Principales hallazgos**

ResNet-34 logró el mejor rendimiento de clasificación general, obteniendo:

98,13 % de exactitud.

99,47 % de sensibilidad.

98,15 % de puntuación F1.

0,9964 de AUC.

ResNet-101 alcanzó la mayor precisión y especificidad, llegando en ambos casos al 97,34 %.

Estos resultados indican que aumentar la profundidad de la red no conlleva necesariamente un mejor rendimiento en la clasificación para esta tarea de clasificación binaria de daños en el maíz.

ResNet-34 ofreció el equilibrio general más favorable entre el rendimiento de la clasificación y la complejidad computacional.








