---
layout: post
title: Loss y Accuracy en deep learning
date: 2023-07-09 19:56
description: Qué significa los indicadores de loss y accuracy en deep learning.
categories: deep-learning
giscus_comments: true
related_posts: true
---

# Precisión y loss en tensorflow
Accuracy es una métrica que sólo se puede aplicar a las tareas de clasificación. Describe exactamente qué porcentajes de los datos se clasifican correctamente. Por ejemplo, tenemos un dataset con perros y gatos; si de 100 se clasifican bien 95, tenemos un 95% de precisión.

La pérdida es qué tan seguro está el algoritmo en la clasificación, y es la suma de de la diferencia entre la probabilidad predicha de la clase real de la imagen y 1.

Tenemos un problema de clasificación de imágenes de un dataset balanceado. Este dataset tiene gatos y
perros. Además, tenemos dos clasificadores:
- clasificador 1: predice correctamente 80/100 casos
- clasificador 2: predice correctamente 95/100 casos

Aquí, el clasificador 2 obviamente tiene una precisión (accuracy) más alta.

Sin embargo, en las 80 imágenes que el clasificador 1 acertó, este clasificador estuvo extremadamente 
seguro (por ejemplo, cuando piensa que una imagen es un gato, está 100% seguro que es así), y en las 
20 que se equivocó no estuvo seguro para nada (por ejemplo, cuando dice que es un gato en vez de un 
perro con un 51% de seguridad). En comparación, el clasificador 2 está extremadamente seguro es sus 
5 respuestas incorrectas (por ejemplo, 100% seguro que la imagen de un gato es un perro), y no estuvo 
muy confiado sobre el 95% que acertó. En este caso, el clasificador 2 tendría peor loss.

[Fuente](https://datascience.stackexchange.com/questions/42599/what-is-the-relationship-between-the-accuracy-and-the-loss-in-deep-learning)