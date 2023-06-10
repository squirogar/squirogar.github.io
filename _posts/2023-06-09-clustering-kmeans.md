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
		# esto es igual a: min_k ||x^(i) - mu_k||^2 (con 1 <= k <= K)
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
min_k ||x^{(i)}-\mu_k||^2
$$

Explicación:
- $$x^{(i)}$$ es el i-ésimo ejemplo de entrenamiento en el dataset
- matemáticamente esto es calcular la distancia entre $$x^{(i)}$$ y $$\mu_k$$. 
- El cálculo de la distancia entre 2 puntos es:
$$
		||x^{(i)} - \mu_k||
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

$$ ||x||=(\sum_{i=1}^{n} {x}_i^2)^{1/2}=\sqrt{\sum_{i=1}^{n} {x}_i^2} $$

Esta es la norma euclidiana que se utiliza para calcular la distancia entre 2 puntos.

En python podemos calcular la norma $$L2$$ así:
```
numpy.linalg.norm(x)
```

Sin embargo, en machine learning se utiliza la norma $$L2$$ al cuadrado: $$||x||^2$$, ¿por qué?:
- Porque se deshace de la raíz cuadrada haciéndola más fácil para operar.
- y terminamos con una simple suma de cada elemento del vector al cuadrado. 
- Es ampliamente usada en machine learning porque puede ser calculada con la operación de vector $$x^\text{T}x$$, operación que se puede hacer muy rápidamente mediante la vectorización. Así, tenemos mejor rendimiento debido a la optimización (*).
- El centroid con la distancia al cuadrado más pequeña es el mismo que el centroid con la distancia más pequeña sin elevar al cuadrado.

$$ ||x||^2=\sum_{i=1}^{n} {x}_i^2= (x_1)^2+(x_2)^2+...+(x_n)^2$$

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

## IV. Función de costo para K-means
K-means también optimiza una cost function específica, aunque el algoritmo de optimización que utiliza para optimizar esa función no es gradient descent, en realidad, es el algoritmo que ya vimos anteriormente: el mismo K-means.

### 1. Notación
- $$c^{(i)}$$= índice del cluster $$1, 2, ..., K$$ al que se asigna actualmente el ejemplo $$x^{(i)}$$
- $$\mu_k$$ = centroid del cluster $$k$$ (con $$1 <= k <= K$$)
- $$\mu_{c^{(i)}}$$= centroid del cluster al que se le ha asignado el ejemplo $$x^{(i)}$$. 
    - Por ejemplo: training example 10, $$x^{(10)}$$. ¿Cuál es la ubicación del centroid al que el décimo training example ha sido asignado? Buscaremos $$c^{(10)}$$, entonces $$\mu_{c^{(10)}}$$ es la ubicación del centroid del cluster al que se ha asignado $$x^{(10)}$$.

### 2. Cost function
K-means intenta minimizar la siguiente cost function:
$$J(c^{(1)}, ..., c^{(m)}, \mu_1, ..., \mu_K) = \frac{1}{m} \cdot \sum_{i=1}^{m}(||x^{(i)} - \mu_{c^{(i)}}||^2)$$

- El nombre de esta cost function es: **Distortion function**.
- $$J$$ es función de $$c^{(1)},...,c^{(m)}$$ y $$\mu_1,...,\mu_K$$.
- $$J$$ es el promedio de la distancia al cuadrado entre cada training example $$x^{(i)}$$ y la ubicación del centroid $$\mu_{c^{(i)}}$$ al que se le asignó el training example $$x^{(i)}$$. Por ejemplo, para el décimo ejemplo la distancia al cuadrado sería: $$(x^{(10)} - \mu_{c^{(10)}})^2$$. 
- K-means con esto intenta encontrar asignaciones de puntos a centroids y ubicaciones de centroids que minimizan la distancia al cuadrado.

### 3. Explicación de la cost function
Una vez asignados los puntos a los centroids, medimos las distancias entre dichos puntos y su repectivo centroid, para después calcular el cuadrado de todas esas distancias y obtener el promedio que finalmente será la cost function $$J$$.

En cada iteración, actualizaremos las asignaciones a clusters $$c^{(1)},...,c^{(m)}$$; o actualizaremos las posiciones de los centroids $$\mu_1,...,\mu_K$$ para seguir reduciendo la función de costo $$J$$.

### 4. ¿Por qué K-means intenta minimizar la función de costo $$J$$?
Veamos el algoritmo K-means:
```
#paso 0: aleatoriamente inicializar K centroids mu_1, mu_2, ..., mu_K

repeat {
	# paso 1: asignar puntos al centroid más cercano
	for i = 1 to m
		c^(i) := índice del centroid más cercano a x^(i)

	# paso 2: mover los centroids
	for k = 1 to K
		mu_k := media de los puntos del cluster k
}
```
K-means intenta minimizar la cost function $$J$$ de la siguiente forma:

* El **paso 1** busca actualizar $$c^{(1)},...,c^{(m)}$$ para minimizar la función de costo $$J$$ tanto como sea posible mientras mantiene $$\mu_1,...,\mu_K$$ fijos.
	- Esto es porque $$c^{(i)}$$ es el centroid más cercano a $$x^{(i)}$$, lo que hace que la distancia sea lo más pequeña posible.
* El **paso 2** busca actualizar $$\mu_1,...,\mu_K$$ y dejar $$c^{(1)},...,c^{(m)}$$ fijos para minimizar la cost function $$J$$ tanto como sea posible.
    - Hacer que $$\mu_k$$ sea la media de los puntos asignados minimiza la función de costo, ya que minimiza la distancia al cuadrado entre los puntos y el centroid. Por ejemplo: 
        - distancia *punto1* a *centroid1* es $$1$$
        - distancia *punto2* a *centroid1* es $$9$$
        - average: $$J = (1^2 + 9^2) / 2 = 41$$
        - Pero, si movemos el centroid donde es el promedio de los puntos: $$(1+11) / 2 = 6$$, entonces si calculamos $$J$$:
        - distancia *punto1* a *centroid1* es $$5$$
        - distancia *punto2* a *centroid1* es $$5$$
        - average: $$J = (5^2 + 5^2) / 2 = 25$$ <- es más pequeña $$J$$!


## V. Convergencia de K-means
El hecho de que K-means optimice una cost function $$J$$ significa que la convergencia está garantizada, es decir, en cada iteracion la distortion cost function debería bajar o mantenerse igual, pero **NUNCA SUBIR!**, si ese es el caso, hay un bug en el código, ya que en cada paso de K-means se está estableciendo el valor $$c^{(i)}$$ y $$\mu_k$$ para intentar reducir la cost function.

También, si la cost function para de bajar, esto nos da una forma para probar si K-means ha convergido. Una vez que haya una sola iteración que se mantiene igual, eso usualmente significa que K-means ha convergido y deberíamos parar el algoritmo. O en algunos casos, ejecutaremos K-means por un largo tiempo, y la cost function va bajando muy, muy lentamente, esto es parecido al gradient descent, donde quizás ejecutarlo por más tiempo puede mejorar, pero si la tasa a la que la cost function está bajando es muy, muy lenta, también podemos decir que es suficiente y está muy cerca de la convergencia.


## VI. Inicializando clusters de K-means
El uso de múltiples inicializaciones aleatorias diferentes de los centroids dará como resultado la búsqueda de un mejor conjunto de clusters.

El primer paso es elegir ubicaciones aleatorias como suposiciones iniciales para los centroids $$\mu_1,...,\mu_K$$.

```
#paso 0: aleatoriamente inicializar K centroids mu_1, mu_2, ..., mu_K

repeat {
	# paso 1: asignar puntos al centroid más cercano
	# paso 2: mover los centroids
}
```
¿Cómo implementamos el **paso 0**?

### 1. A tener en cuenta
Siempre elegir un valor de $$K$$ menor a $$m$$:

$$
K < m
$$

$$K$$ es el número de clusters y $$m$$ es el número de training examples.

No tiene sentido tener más centroids que ejemplos $$m$$ porque no habrán suficientes training examples para que cada cluster tenga al menos un ejemplo dentro.

### 2. Inicialización de clusters
La forma de inicializarlos más común es aleatoriamente escoger $$K$$ datos de nuestro dataset de entrenamiento y establecer $$\mu_1, ..., \mu_K$$ con un valor igual a estos $$K$$ datos. Por ejemplo, si $$K=2$$, entonces elegimos 2 training examples y ubicamos los 2 centroids, $$\mu_1$$ y $$\mu_2$$ en el mismo lugar que éstos.

### 3. A considerar en la inicialización de clusters
La forma anterior de inicializar los cluster centroids tiene algunos puntos a considerar:
- Con este método hay una posibilidad de terminar con 2 (o más) clusters inicializados muy cerca. Y dependiendo de cómo elijamos las posiciones iniciales aleatorias de los centroids, K-means terminará escogiendo diferentes set de clusters para el dataset que tenemos. 

- Si queremos encontrar 3 clusters y justo tenemos 3 nubes de puntos separadas, el resultado óptimo sería un cluster que agupe a cada nube de puntos. Sin embargo, podemos tener una inicialización diferente que tenga 2 centroids a muy poca distancia (ambos están en la misma nube de puntos), por lo que estos 2 clusters van a tener muy pocos puntos mientras que el tercero tendrá muchos puntos dispersos (las otras 2 nubes de puntos), generando así un resultado subóptimo o mínimo local. Otra situación que puede suceder al inicializar aleatoriamente los centroids es, que un cluster se quede con muchos puntos, otro se quede con unos pocos y finalmente el último se quede sin puntos, generando que K-means se quede estancado en un resultado subóptimo.

### 4. Solución al resultado subóptimo de k-means 
Si le damos muchas oportunidades o ejecuciones al algoritmo K-means para encontrar el mejor óptimo local se podrá solucionar esto. Así, podemos intentar múltiples inicializaciones aleatorias para tener una mejor chance de encontrar mejores clusters.

### 5. ¿Y si hay un cluster que no tenga data points asignados?
En el **paso 2**, $$\mu_k$$ intentaría calcular el promedio de $$0$$ training examples, lo cual no está definido.

Hay 2 soluciones:
* a. La más común: eliminar ese cluster, por lo que terminaremos con $$K-1$$ clusters
* b. Si realmente necesitamos los $$K$$ clusters, entonces aleatoriamente reinicializar ese cluster y esperar a que se le asignen data points la próxima vez.

## VII. Ejecución de K-means múltiples veces
Si ejecutamos varias veces a K-means, debemos elegir uno de entre varios clustering. Una forma de elegir cuál clustering es el mejor, o sea, qué tirada nos dió mejores resultados, es calcular la cost function $$J$$ para **todas** las soluciones, para todas las alternativas de clustering encontradas en las ejecuciones de K-means, y elegir una de éstas de acuerdo a cuál da el menor valor para la cost function $$J$$.

Nos damos cuenta que en la mejor solución de clustering, los data points en cada cluster tienen una distancia al cuadrado relativamente pequeña con respecto a su centroid, por lo tanto, la cost function $$J$$ será relativamente pequeña para esta solución. En cambio, en las demás soluciones en algunos clusters, habrá una distancia al cuadrado más grande entre los puntos y el centroid correspondiente, por lo cual la función de costo será más grande.

### 1. Formulación de K-means inicializado aleatoriamente múltiples veces
```
for i = 1 to 100 { #100 inicializaciones aleatorias, o sea, ejecutamos 100 veces K-means
	Aleatoriamente inicializar los K centroids
		# Elegir K ejemplos de entrenamiento aleatorios e inicializar los K centroids en esas ubicaciones.

	Ejecutar K-means hasta la convergencia:
        1. Obtener c^(1), ..., c^(m), mu_1, mu_2, ..., mu_K

	    2. Calcular cost function (distortion) J(c^(1), ..., c^(m), mu_1, mu_2, ..., mu_K)
}
```

>Elegir el set de clusters que dan la cost function $$J$$ más baja.

Después de ejecutar este bucle 100 veces (el número es arbitrario), escogeremos el set de clusters que nos dan el costo $$J$$ más bajo. Si hacemos esto, a menudo obtendremos un mejor set de clusters que si ejecutaramos K-means una sola vez.

#### Sobre el número de veces que tenemos que ejecutar K-means
Se recomienda un número entre 50 a 1000 (en el ejemplo pusimos 100). Si ejecutamos K-means más de 1000 veces tiende a ser computacionalmente caro y obtendremos muy pocas mejoras si lo ejecutamos más de esas veces.


## VIII. Cómo elegir el número de clusters K
Para muchos problemas de clustering, el valor correcto de $$K$$ es ambigüo. Hay muchas aplicaciones donde los datos no dan un indicador claro de cuántos clusters hay en ellos.

### 1. Método 1: Elbow method
Técnica para encontrar automáticamente el número de clusters para usar en una aplicación.

Ejecutamos K-means con un variedad de valores de $$K$$ y graficamos la función de costo o la **distortion function** $$J$$ (eje $$y$$) como una función del número de clusters (eje $$x$$).

Lo que encontramos es que cuando tienes muy pocos clusters, la distorion function o la cost function $$J$$ será alta y a medida de que aumentes el número de clusters, bajará. Veremos que la cost function va disminuyendo rápidamente hasta que lleguemos a un punto determinado, luego de ese punto, la función de costo disminuirá más lentamente (la curva se va aplanando). Básicamente se elige $$K$$ en el punto donde la curva se dobla (se forma como un codo), o sea, donde termina el descenso rápido y empieza a descender más lentamente.

#### El problema del elbow method
El problema es que en muchas aplicaciones la cost function tiene un descenso suavizado, por lo que paulatinamente va descendiendo y no se forma un codo notorio para elgir un valor de $$K$$.

### 2. Método 2: elegir el K que más hace disminuir J
Elegir $$K$$ que minimiza la función de costo $$J$$ no sirve porque implicaría elegir el valor de $$K$$ más grande posible, ya que tener más clusters siempre reducirá la cost function.

### 3. Método 3 (recomendado): decidir qué tiene sentido para la aplicación
A menudo, ejecutamos K-means para obtener clusters y usarlos para algún propósito posterior. Lo que es recomendable, es evaluar K-means en función de qué tan bien se desempeñe para ese propósito posterior. Por ejemplo, tenemos una nube de puntos dispersos y los ejes son: *altura* y *peso*. Dado estas medidas de personas, queremos agruparlas en clusters para determinar cómo medir las poleras *S (small)*, *M (medium)* y *L (large)*. Elegimos $$K=3$$. Sin embargo, podemos ejecutar K-means con 5 clusters $$(K=5)$$ y así podemos medir las poleras de acuerdo a *XS (extra small), S, M, L y XL (extra large)*. Entonces, Ambas opciones son completamente válidas, por lo que la elección dependerá de lo que tenga sentido para el negocio de las poleras.

Hay un trade-off entre qué tan bien las poleras entran o se ajustan a las personas, dependiendo si tenemos 3 tallas o 5 tallas, pero habrá un costo extra asociado a la manufactura y al envío de las poleras. ¿Qué hacemos? Ejecutar K-means con $$K=3$$ y $$K=5$$ y con estas 2 soluciones elegir, basandonos en el trade-off entre fabricar las poleras con más tamaños implicando un mejor ajuste, y el costo extra de hacer más poleras. 


## IX. ¿Por qué normalizar las features en K-means?
Es siempre mejor normalizar las features cuando usamos K-means porque K-means es basado en distancia, lo que significa que es sensible a la diferencia de las escalas de las features.

Por ejemplo, tenemos 2 features:
1. Peso (lbs)
2. Altura (feet)

Utilizamos éstas para predecir si una persona necesita una polera *S* o una *L*.

En nuestro training set tenemos 2 personas ya en los clusters:
1. Adam (175 lbs, 5.9 ft) en *L*.
2. Lucy (115 lbs, 5.2 ft) en *S*.

Tenemos una nueva persona: Alan (140 lbs, 6.1 ft).

Nuestro algoritmo de clustering lo pondrá en el cluster más cercano.
- Cálculo distancia en cluster 1: $$(175-140)^2 + (5.9-6.1)^2 = \sqrt{(1225 + 0.04)} = 35$$

- Cálculo distancia en cluster 2: $$(115-140)^2 + (5.2-6.1)^2 = \sqrt{(625 + 0.81)} = 25$$

Notar el gran impacto de la feature 1 (*Peso*) en el cálculo de la distancia. Además, notar el pequeñísimo impacto de la feature 2 (*altura*) en el cálculo de la distancia.

Esto impactará el rendimiento de todos los modelos basados en distancia, ya que otorgará mayor peso a las variables que tienen una mayor magnitud (*Peso* en este caso). Entonces, el algoritmo está sesgado hacia una variable con mayor magnitud. Para superar este problema, podemos reducir todas las variables a la misma escala. Si no escalamos las features, la variable *altura* no tiene mucho efecto y a Alan se le asignará en el grupo *S*, lo que no tiene sentido, ya que es muy alto.

### En resumen
El problema está en que en K-means, si las features están a escalas muy diferentes, por ejemplo, edad vs sueldo, entonces va a haber una feature que va a tener demasiado impacto sobre el cálculo de la distancia, mientras que la otra va a tener poco o casi nulo.

## X. Algunas Q&A
1. ¿Por qué con K-means resultan clusters tan diferentes?
- Tiene que ver con la data. Si nuestros clusters tienen un límite bien definido entre ellos siempre encontraremos los mismos centroids y solo dependerá del parámetro $$K$$. Si nuestra data no tiene un límite claro entre cada cluster (datos más dispersos) entonces inicializar los centroids aleatoriamente darán diferentes resultados dependiendo de la inicialización.
- Una sugerencia es usar el parámetro `random_state` cuando usemos la implementación de K-means de la librería `scikit-learn`, ya que si lo establecemos con un valor podemos garantizar que tendremos los mismos centroids cada vez que usemos el modelo (que lo entrenemos).
```
from sklearn.cluster import KMeans
kmeans = KMeans(n_clusters=3, random_state=42)
```

2. ¿Por qué hay clusters sin puntos asignados?
- A medida que aumenta $$K$$ (más centroids) existe más probabilidad de que un centroid no sea el más cercano de ningún punto, o sea, no se le asigne ningún data point. También existe el caso que si hay 2 centroids muy cerca y le asignamos correctamente de forma aleatoria sus ubicaciones, pero tenemos datos con ejemplos repetidos; al momento de asignar los centroids más cercanos a cada uno de los puntos, puede pasar que uno le "robe" el dato a otro, quedandose con dos datos asignados y el otro sin nada, esto pasa en el caso de aplicar K-means para comprimir imágenes, en donde dos pixeles pueden tener exactamente el mismo valor. 

