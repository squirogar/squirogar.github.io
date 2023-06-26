---
layout: post
title: Tipos de datasets
date: 2023-06-25 20:04
description: Tipos de datasets training set, validation set, test set.
categories: datos
giscus_comments: true
related_posts: true
---

# Diferencia entre los distintos tipos de datasets
Existen 3 tipos de datasets:
1. Training set
2. Test set
3. Validation set

## Training set
**Se usa para entrenar el modelo.**

Un conjunto de ejemplos usados durante el aprendizaje (entrenamiento) para ajustar los parámetros del modelo de tal forma de encontrar los óptimos.

## Validation set
**Un conjunto de ejemplos utilizados para sintonizar los hiperparámetros de un modelo (tunning hyperparameters). El validation set nos permite comparar configuraciones de modelos y seleccionar las mejores.**

Tenemos diferentes modelos con diferentes configuraciones de hiperparámetros. Utilizamos el validation set para seleccionar el de mejor rendimiento y seguir entrenándolo.

Es un conjunto de datos utilizado para proporcionar una evaluación imparcial del ajuste de un modelo en el training set mientras se sintonizan los hiperparámetros del modelo. A medida que vamos configurando el modelo de acuerdo al validation set, se va volviendo más sesgada la evaluación, ya que vamos incorporando información al modelo.

Ejemplos de uso de validation set:
- Establecer el número de hidden units en una red neuronal. 
- Elegir entre 2 o más redes neuronales
- Selección de features.

## Test set
**Se usa para evaluar el modelo final (capacidad de generalización).**

Evaluar la capacidad del modelo final con el training set resultaría en una puntuación sesgada. Es por esto, que la evaluación del modelo final se realiza con un dataset apartado de todo. Este dataset es el test set.

Se le llama *peeking* a utilizar el test set para escoger una hipótesis (modelo) y para evaluarla. La forma de evitar esto es *no* acceder al test set hasta que hayamos terminado completamente con el training y emplearlo para obtener una evaluación independiente de la hipótesis final.
  - Si no nos gustan los resultados, obtenemos una nueva hipótesis y la probamos con un nuevo test set (Rusell y Norvig, 2009).

## Pseudocódigo de los distintos datasets

```
### Pseudocódigo ###

# split data
train, validation, test = split(data)

# tune model hyperparameters
parameters = ...
for params in parameters:
    model = fit(train, params)
    skill = evaluate(model, validation)

# evaluate final model for comparison with other final models
model = fit(train)
skill = evaluate(model, test)
```

## Alternativa al validation dataset: k-fold cross-validation
k-fold cross-validation se emplea para sintonizar los hiperparámetros del modelo en lugar de un validation set.

Según el artículo de donde se extrajo esta información, se recomienda un 10-fold cross-validation en general.
> *Reference to a “validation dataset” disappears if the practitioner is choosing to tune model hyperparameters using k-fold cross-validation with the training dataset.*

Ver más sobre k-fold cross validation [aquí](2023-06-25-k-fold.md)


[Fuente 1](https://machinelearningmastery.com/difference-test-validation-datasets)

[Fuente 2](https://stats.stackexchange.com/questions/19048/what-is-the-difference-between-test-set-and-validation-set)



