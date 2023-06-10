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

    2.1 **Asignar puntos de datos a los centroids:** K-means recorrerá cada dato en nuestro dataset y le asignará el centroid más cercano, ya sea el **centroid1** o al **centroid2** (recordar que para este ejemplo elegimos 2 clusters).

    2.2 **Mover los centroids:** K-means mirará todos los puntos de datos asignados al **centroid1** y tomará un promedio de ellos. Luego, moverá el **centroid1** a la ubicación del promedio de estos puntos de datos.Para el **centroid2** se hace exactamente lo mismo: se toman todos los puntos de datos asignados a este centroid y se calcula el promedio, para luego mover el **centroid2** a esa ubicación.

3. K-means recorrerá todos los datos de nuestro dataset de nuevo y repetirá el **paso 2** completo. En otras palabras, asociaremos cada punto de dato al centroid más cercano (**paso 2.1**). Algunos datos puede que sean asignados a diferentes centroids con el pasar de las iteraciones, esto es normal, ya que queremos determinar correctamente cuáles son sus centroids más cercanos. Después de esto, recalcularemos los centroids (o los moveremos que es lo mismo) (**paso 2.2**). Hacemos esto hasta que no hayan más cambios en la asignación de los datos a centroids o que no haya cambio al mover los centroids. En ese momento, K-means habrá **convergido**. Así, se habrán formado 2 clusters conteniendo cada uno data points similares.


## II. Definición formal de K-means

1. Aleatoriamente inicializar $$K$$ centroids $$\mu_1$$,

Randomly initialize $K$ cluster centroids $\mu_1, \mu_2, ..., \mu_k$.
  - randomly choose a location for the centroids
  - $\mu_1, \mu_2, ..., \mu_k$ are vectors which have the same dimension as your training examples $x^{(1)}, x^{(2)}, ..., x^{(m)}$. All of the centroids are lists of $n$ numbers or $n$-dimension vectors, where $n$ is the number of features for each of the training examples. For example, if $n=2$, we have 2 features $x_1$ and $x_2$, then $\mu_1$ and $\mu_2$ will be vectors with 2 numbers in them.

2. K-means will repeatedly carry out the two steps that we saw previously:

	2.1. assign data points to clusters centroids, meaning color each of the points according the color of the centroid. 

  2.2. Move the cluster centroids. We are going to update the location of the cluster centroids to be the average or the mean of the points assigned to that cluster. Concretely, we'll look at all data points assigned to the cluster centroid and look at their position on the horizontal axis (look at the value of the first feature $x_1$) and average that out, and compute the average value on the vertical axis as well. After computing those two averages, we find the mean and move the corresponding cluster centroid.
  - en python esto sería:
        ```
		    a = np.array([x^(1), x^(5), x^(6), x^(10)])
		    mu_1 = np.sum(a) / 4
		    # x^(m) es un vector de tam n, con n=num. features
		    # mu_1 es un vector de tam n, con n=num. features
        ```

En código el algoritmo es:

```
Randomly initialize K cluster centroids mu_1, mu_2, ..., mu_k

repeat {
	# assign points to cluster centorids
	for i = 1 to m:
		c^(i) := index (from 1 to K) of cluster centroid closest to x^(i).
			# this is equal to: min_k ||x^(i) - mu_k||^2
			# ver nota1, nota2 y nota3

	# move cluster centroids
	for k = 1 to K:
		mu_k := average (mean) of points assigned to cluster k
}
```

* Nota 1:
```
c^(i) := index (from 1 to K) of cluster centroid closest to x^(i)
```
Esto es lo mismo que: 
$$
min_k ||x^{(i)}-\mu_k||^2_2
$$

Explicación:
- $x^{(i)}$ es el i-ésimo training example
- matemáticamente esto es calcular la distancia entre $x^{(i)}$ y $\mu_k$. 
- El cálculo de la distancia entre 2 puntos es:
$$
		||x^{(i)} - \mu_k||_2
$$
A esto se le llama $L2$-norm.
- Queremos encontrar el centroid $k$ que minimiza esta distancia al cuadrado.

## 1. $L2$ norm
Dado el vector 
$$
\begin{align}
    x &= \begin{bmatrix}
           x_{1} \\\\
					 x_{2} \\\\
           \vdots \\\\
           x_{n}
         \end{bmatrix}
\end{align}
$$

La $L2$ norm está dada por:

$$ ||x||_2=(\sum_{i=1}^{n} {x}_i^2)^{1/2}=\sqrt{\sum_{i=1}^{n} {x}_i^2} $$

Esta es la norma euclidiana que se utiliza para calcular la distancia entre 2 puntos.

En python podemos calcular la norma $L2$ así:
```
numpy.linalg.norm(x)
```

Sin embargo, en machine learning se utiliza la norma $L2$ al cuadrado: $||x||^2_2$, ¿por qué?:
- porque se deshace de la raíz cuadrada haciéndola más fácil para operar.
- y terminamos con una simple suma de cada elemento del vector al cuadrado. 
- Es ampliamente usada en machine learning porque puede ser calculada con la operación de vector $x^\text{T}x$ (recordar las operaciones mediante vectorización). Así, tenemos mejor rendimiento debido a la optimización (*).
- El cluster centroid con la distancia al cuadrado más pequeña es el mismo que el cluster centroid con la distancia más pequeña (sin elevar al cuadrado).

$$ ||x||^2_2=\sum_{i=1}^{n} {x}_i^2= (x_1)^2+(x_2)^2+...+(x_n)^2$$

```
# l2-norm squared
np.linalg.norm(x)**2

```

* (*): 
```
# x^Tx es lo mismo que la l2-norm
x^T = [1,
		2,
		3]
x = [1, 2, 3]
x^T dot product x = (1*1 + 2*2 + 3*3) = 14
```



