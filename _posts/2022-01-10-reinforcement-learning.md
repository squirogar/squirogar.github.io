---
layout: post
title: Reinforcement Learning
date: 2022-01-10 21:41
description: Definición y workflow del Aprendizaje por refuerzo
categories: algoritmos
giscus_comments: true
related_posts: false
---

*Material obtenido del e-book de matlab de Reinforcement Learning*

# Reinforcement Learning (RL)

Permite resolver problemas muy difíciles de control. Por ejemplo, un robot caminante. Un robot caminante es muy difícil de lograr con el enfoque de control, ya que necesitaríamos muchos loops de control, sensores, controladores de motor, etc.

En RL, trabajamos con un entorno dinámico, a diferencia del Supervised learning y el Unsupervised learning que son datasets estáticos.

RL consiste en un agente que se relaciona con un entorno y va tomando acciones que afectan a ese entorno. Dicho entorno cambia de estado tras cada acción que tome el agente y además le proporciona una recompensa al agente por cada acción que ejecute. Estas acciones o decisiones pueden ser buenas o malas, y dependiendo de ésto va a recibir un recompensa buena o mala respectivamente. Cabe destacar que **al agente nunca se le dice qué hacer, sino que él mismo debe descubrir qué acciones tomar, de tal forma que produzca la mayor recompensa a largo plazo**.

>Con RL, queremos encontrar la mejor secuencia de acciones que generarán la salida óptima, o sea, la que genera la mejor recompensa final. La idea es maximizar esta recompensa a largo plazo.

* Agente: el agente es un software que explora, interactúa y aprende del entorno

... Aprende ... ¿Cómo un agente *aprende* del entorno? El agente usa la información que obtiene del entorno (observaciones del estado del entorno y recompensa) para ajustar sus acciones a futuro. La recompensa es muy importante porque le va a decir al agente qué tan buena fue la acción que acaba de realizar.

Ejemplo de reinforcement learning:
1. agente: ser humano
2. entorno: ciudad en la que vive el agente
3. acción 1: el agente mira a ambos lados para cruzar la calle
4. Luego de la acción 1, el entorno cambia de estado y el agente obtiene una recompensa:
    - observaciones de estado: el agente se encuentra en la otra vereda de la calle de la ciudad
    - recompensa: no resultó herido al cruzar la calle.


## Funcionamiento de RL
El procedimiento es el siguiente:

1. Agente observa el estado actual del entorno
```
AGENTE <----observaciones--- ENTORNO
```

2. Decide cual acción tomar dependiendo del estado
```
AGENTE ----acciones----> ENTORNO
```

3. Entorno cambia de estado y produce una recompensa por la acción ejecutada
```
AGENTE <---- observaciones nuevo estado ---- ENTORNO
       <----  Recompensa -------------------
```

4. ¿Fueron buenas las acciones o malas?
- buenas: repetirlas
- malas: evitarlas
Repetir hasta terminar el aprendizaje.
```
AGENTE INTELIGENTE ----- acciones ----> ENTORNO
                    <---observaciones--
                    <----recompensas---
```





## Política
El agente internamente toma las observaciones del estado del entorno y las asigna a acciones realizando así un mapeo. En otras palabras, se puede entender este mapeo como una función que recibe entradas y genera una salida. A este mapeo se le llama **política**. La política es muy importante, ya que decide qué acción ejecutar dado un conjunto de observaciones de estado. Básicamente la política es el *cerebro* de nuestro agente, y le va a indicar qué hacer. La política se puede representar de varias formas, una forma muy útil es que si tenemos observaciones de estado complejas como por ejemplo, imágenes, entonces podemos usar una red neuronal para procesar dichos datos.

Ejemplo de política en un robot caminante:
1. observaciones: estado de cada articulación y los miles de pixeles de un sensor de cámara
2. política: toma las observaciones y genera comandos de acción que mueven al robot
3. recompensa: qué tan bien funcionaron los comandos: ¿Se cayó el robot?, ¿Se desvió del camino?, ¿Se está arrastrando? ¿Esta caminando erguido? Etc.

## Algoritmo de Reinforcement Learning
La política debe ajustarse, no puede ser estática, ya que el entorno es dinámico; para esto existen los **algoritmos de Reinforcement Learning**. Un algoritmo de RL hace óptima a la política, cambiandola en función de las acciones que se tomaron, las observaciones del estado del entorno y la cantidad de recompensa recolectada.

>Un agente de RL utiliza un algoritmo de RL para modificar su política a medida que interactúa con el entorno, de modo que eventualmente, dado cualquier estado, siempre tomará la mejor acción: la que producirá la mayor recompensa a largo plazo.

*-----imagen de como es rl----*

## Valor y recompensa
* Valor: recompensa total que un agente puede esperar recibir desde ese estado en adelante.
* Recompensa: beneficio inmediato de estar en un estado específico.

### ¿Por qué es importante el valor?
Evaluar el valor de un estado en vez de la recompensa inmediata ayuda al agente a elegir la acción que obtendrá la mayor recompensa a lo largo del tiempo, en vez de un beneficio a corto plazo.

Supongamos que tenemos la siguiente situación:
```
+1  agente  -1  -1  +10
```
¿Dónde debe ir el agente? Si va al estado de la izquierda obtendrá un beneficio instantáneo. Pero, si va a la derecha obtendrá un beneficio a futuro mayor de +8.

Ahora, tenemos la siguiente situación en la que sólo podemos dar 2 pasos:
```
                    agente
                      |
Valor             0   |     +4
Recompensa   -1  +1   0     -1    +5
Estado       s0  s1   s2    s3    s4
```
El valor para el estado s3 es +4 porque se espera recibir desde aquí en adelante +4 de recompensa total.

- Si se elige en base a la recompensa:
    ```
    1. agente va a s1 ---> recompensa_total = +1
    2. agente vuelve a s2 ---> recompensa_total = +1
    ```

- Si se elige en base a al valor estimado de un estado:
    ```
    1. Agente va a s3 ---> recompensa_total = -1
    2. Agente va a s4 ---> recompensa_total = +4
    ```

No obstante, elegir a corto plazo igual puede servir:
- Recompensa inmediata puede ser mejor que esperar por una futura.
- Predicción de recompensas puede fallar y esa recompensa alta puede que no esté cuando lleguemos a esos estados, o sea, existe mayor incertidumbre.
- La solución a esto es descontar recompensas mientras más lejos estén el futuro.

¿Y qué pasa cuando hay estados que no se conocen? Cabe la posibilidad de que existan estados que sean desconocidos para el agente, pero éstos pueden contener recompensas mayores a las de los estados que actualmente conocemos. Es aquí donde entran los dos enfoques que puede aplicar el agente.

## Enfoque muy codicioso: explotación del entorno
Recolectamos la mayor cantidad de recompensas que se conozcan, o sea, las más cercanas. Le damos más relevancia al beneficio inmediato que al futuro.

```
+1  agente  -1  ?  ?
```
El agente irá a la izquierda.

## Enfoque poco codicioso: exploración del entorno
Exploramos estados desconocidos del entorno con la esperanza de obtener mejores recompensas y por consiguiente, una mejor recompensa a largo plazo. Sin embargo, corremos el riesgo de recolectar peores recompensas por algún tiempo, o que incluso descubramos que estas recompensas no sean tan buenas como las que conocíamos.

```
+1  agente  -1  -1  -1   10
```
El agente irá a la derecha.

## Explotación vs Exploración
El algoritmo de RL explorará o explotará el espacio de estados, convirtiéndose esto en un problema de optimización

Si bien explorar para obtener una gran recompensa en el futuro puede ser muy tentador a elegir, puede que no sea tan buena opción, esto se debe a que es posible que:
1. La recompensa actual es mayor que la recompensa después
2. Las recompensas en el futuro son menos confiables, ya que podrían no estar allí cuando se alcancen, por lo que no hay que confiar 100% en la predicción de recompensa.

> RL descuenta las recompensas en una cantidad mayor cuanto más lejos estén en el futuro.

> El algoritmo de RL establece un equilibrio entre exploración y explotación. Este trade-off se da mientras el agente interactúa con el entorno.


## Control de sistamas y RL
Tenemos un sistema o proceso industrial que queremos controlar. Controlamos las entradas del sistema (acciones) para intentar generar las salidas deseadas (comportamientos).

```
acción -> sistema -> comportamiento deseado
```

Controlamos las entradas mediante un software controlador.

Utilizamos las observaciones del estado (retroalimentación) para mejorar el rendimiento y corregir errores

*----imagen de lazo cerrado-----*

Si el problema es muy complejo, utilizamos loops de control andidados, sensores y controladores, lo que es muy difícil de implementar y modificar. En control, el diseñador es el que se preocupa de modificar explícitamente el sistema para que tenga el comportamiento idóneo. No obstante, podemos utilizar RL para resolver los problemas de control. En RL, el software por sí mismo intenta aprender el comportamiento óptimo a lo largo del tiempo. La idea es tomar las observaciones del estado del sistema y el agente generará la mejor acción para controlar el sistema. *El agente sería el controlador*.

```
observaciones del estado -> agente RL (controlador) -> acciones
```

*---imagen rl + control----*

Estrictamente hablando, el controlador sería la política (recordar que la política es el cerebro del agente), ya que va a mapear las observaciones del estado del sistema a comandos de acción que permitan generar el comportamiento que queremos que este sistema tenga. Nótese que *el entorno sería el sistema a controlar*. 

> El controlador influye sobre el sistema cambiando su estado



## Uso de RL en control
Para utilizar RL en control tenemos que:
1. Establecer la estructura del controlador: definir la política
2. Definir ¿qué es un resultado exitoso? Y establecer recompensas cuando se consiga: Establecer una función de recompensa que el indique al algoritmo si está mejorando o no.
3. Aplicar un algoritmo de aprendizaje eficiente que sepa cómo ajustar los parámetros para que el proceso converja en un tiempo razonable: Elegir un algoritmo de RL

## Workflow de RL
El flujo de trabajo de RL consiste en:
1. Establecer un entorno: qué debe existir en ese entorno. Además debemos decidir: ¿Durante el entrenamiento probamos con un entorno real (hardware real) o simulado (uso de modelos matemáticos del sistema)?
2. definir la señal de recompensa: qué debe hacer el agente, cómo debe llegar al objetivo, diseñar la función de recompensa
3. Establecer la política: cómo representamos la política: estructuración de los parámetros y la lógica de la toma de decisiones del agente. ¿Cuál es la información que recibe el agente? ¿Cuál debe ser la salida que genera el agente? ¿De qué tipo son estas entradas y salidas? ¿Voy a representar la política con una red neuronal? Etc.
4. Algoritmo de RL: mediante el algoritmo obtendremos la política óptima. Se debe elegir el mejor de acuerdo a nuestro caso. Existen algoritmo de RL que dependen de que si las entradas y salidas son continuas o discretas.
5. Deploy/verificación: se implementa la política en un agente y se verfican los resultados.

### 1. Entorno
En RL, el entorno es de dónde el agente aprende. El entorno es todo lo que está **afuera** del agente. Esto llevado a control sería todo lo que no es el controlador: lazo de retroalimentación, sistema, señal de referencia, etc.

Un agente realiza **acciones** que influyen sobre el entorno. El entorno cambia de estado, al hacerlo, informa al agente entregándole **observaciones de estado** y una **recompensa**.


Existen dos tipos de RL:
### 1.1. RL sin modelo 
El agente no necesita saber nada sobre el entorno. Se coloca un agente RL en cualquier sistema y asumiendo que a la política se le da acceso a las observaciones, acciones y estados internos, el agente obtendrá la mayor recompensa por sí solo.

El agente debe explorar todo el espacio de estados para calcular la mayor recompensa, lo que conlleva más tiempo en aprender la política óptima.

El RL sin modelo se utiliza cuando es difícil generar un modelo. Ej: controlar un auto, robot caminante.


### 1.2. RL basado en modelo: 
Sin ninguna comprensión del entorno el agente deberá explorar todas las áreas del espacio de estados, para cumplir su función de valor, por lo que gastará tiempo explorando áreas de bajas recompensas mientras está aprendiendo. Al proporcionar un modelo del entorno, proporcionamos al agente información de zonas del espacio de estado que no valen la pena explorar. Entonces, el modelo puede complementar el proceso de aprendizaje evitando áreas que sabe que son malas y el agente aprenderá mejor.

Este modelo sirve de guía para el agente, complementando el aprendizaje de este último. El tiempo es menor en aprender la política óptima.


### 1.3. ¿Entorno real (físico) o simulado?
Cuando puede ocurrir daño en el hardware o a las personas se ocupa un entorno simulado, debido a los costos. Además, la simulación es más rápida que en tiempo real y se pueden ejecutar en paralelo.

Comparación entorno real vs simulado:
1. Real 
    - preciso
    - simple
    - algunas veces necesario (difícil de hacer modelo)
2. Simulado
    - veloz
    - fácil de modelar condiciones de testeo
    - seguro

## 2. Señal de recompensa
Debemos recompensar al agente por sus acciones, para así orientarlo a alcanzar el objetivo. 

En RL no hay un límite para crear una función de recompensa, podemos tener muchas, pocas, sólo al final de un episodio, por tiempo, por alcanzar el objetivo final, etc. Lo que sí hay que tener en cuenta es que la función de recompensa debe producir un número escalar que indique la *bondad* de estar en un estado y haber ejecutado una acción determinada.

$$
recompensa = f(estado, acción)
$$

Las recompensas pueden ser:
- escasas
- en cada paso de tiempo (time step)
- al final de cada episodio
- basada en parámetros
- etc.

Hacer una función de recompensa buena es difícil. No existe una forma sencilla de diseñar una señal o función de recompensa para garantizar que el agente converja a la solución que se quiere. Esto se debe a dos razones:

1. A menudo el objetivo viene de una larga secuencia de acciones. Si sólo recompensamos por cumplir el objeivo, el agente se equivocará por largos periodos de tiempo sin recibir recompensa, a esto se le llama **recompensas escasas**. Es muy poco probable que el agente se tope al azar con la secuencia de acciones que produce la recompensa escasa. Confiar en esta exploración aleatoria es bastante ineficiente, ya que es un aprendizaje muy lento, incluso llega a no ser práctico. Este problema se puede solucionar con el **reward shaping** o dar forma a la recompensa. Si proporcionamos **recompensas intermedias** más pequeñas que induzcan al agente a seguir el camino correcto, lograremos que el agente consiga su objetivo.

2. Si se le da un atajo a un algoritmo de optimización, él lo tomará. Esto hace que el agente converja a la solución que queremos dada la función de recompensa, pero no de una manera idónea. Los atajos están ocultos en la función de recompensa. Por ejemplo: tenemos un robot caminante cuyo objetivo es caminar hasta los 10 metros. Supongamos que tenemos pensado darle 2 recompensas intermedias y 1 final por llegar a los 10 metros. Además de llegar al objetivo es importante considerar el *cómo* se llega a él; para este robot desplomarse y arrastrase hasta llegar a los 10 metros es lo mismo que caminar erguido y sin desviarse hasta los 10 metros. Al agente le da lo mismo porque al final igual va a recibir la recompensa. Sin embargo, a nosotros nos interesa que el robot cumpla su objetivo de manera idónea.

¿Cómo solucionamos el problema de los atajos? Inyectando conocimiento específico del dominio en el agente. Vamos a recompensar al agente no solo por llegar al objetivo sino también por el *cómo lo hace*. Por ejemplo, recompensar al robot por caminar erguido, a una velocidad determinada, que no se desvíe del camino, etc.

### 2.1. Exploración y explotación
La idea es la siguiente:
- Explotación: agente explota el entorno eligiendo las acciones que recogen la mayor cantidad de recompensas que ya conoce.
- Exploración: elegir acciones que exploran partes del entorno que aún no se conocen.


Existe un trade-off entre exploración y explotación mientras el agente interactúa con el entorno.

Las acciones que hace el agente determinan la información que recibe y, por lo tanto, lo que aprende.

```
                    Estado actual
                          s1
         explotar    a1 /     \ a2     explorar
                      /         \
                    s2          s3
                  R. conocida     R. desconocida
```
Si ejecutamos `a1` el agente sólo recibirá información de `s2` obteniendo una recompensa conocidad y nada sobre `s3`. Si realizamos `a2` el agente sólo recibirá información de `s3` (un estado nuevo) y la recompensa puede que sea mejor que la entregada por `s2`.

#### Explotación pura
```
estados conocidos      estado actual            estados desconocidos
0    +1   0    +1        agente          +1     ¿?   ¿?
s0   s1   s2   s3          s4            s5     s6   s7
      |____|____|___________v   
```
El agente nunca recibirá información adicional sobre los estados desconocidos.

Con explotación pura, el algoritmo de aprendizaje converge en una política subóptima. No sabremos si las áreas desconocidas poseían mayores recompensas.

#### Exploración pura
Si bien existe riesgo de recibir menos recompensas, la exploración permite expandir la política para los nuevos estados. Tenemos la chance de recibir mejores recompensas, y por ende, más posibilidades de encontrar la solución global.

La exploración pura no es buena cuando se entrena con hardware físico, ya que lo puede dañar. Además el aprendizaje es más lento y puede que no encontremos una solución en tiempo razonable.

Los mejores algoritmos son los que logran el equilibrio entre ambas.


## 3. Política
El agente se compone de una política y un algoritmo de aprendizaje de RL. Muchos algoritmos de aprendizaje requieren una estructura de política específica. La elección del algoritmo también depende de la naturaleza del entorno.

En la política se representan la lógica y los parámetros. Esta política es una función matemática que toma las observaciones de estado y genera las acciones.
$$
\text{observaciones} \rightarrow f(x) \rightarrow \text{acciones}
$$

Existen dos enfoques para estructurar la función de política:
1. Directo (actores o policy-based): hace un mapeo entre observaciones y acciones específico.
2. Indirecto (críticos o value-based): se basa en otras métricas como el valor para inferir el mapeo óptimo.

### Representamos la política con una tabla: Función o Tabla Q 
Si los espacios de estado y acción son **discretos** y pequeños en número, podemos ocupar una tabla simple para representar la política.

La función o tabla Q es una tabla que asigna estados y acciones a valores. Entonces, dado un estado *S*, la política sería buscar el valor de cada acción *A* posible en ese estado y elegir la acción con el valor más alto.

$$
\begin{table}[]
\begin{tabular}{lllll}
 & a1 & a2 & a3 \\
 s1 & -1 & 2 & 0 \\
 s2 & 0 & 1 & 5 \\
 s3 & 3 & -1 & 2 
\end{tabular}
\end{table}
$$

> Entrenar a un agente con una Tabla Q consistiría en calcular los valores para todas las acciones posibles en cada estado.

> La función Q falla cuando el número de acciones aumentan mucho o se vuelve infinito, en otras palabras, cuando son continuos.

Calcular los valores para muchísimas acciones posibles para cada uno de los muchísimos estados que tenemos no es factible. Si una función o tabla Q no nos sirve, entonces necesitamos una función continua que sea capaz de representar la política, aunque es bastante difícil de diseñar. La solución a esto es usar un **aproximador de función** de propósito general para representar la política, algo que pueda manejar estados y acciones dentro de un espacio continuo: **Las redes neuronales profundas (Deep Learning)**.

#### ¿Por qué usar redes neuronales y no tablas o una función de transferencia para la política?

*Nota: la función de transferencia es una función que relaciona las entradas con las salidas de un sistema. Se utiliza mucho en control.*

Las tablas no son prácticas cuando el espacio de estados y acciones son muy grandes (continuos).

Para el caso de las funciones de transferencia, es difícil diseñar la estructura de estas funciones para entornos complejos.

## 2.1 Política como red neuronal
Una red neurona es un grupo de nodos llamados **neuronas artificiales** que están interconectados de forma que se vuelven un **aproximador de función universal**. Esto significa que se puede configurar la red con la correcta combinación de nodos y conexiones para imitar cualquier relación de entrada y salida. La función generada por la red neuronal puede ser extremadamente compleja, sin embargo, la naturaleza de las redes neuronales asegura que se toda función se puede aproximar.

El aprendizaje de la red consiste en el ajuste de los parámetros sistemáticamente para encontrar la relación óptima de entrada/salida.

*----imagen de red neuronal----*

La salida de una neurona está dada por:
$$
a = f(w_0\cdot x_0 + w_1\cdot x_1 + ... + w_{n-1}\cdot x_{n-1} + b)
$$
donde $$b$$ es el **bias** o sesgo, $$w$$ son los pesos asginados a cada entrada $$x$$.

Sin $$f$$ la salida o activación de una neurona es una operación lineal ($$w\cdot x + b$$ es una suma ponderada o combinación lineal). Si ninguna neurona de nuestra red neuronal utilizara la función $$f$$, la salida de la red sería lineal. El inconveniente es que los problemas lineales son simples, muy por el contrario de los problemas de la vida real que son complejos, o sea, no lineales. Es por esto que se utiliza una **función de activación $$f$$** para poder aproximar funciones no lineales. Esta función de activación transforma el valor de la suma ponderada a otro valor (depende de la función) que es el que finalmente sale de la neurona y sirve de entrada a las neuronas de las siguientes capas.

### Funciones de activación
Las 3 funciones de activación más populares son:
1. Linear: simplemente es dejar como salida de la neurona a la suma ponderada $$w\cdot x + b$$.
2. Sigmoid: se mapea $$w\cdot x + b$$ a un valor entre 0 y 1.
3. ReLU: si $$w\cdot x + b$$ es positivo, se deja como salida $$w\cdot x + b$$. Si es 0 o negativo, la salida será 0.

Existen más funciones de activación no lineales como por ejemplo, **Tanh**, **LeakyReLU**, **Softmax**, etc., que igualmente se utilizan, pero son menos populares.

$$
\begin{table}[]
\begin{tabular}{lllll}
 Valor & Sigmoid & ReLU \\
 -2 & 0.12 & 0 \\
 -1 & 0.27 & 0 \\
 1 & 0.73 & 1 \\
 2 & 0.88 & 2 \\
\end{tabular}
\end{table}
$$

> Se debe tener en cuenta que la red neuronal debe ser lo suficientemente compleja como para aproximarse a la función, pero no tan compleja como para que el entrenamiento no sea posible o sea muy lento.


### Diseño de una red neuronal
Se debe elegir lo siguiente para implementar una red neuronal:
- Función de activación para las capas ocultas y la capa de salida respectivamente (pueden ser distintas o para los dos tipos de capa puede ser la misma)
- Número de capas ocultas
- Número de neurona en cada capa
- Estructura interna de la red: ¿Totalmente conectada (fully connected)? ¿Nos saltamos capas (red residual)? ¿La red tiene memoria interna (red recurrente)? ¿Grupos de neuronas trabajan en conjunto (red convolusional)?

*--- imagen de los tipo de redes ----*

No existe un enfoque preestablecido para la estructura de la red, pero una idea sería comenzar con una estructura que ya ha funcionado para el tipo de problema que estamos resolviendo.




## 4. Algortimos de aprendizaje RL
Existen 3 tipos:
1. Policy function
2. Value function
3. Actor / critic

### 4.1. Policy function