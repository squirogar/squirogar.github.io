---
layout: post
title: K-means clustering algorithm
date: 2023-06-09 21:41
description: Definición, análisis, implementación y consejos del algoritmo K-means
categories: algoritmos
giscus_comments: true
related_posts: true
---


K-means es un algoritmo de clustering. Antes de ver en qué consiste K-means, es bueno saber qué es un algoritmo de clustering.

# Clustering
Los algoritmos de clustering son un tipo de algoritmo de Unsupervised Learning (Aprendizaje no supervisado) y se encargan de agrupar los datos en *clusters* o grupos. Específicamente, el algoritmo mira los datos y automáticamente los agrupa, encontrando así la relación o similitud que hay entre ellos. Los puntos que pertenecen a un mismo cluster son más similares entre sí en comparación con puntos de otros clusters. La similitud entre puntos se basa en la distancia que haya entre ellos, por lo que mientras más juntos los puntos en un cluster, más similares son. 

# Diferencia con supervised learning
En supervised learning tenemos un dataset compuesto por:
$$
\{(x^{(1)},y^{(1)}), (x^{(2)},y^{(2)}), (x^{(3)},y^{(3)}), ..., (x^{(m)},y^{(m)})
\}
$$

En este dataset tenemos las input features $x$ y las true labels $y$.

Pero en unsupervised learning nuestro dataset no tiene estas respuestas correctas o true labels $y$, sino que sólo está compuesto por las input features $x$.
$$
\{x^{(1)}, x^{(2)}, x^{(3)}, ..., x^{(m)}
\}
$$

Como no tenemos las output targets $y$, no somos capaces de decirle al algoritmo cuál es la respuesta correcta $y$ que queremos predecir. En vez de eso, vamos a preguntarle al algoritmo que encuentre alguna estructura interesante sobre esta data, por ejemplo, que la agrupe en clusters, así quizas podemos obtener conocimiento que a simple vista sea difícil de adquirir.

# K-means: el algoritmo de clustering más utilizado
Para ejecutar K-means necesitamos un dataset sin labels $y$.

## I. Procedimiento
El algoritmo consta de 3 pasos. El primero se realiza una sola vez, mientras que los 2 siguientes se ejecutan varias veces. K-means es un algoritmo iterativo, por lo que dependiendo de la cantidad de iteraciones que le demos podremos obtener mejores resultados.

1. Lo primero que hace K-means es tomar suposiciones aleatorias de dónde podrían estar los centroids de los clusters que queremos encontrar.
    - Podemos decidir cuántos clusters vamos a encontrar. Por ejemplo, 2.
    - Si elegimos 2, entonces K-means elegirá aleatoriamente 2 puntos donde podrían estar los centroids de estos dos clusters. Como son suposiciones aleatorias iniciales no son particulamente buenas.
    - Un **centroid** es el centro de un cluster. Y un **cluster** es un grupo de puntos de datos relacionados.

2. K-means repetidamente hará estos dos pasos:
    2.1 Asignar puntos de datos a los centroids
    - K-means recorrerá cada dato en nuestro dataset y le asignará el centroid más cercano, ya sea el *centroid1* o al *centroid2* (recordar que para este ejemplo elegimos 2 clusters).
    2.2 Mover los centroids
    - K-means mirará todos los puntos de datos asignados al *centroid1* y tomará un promedio de ellos. Luego, moverá el *centroid1* a la ubicación del promedio de estos puntos de datos.
    - Para el *centroid2* se hace exactamente lo mismo: se toman todos los puntos de datos asignados a este centroid y se calcula el promedio, para luego mover el *centroid2* a esa ubicación.

3. K-means recorrerá todos los datos de nuestro dataset de nuevo y repetirá el *paso 2* completo. En otras palabras, asociaremos cada punto de dato al centroid más cercano (*paso 2.1*). Algunos datos puede que sean asignados a diferentes centroids con el pasar de las iteraciones, esto es normal, ya que queremos determinar correctamente cuáles son sus centroids más cercanos. Después de esto, recalcularemos los centroids (o los moveremos que es lo mismo) (*paso 2.2*). Hacemos esto hasta que no hayan más cambios en la asignación de los datos a centroids o que no haya cambio al mover los centroids. En ese momento, K-means habrá **convergido**. Así, se habrán formado 2 clusters conteniendo cada uno data points similares.



