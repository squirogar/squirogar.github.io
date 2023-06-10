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

En este dataset tenemos las input features $$x$$ y las true labels $$y$$.

Pero en unsupervised learning nuestro dataset no tiene estas respuestas correctas o true labels $$y$$, sino que sólo está compuesto por las input features $$x$$.
$$
\{x^{(1)}, x^{(2)}, x^{(3)}, ..., x^{(m)}
\}
$$

Como no tenemos las output targets $$y$$, no somos capaces de decirle al algoritmo cuál es la respuesta correcta $$y$$ que queremos predecir. En vez de eso, vamos a preguntarle al algoritmo que encuentre alguna estructura interesante sobre esta data, por ejemplo, que la agrupe en clusters, así quizas podemos obtener conocimiento que a simple vista sea difícil de adquirir.

# K-means: el algoritmo de clustering más utilizado
Para ejecutar K-means necesitamos un dataset sin labels $$y$$.

## I. Procedimiento
El algoritmo consta de 3 pasos. El primero se realiza una sola vez, mientras que los 2 siguientes se ejecutan varias veces. K-means es un algoritmo iterativo, por lo que dependiendo de la cantidad de iteraciones que le demos podremos obtener mejores resultados.

1. Lo primero que hace K-means es tomar suposiciones aleatorias de dónde podrían estar los centroids de los clusters que queremos encontrar.
    - Podemos decidir cuántos clusters vamos a encontrar. Por ejemplo, 2.
    - Si elegimos 2, entonces K-means elegirá aleatoriamente 2 puntos donde podrían estar los centroids de estos dos clusters. Como son suposiciones aleatorias iniciales no son particulamente buenas.
    - Un **centroid** es el centro de un cluster. Y un **cluster** es un grupo de puntos de datos relacionados.

2. K-means repetidamente hará estos dos pasos:

    2.1 **Asignar puntos de datos a los centroids:** K-means recorrerá cada dato en nuestro dataset y le asignará el centroid más cercano, ya sea el **centroid1** o al **centroid2** (recordar que para este ejemplo elegimos 2 clusters).

    2.2 **Mover los centroids:** K-means mirará todos los puntos de datos asignados al **centroid1** y tomará un promedio de ellos. Luego, moverá el **centroid1** a la ubicación del promedio de estos puntos de datos.Para el **centroid2** se hace exactamente lo mismo: se toman todos los puntos de datos asignados a este centroid y se calcula el promedio, para luego mover el **centroid2** a esa ubicación.

3. K-means recorrerá todos los datos de nuestro dataset de nuevo y repetirá el **paso 2** completo. En otras palabras, asociaremos cada punto de dato al centroid más cercano (**paso 2.1**). Algunos datos puede que sean asignados a diferentes centroids con el pasar de las iteraciones, esto es normal, ya que queremos determinar correctamente cuáles son sus centroids más cercanos. Después de esto, recalcularemos los centroids (o los moveremos, que es lo mismo) (**paso 2.2**). Hacemos esto hasta que no hayan más cambios en la asignación de los datos a centroids o que no hayan cambios al mover los centroids. En ese momento, K-means habrá **convergido**. Así, se habrán formado 2 clusters conteniendo cada uno data points similares.


## II. Definición formal de K-means

1. Aleatoriamente inicializar $$K$$ centroids $$\mu_1, \mu_2, ..., \mu_K$$:
    - Elegir aleatoriamente una ubicación para los centroids
    - $$\mu_1, \mu_2, ..., \mu_K$$ son vectores que tienen la misma dimensión que el dataset de ejemplos de entrenamiento $$x^{(1)}, x^{(2)}, ..., x^{(m)}$$. Todos los centroids son listas de $$n$$ números o vectores de $$n$$-dimensiones, donde $$n$$ es el número de features por cada uno de los ejemplos de entrenamiento. Por ejemplo, si $$n=2$$, tenemos 2 features $$x_1$$ y $$x_2$$, entonces $$\mu_1$$ y $$\mu_2$$ serán vectores con 2 números en ellos.

2. K-means repetidamente llevará a cabo los dos pasos descritos en la sección **I.**:

    2.1 Asignar cada uno de los puntos de datos $$x^{(i)}$$ al centroid $$\mu$$ más cercano.

    2.2 Mover los centroids de los clusters. Vamos a actualizar la ubicación de los centroids para ser el promedio o media de los datos asignados a cada cluster. Concretamente, veremos todos los puntos de datos asignados al centroid $$\mu$$ y calcularemos el promedio. El promedio se calcula para cada dimensión o feature, entonces: veremos el valor de cada punto en el eje x (feature $$x_1$$) y lo promediamos; luego, veremos el valor de cada punto en el eje y (feature $$x_2$$) y lo promediamos. Con esto, tenemos la media de los puntos que pertenecen al cluster $$\mu$$ y que será la nueva ubicación de este último. En python esto sería:
    ```
		a = np.array([x^(1), x^(5), x^(6), x^(10)])
		mu_1 = np.sum(a) / 4
		# x^(m) es un vector de tam n, con n=num. features
		# mu_1 es un vector de tam n, con n=num. features
        # numpy utiliza vectorización, por lo que las operaciones se aplican al vector completo
    ```

## III. Pseudocódigo

En pseudocódigo el algoritmo es:

```
Aleatoriamente inicializar K clusters mu_1, mu_2, ..., mu_K

repeat {
	# asignar cada data point al centroid más cercano
	for i = 1 to m:
		c^(i) := índice (de 1 a K) del centroids más cercano a x^(i).
		# esto es igual a: min_k ||x^(i) - mu_k||^2
		# ver nota

	# mver los centroids
	for k = 1 to K:
		mu_k := media de los data points asignados al cluster k
}
```

Nota:
```
c^(i) := índice (de 1 a K) del centroids más cercano a x^(i).
```
Esto es lo mismo que: 
$$
min_k ||x^{(i)}-\mu_k||^2_2
$$

Explicación:
- $$x^{(i)}$$ es el i-ésimo ejemplo de entrenamiento en el dataset
- matemáticamente esto es calcular la distancia entre $$x^{(i)}$$ y $$\mu_k$$. 
- El cálculo de la distancia entre 2 puntos es:
$$
		||x^{(i)} - \mu_k||_2
$$
A esto se le llama norma $$L2$$.
- Queremos encontrar el centroid $$k$$ que minimiza esta distancia al cuadrado.

### 1. Norma $$L2$$
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

La $$L2$$ norm está dada por:

$$ ||x||_2=(\sum_{i=1}^{n} {x}_i^2)^{1/2}=\sqrt{\sum_{i=1}^{n} {x}_i^2} $$

Esta es la norma euclidiana que se utiliza para calcular la distancia entre 2 puntos.

En python podemos calcular la norma $$L2$$ así:
```
numpy.linalg.norm(x)
```

Sin embargo, en machine learning se utiliza la norma $$L2$$ al cuadrado: $$||x||^2_2$$, ¿por qué?:
- Porque se deshace de la raíz cuadrada haciéndola más fácil para operar.
- y terminamos con una simple suma de cada elemento del vector al cuadrado. 
- Es ampliamente usada en machine learning porque puede ser calculada con la operación de vector $$x^\text{T}x$$, operación que se puede hacer muy rápidamente mediante la vectorización. Así, tenemos mejor rendimiento debido a la optimización (*).
- El centroid con la distancia al cuadrado más pequeña es el mismo que el centroid con la distancia más pequeña sin elevar al cuadrado.

$$ ||x||^2_2=\sum_{i=1}^{n} {x}_i^2= (x_1)^2+(x_2)^2+...+(x_n)^2$$

```
# l2-norm squared
np.linalg.norm(x)**2
```

(*): 
```
# x^Tx es lo mismo que la l2-norm
x^T = [1,
		2,
		3]
x = [1, 2, 3]
x^T dot product x = (1*1 + 2*2 + 3*3) = 14
```

## III. Función de costo para K-means

K-means también optimiza una cost function específica, aunque el algoritmo de optimización que utiliza para optimizar esa función no es gradient descent, en realidad, es el algoritmo que ya vimos anteriormente: el mismo K-means.

### 1. Notación
- $$c^{(i)}$$= índice del cluster $$1, 2, ..., k$$ al que se asigna actualmente el ejemplo $$x^{(i)}$$
- $$\mu_k$$ = centroid del cluster $$k$$
- $$\mu_{c^{(i)}}$$= centroid del cluster al que se ha asignado el ejemplo $x^{(i)}$. 
    - Por ejemplo: training example 10: $$x^{(10)}$$. Cuál es la ubicación del centroid al que el décimo training example ha sido asignado? Buscaremos $$c^{(10)}$$, entonces $$\mu_c^{(10)}$$ es la ubicación del centroid del cluster al que se ha asignado $$x^{(10)}$$.

### 2. Cost function
K-means intenta minimizar la siguiente cost function:
$$J(c^{(1)}, ..., c^{(m)}, \mu_1, ..., \mu_k) = \frac{1}{m} \cdot \sum_{i=1}^{m}(||x^{(i)} - \mu_{c^{(i)}}||^2)$$

- El nombre de esta cost function es: **Distortion function**.
- $$J$$ es función de $$c^{(1)},...,c^{(m)}$$ y $$\mu_1,...,\mu_k$$.
- $$\frac{1}{m} \cdot \sum^{m}_{i=1}(||x^{(i)} - \mu_{c^{(i)}}||^2)$$ es el promedio de la distancia al cuadrado entre cada training example $$x^{(i)}$$ y la ubicación del centroid $$\mu_{c^{(i)}}$$ al que se le asignó el training example $$x^{(i)}$$. Por ejemplo, para el 10mo ejemplo la distancia (al cuadrado) sería: $$(x^{(10)} - \mu_{c^{(10)}})^2$$. 
- K-means con esto intenta encontrar asignaciones de puntos a centroids y ubicaciones de centroids que minimizan la distancia al cuadrado.

### 3. Explicación de la cost function
Una vez asignados los puntos a los centroids, medimos las distancias entre dichos puntos y su repectivo centroid, para después calcular el cuadrado de todas esas distancias y obtener el promedio que finalmente será la cost function $$J$$.

En cada iteración, actualizaremos las asignaciones a clusters $$c^{(1)},...,c^{(m)}$$; o actualizaremos las posiciones de los centroids $$\mu_1,...,\mu_k$$ para seguir reduciendo la función de costo $$J$$.

### 4. ¿Por qué K-means intenta minimizar la función de costo $$J$$?

Veamos el algoritmo K-means:
```
#step 0: Randomly initialize K cluster centroids mu_1, mu_2, ..., mu_K

repeat {
	# step 1: assign points to cluster centroids
	for i = 1 to m
		c^(i) := index of cluster centroid closest to x^(i)

	# step 2: move cluster centroids
	for k = 1 to K
		mu_k := average of points in cluster k
}
```
K-means intenta minimizar la cost function $$J$$ de la siguiente forma:

* El **paso 1** busca actualizar $$c^{(1)},...,c^{(m)}$$ para minimizar la función de costo $$J$$ tanto como sea posible mientras mantiene $$\mu_1,...,\mu_k$$ fijos.
	- Esto es porque $$c^{(i)}$$ es el centroid más cercano a $$x^{(i)}$$, lo que hace que la distancia sea lo más pequeña posible.
* El **paso 2** busca actualizar $$\mu_1,...,\mu_k$$ y dejar $$c^{(1)},...,c^{(m)}$$ fijos para minimizar la cost function $$J$$ tanto como sea posible.
    - Hacer que $$\mu_k$$ sea la media de los puntos asignados minimiza la función de costo, ya que minimiza la distancia al cuadrado entre los puntos y el centroid. Por ejemplo: 
        - distancia *punto1* a *centroid1* es $$1$$
        - distancia *punto2* a *centroid1* es $$9$$
        - average: $$J = (1^2 + 9^2) / 2 = 41$$
        - Pero, si movemos el centroid donde es el promedio de los puntos: $$(1+11) / 2 = 6$$, entonces si calculamos $$J$$:
        - distancia *punto1* a *centroid1* es $$5$$
        - distancia *punto2* a *centroid1* es $$5$$
        - average: $$J = (5^2 + 5^2) / 2 = 25$$ <- es más pequeña $$J$$!


