---
layout: post
title: Reinforcement Learning y control
date: 2023-06-10 20:30
description: Definición y workflow del Aprendizaje por refuerzo, y su aplicación para la resolución de problemas de control.
categories: algoritmos
giscus_comments: true
related_posts: true
---

*Material obtenido del e-book de matlab de Reinforcement Learning*

# Reinforcement Learning (RL)
Permite resolver problemas muy difíciles de control. Por ejemplo, un robot caminante. Un robot caminante es muy difícil de lograr con el enfoque de control, ya que necesitaríamos muchos loops de control, sensores, controladores de motor, etc.

En RL, trabajamos con un entorno dinámico, a diferencia del Supervised learning y el Unsupervised learning que son datasets estáticos.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/ul_sl_rl.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Unsupervised learning, Supervised learning y Reinforcement Learning se utilizan para problemas diferentes. El primero para encontrar patrones ocultos interesantes, el segundo para etiquetar correctamente los datos, y el tercero se utiliza para encontrar la mejor secuencia de acciones que generan la salida óptima.
</div>


RL consiste en un agente que se relaciona con un entorno y va tomando acciones que afectan a ese entorno. Dicho entorno cambia de estado tras cada acción que tome el agente y además le proporciona una recompensa al agente por cada acción que ejecute. Estas acciones o decisiones pueden ser buenas o malas, y dependiendo de ésto va a recibir un recompensa buena o mala respectivamente. Cabe destacar que **al agente nunca se le dice qué hacer, sino que él mismo debe descubrir qué acciones tomar, de tal forma que produzca la mayor recompensa a largo plazo**.

> Con RL, queremos encontrar la mejor secuencia de acciones que generarán la salida óptima, o sea, la que genera la mejor recompensa final. La idea es maximizar esta recompensa a largo plazo.

> El agente es un software que explora, interactúa y aprende del entorno

Ejemplo de reinforcement learning:
1. agente: ser humano
2. entorno: ciudad en la que vive el agente
3. acción 1: el agente mira a ambos lados para cruzar la calle
4. Luego de la acción 1, el entorno cambia de estado y el agente obtiene una recompensa:
    - observaciones de estado: el agente se encuentra en la otra vereda de la calle de la ciudad
    - recompensa: no resultó herido al cruzar la calle.



## I. Funcionamiento de RL
El procedimiento es el siguiente:

* Paso 1: Agente observa el estado actual del entorno
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/paso1.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

* Paso 2: Decide cual acción tomar dependiendo del estado
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/paso2.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

* Paso 3: Entorno cambia de estado y produce una recompensa por la acción ejecutada
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/paso3.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

* Paso 4: ¿Fueron buenas las acciones o malas?
    - buenas: repetirlas
    - malas: evitarlas
    Repetir hasta terminar el aprendizaje.
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/paso4.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

## II. Política
El agente internamente toma las observaciones del estado del entorno y las asigna a acciones realizando así un mapeo. En otras palabras, se puede entender este mapeo como una función que recibe entradas y genera una salida. A este mapeo se le llama **política**. La política es muy importante, ya que decide qué acción ejecutar dado un conjunto de observaciones de estado. Básicamente la política es el *cerebro* de nuestro agente, y le va a indicar qué hacer. La política se puede representar de varias formas, una forma muy útil es que si tenemos observaciones de estado complejas como por ejemplo, imágenes, entonces podemos usar una red neuronal para procesar dichos datos.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/politica.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    La política es la función que toma observaciones y genera acciones que influyen sobre el entorno.
</div>


Ejemplo de política en un robot caminante:
1. Observaciones: ángulo de cada articulación, aceleración, velocida angular del tronco y los miles de pixeles de un sensor de visión.
2. Política: toma las observaciones y genera comandos de acción que mueven los brazos y piernas del robot.
3. Recompensa: qué tan bien funcionaron la combinación de comandos: ¿Se cayó el robot?, ¿Se desvió del camino?, ¿Se está arrastrando? ¿Esta caminando erguido? Etc.


<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/politica_robot.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    RL en un robot caminante.
</div>

## III. Algoritmo de Reinforcement Learning
La política debe ajustarse, no puede ser estática, ya que el entorno es dinámico; para esto existen los **algoritmos de Reinforcement Learning**. Un algoritmo de RL hace óptima a la política, cambiandola en función de las acciones que se tomaron, las observaciones del estado del entorno y la cantidad de recompensa recolectada.

> Un agente de RL utiliza un algoritmo de RL para modificar su política a medida que interactúa con el entorno, de modo que eventualmente, dado cualquier estado, siempre tomará la mejor acción: la que producirá la mayor recompensa a largo plazo.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/agente_alg.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Algoritmos RL + la política.
</div>

## IV. ¿Cómo un agente aprende del entorno? 
Una política es una función compuesta por parámetros lógicos y ajustables. Para una estructura de política dada, existe un conjunto de parámetros que producirán una política óptima, o sea, un mapeo de los estados a las acciones que producen la recompensa más grande a largo plazo.

> El aprendizaje es el término que se le da al proceso de ajustar sistemáticamente esos parámetros para converger en la política óptima.

El agente usa la información que obtiene del entorno (observaciones del estado del entorno y recompensa) para ajustar sus acciones a futuro. La recompensa es muy importante porque le va a decir al agente qué tan buena fue la acción que acaba de realizar.


## V. Valor y recompensa (Value and reward)
* Valor (value): recompensa total que un agente puede esperar recibir desde ese estado en adelante.
* Recompensa (reward): beneficio inmediato de estar en un estado y realizar una acción específica.

### ¿Por qué es importante el valor?
Evaluar el valor de un estado o una acción en vez de la recompensa inmediata ayuda al agente a elegir la acción que obtendrá la mayor recompensa a lo largo del tiempo, en vez de un beneficio a corto plazo.

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
- Recompensa inmediata puede ser mejor que esperar por una futura que implique una secuencia de varios pasos.
- Predicción de recompensas puede fallar y esa recompensa alta puede que no esté cuando lleguemos a esos estados, o sea, existe mayor incertidumbre.
- La solución a esto es descontar recompensas mientras más lejos estén el futuro. Esto se hace estableciendo el factor de descuento $$\gamma$$ entre 0 y 1:

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/rec_descontada.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Descuento de recompensas futuras.
</div>


¿Y qué pasa cuando hay estados que no se conocen? Cabe la posibilidad de que existan estados que sean desconocidos para el agente, pero éstos pueden contener recompensas mayores a las de los estados que actualmente conocemos. Es aquí donde entran los dos enfoques que puede aplicar el agente.

### Enfoque muy codicioso: explotación del entorno
Recolectamos la mayor cantidad de recompensas que se conozcan, o sea, las más cercanas. Le damos más relevancia al beneficio inmediato que al futuro.

```
+1  agente  -1  ?  ?
```
El agente irá a la izquierda.

### Enfoque poco codicioso: exploración del entorno
Exploramos estados desconocidos del entorno con la esperanza de obtener mejores recompensas y por consiguiente, una mejor recompensa a largo plazo. Sin embargo, corremos el riesgo de recolectar peores recompensas por algún tiempo, o que incluso descubramos que estas recompensas no sean tan buenas como las que conocíamos.

```
+1  agente  -1  -1  -1   10
```
El agente irá a la derecha.

### Explotación vs Exploración
El algoritmo de RL explorará o explotará el espacio de estados, convirtiéndose esto en un problema de optimización

Si bien explorar para obtener una gran recompensa en el futuro puede ser muy tentador a elegir, puede que no sea tan buena opción, esto se debe a que es posible que:
1. La recompensa actual es mayor que la recompensa después
2. Las recompensas en el futuro son menos confiables, ya que podrían no estar allí cuando se alcancen, por lo que no hay que confiar 100% en la predicción de recompensa.

> RL descuenta las recompensas en una cantidad mayor cuanto más lejos estén en el futuro.

> El algoritmo de RL establece un equilibrio entre exploración y explotación. Este trade-off se da mientras el agente interactúa con el entorno.


## VI. Control de sistemas y RL
Tenemos un sistema o proceso industrial que queremos controlar. Controlamos las entradas del sistema (acciones) para intentar generar las salidas deseadas (comportamientos).

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/control.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Sistema de control.
</div>

Controlamos las entradas mediante un software controlador.

Utilizamos las observaciones del estado (retroalimentación) para mejorar el rendimiento y corregir errores y perturbaciones aleatorias.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/lazo_cerrado.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Un sistema de control en lazo cerrado utiliza la retroalimentación para ajustar las acciones.
</div>

Si el problema es muy complejo, utilizamos múltiples loops de control andidados, motores, sensores y controladores, lo que es muy difícil de implementar y modificar. 

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/loops.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Este sería el controlador para un robot. Estos loops interactúan entre sí.
</div>


El controlador tiene que encontrar la combinación de comandos o acciones que hacen que el sistema de comporte correctamente, incluso frente a perturbarciones.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/robot_control.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Para el caso de un robot caminante, el controlador tendría que mover cada articulación del robot para que camine y no tropiece, incluso frente a obstáculos.
</div>


En control, el diseñador es el que se preocupa de modificar explícitamente el sistema para que tenga el comportamiento idóneo. No obstante, podemos utilizar RL para resolver los problemas de control. En RL, el software por sí mismo intenta aprender el comportamiento óptimo a lo largo del tiempo. La idea es tomar las observaciones del estado del sistema y el agente generará las mejores acciones para controlar el sistema. *El agente sería el controlador*.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/rl_ayuda_control.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    RL permite generar una sola función matemática que reemplace a todas esos loops complicados de control.
</div>

Estrictamente hablando, el controlador sería la política (recordar que la política es el cerebro del agente), ya que va a mapear las observaciones del estado del sistema a comandos de acción que permitan generar el comportamiento que queremos que este sistema tenga. Nótese que *el entorno sería el sistema a controlar*. 

> El controlador influye sobre el sistema cambiando su estado.

## VII. Uso de RL en control
Para utilizar RL en control tenemos que:
1. Establecer la estructura del controlador: definir la política
2. Definir ¿qué es un resultado exitoso? Y establecer recompensas cuando se consiga: Establecer una función de recompensa que el indique al algoritmo si está mejorando o no.
3. Aplicar un algoritmo de aprendizaje eficiente que sepa cómo ajustar los parámetros para que el proceso converja en un tiempo razonable: Elegir un algoritmo de RL


<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/uso_rl_control.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Uso de RL en un sistema de control.
</div>


## VIII. Workflow de RL
El flujo de trabajo de RL consiste en:
* Paso 1: Establecer un entorno: qué debe existir en ese entorno. Además debemos decidir: ¿Durante el entrenamiento probamos con un entorno real (hardware real) o simulado (uso de modelos matemáticos del sistema)?
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/1_entorno.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>


* Paso 2: Definir la señal de recompensa: qué debe hacer el agente, cómo debe llegar al objetivo, diseñar la función de recompensa que incentive al agente.
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/2_recompensa.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

* Paso 3: Establecer la política: cómo representamos la política: estructuración de los parámetros y la lógica de la toma de decisiones del agente. ¿Cuál es la información que recibe el agente? ¿Cuál debe ser la salida que genera el agente? ¿De qué tipo son estas entradas? ¿Voy a representar la política con una red neuronal? Etc.
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/3_politica.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

* Paso 4: Entrenamiento: necesitamos escoger un algoritmo de RL que permita obtener la política óptima. Se debe elegir el mejor de acuerdo a nuestro caso. Los algoritmos de RL dependen de la estructuración de la política.
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/4_entrenamiento.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

* Paso 5: Deploy/Implementación: se implementa la política en un entorno real y se verfican los resultados.
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/5_deploy.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>


## 1. Entorno
En RL, el entorno es de dónde el agente aprende. El entorno es todo lo que está **afuera** del agente. Es donde el agente envía acciones, y es lo que genera recomepensas y observaciones. 

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/entorno_rl.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Entorno en RL.
</div>

En RL, el entorno es todo menos el agente. Esto llevado a control sería todo lo que no es el controlador: lazo de retroalimentación, sistema, señal de referencia, etc.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/entorno_rl_control.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Entorno RL en control.
</div>


Un agente realiza **acciones** que influyen sobre el entorno. El entorno cambia de estado, al hacerlo, informa al agente entregándole **observaciones de estado** y una **recompensa**.


Existen dos tipos de RL:
### 1.1. RL sin modelo (model-free RL)
El agente aún no sabiendo nada sobre el entorno es capaz de aprender la política óptima. Por ejemplo, el agente no necesita saber las leyes que rigen al robot caminante ni como se mueven las articulaciones. Se coloca un agente RL en cualquier sistema y asumiendo que a la política se le da acceso a las observaciones, acciones y estados internos, el agente obtendrá la mayor recompensa por sí solo. A esto se le llama **RL sin modelo**. 


<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/rl_no_modelo.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    RL sin un modelo que lo ayude.
</div>

Como el agente no tiene conocimiento del entorno, debe explorar todas las áreas del espacio de estados para averiguar cómo recolectar la mayor recompensa, lo que conlleva más tiempo en aprender la política óptima.

El RL sin modelo se utiliza cuando es difícil generar un modelo. Ej: controlar un auto, robot caminante.


### 1.2. RL basado en modelo (Model-based RL) 
Sin ninguna comprensión del entorno el agente deberá explorar todas las áreas del espacio de estados, por lo que gastará tiempo explorando áreas de bajas recompensas mientras está aprendiendo. Al proporcionar un modelo del entorno, o parte de él, proporcionamos al agente información de zonas del espacio de estado que no valen la pena explorar. Entonces, el modelo puede complementar el proceso de aprendizaje evitando áreas que sabe que son malas y el agente aprenderá mejor.

Este modelo sirve de guía para el agente, complementando el aprendizaje de este último. El tiempo es menor en aprender la política óptima.


<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/rl_con_modelo.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    RL con un modelo que lo ayude.
</div>



### 1.3. ¿Entorno real (físico) o simulado?
Cuando puede ocurrir daño en el hardware o a las personas se ocupa un entorno simulado, debido a los costos. Además, la simulación es más rápida que en tiempo real y se pueden ejecutar en paralelo.

Comparación entorno real vs simulado:
1. Real 
    - Preciso
    - Simple: no se gasta tiempo creando y validando el modelo
    - Necesario: algunas veces necesario (difícil de hacer modelo)
2. Simulado
    - Veloz y paralelizable
    - Fácil de modelar condiciones de testeo
    - Seguro

#### RL en matlab
Si ya tenemos un modelo en Matlab de nuestro sistema, reemplazamos el controlador existente con un agente RL, y añadimos una función de recompensa al entorno, y así empezamos el proceso de aprendizaje.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/rl_matlab.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Implementación de RL con modelo Matlab.
</div>


## 2. Señal de recompensa
Debemos recompensar al agente por sus acciones, para así orientarlo a alcanzar el objetivo. Esto requiere crear una función de recompensa para que el algoritmo de aprendizaje entienda cuando la política se está volviendo mejor.

### 2.1. ¿Qué es una recompensa?
> La recompensa es una función que produce un número escalar que representa la *bondad* de estar en un estado particular y realizar una acción determinada.

$$
\text{recompensa} = f(\text{estado}, \text{acción})
$$

En RL no hay un límite para crear una función de recompensa. Las recompensas pueden ser:
- escasas
- en cada paso de tiempo (time step)
- al final de cada episodio
- basada en parámetros
- etc.

Hacer una función de recompensa buena es difícil. No existe una forma sencilla de diseñar una señal o función de recompensa para garantizar que el agente converja a la solución que se quiere. Esto se debe a dos razones:

* 1.- A menudo el objetivo se cumple luego de una larga secuencia de acciones. Si sólo recompensamos por cumplir el objetivo, el agente se equivocará por largos periodos de tiempo sin recibir recompensas, a esto se le llama **recompensas escasas**. Es muy poco probable que el agente se tope al azar con la secuencia de acciones que produce la recompensa escasa. Confiar en esta exploración aleatoria es bastante ineficiente, ya que es un aprendizaje muy lento, incluso llega a no ser práctico. Este problema se puede solucionar con el **reward shaping** o dar forma a las recompensas. Si proporcionamos **recompensas intermedias** más pequeñas que induzcan al agente a seguir el camino correcto, lograremos que el agente consiga su objetivo.
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/reward_shaping.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Reward shaping.
</div>


* 2.- El reward shaping viene con su propio conjunto de problemas. Si se le da un atajo a un algoritmo de optimización, él lo tomará. Los atajos están ocultos en la función de recompensa. Una función de recompensa mal moldeada puede hacer que el agente converja a una solución no ideal, incluso si produce la mayor recompensa. Por ejemplo: tenemos un robot caminante cuyo objetivo es caminar hasta los 10 metros. Supongamos que tenemos pensado darle recompensas intermedias y 1 grande al final por llegar a los 10 metros. Además de llegar al objetivo es importante considerar el *cómo* se llega a él; para este robot desplomarse y arrastrase como una oruga hasta llegar a los 10 metros es lo mismo que caminar erguido y sin desviarse hasta los 10 metros. Al agente le da lo mismo porque al final igual va a recibir la recompensa. Sin embargo, a nosotros nos interesa que el robot cumpla su objetivo de manera idónea.
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/reward_shaping_problema.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    El problema del reward shaping.
</div>

¿Cómo solucionamos el problema de los atajos? Inyectando conocimiento específico del dominio en el agente. Vamos a recompensar al agente no solo por llegar al objetivo sino también por el *cómo lo hace*. Por ejemplo, recompensar al robot por caminar erguido, a una velocidad determinada, que no se desvíe del camino, etc.

### 2.2. Exploración y explotación
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

La exploración pura no es buena cuando se entrena con hardware físico, ya que lo puede dañar, o peor aún, puede dañar a las personas, por ejemplo, probar una entrada de volante aleatoria en un auto autónomo. Además el aprendizaje es más lento y puede que no encontremos una solución en tiempo razonable.

Los mejores algoritmos de aprendizaje RL son los que logran el equilibrio entre ambas. Generalmente, un agente explora más al comienzo del aprendizaje y gradualmente pasa a un rol más de explotación al final. 


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

La función o tabla Q es una tabla que asigna (mapea) estados y acciones a un valor. 

$$
\begin{aligned}
& \text {Tabla 1. Ejemplo de política como una Tabla Q}\\
&\begin{array}{c|cc}
\hline \hline \text {  } & \text { a1 } & \text { a2 } & \text { a3 }\\
\hline
s1 & -1 & 2 & 0 \\
s2 &  0 & 1 & 5 \\
s3 & 3 & -1 & 2 \\
\hline
\end{array}
\end{aligned}
$$

Entonces, dado un estado *S* actual, la política sería buscar el valor de cada acción *A* posible en ese estado y elegir la acción con el valor más alto.


<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/tablaq.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    La política como tabla Q toma el estado actual en el que estamos y elige la acción con el valor más alto.
</div>


> Entrenar a un agente con una Tabla Q consistiría en calcular el valor para todas las acciones posibles en cada estado.

> La función Q falla cuando el número de pares estado/acción se vuelve muy grande o infinito, en otras palabras, cuando son continuos.

Por ejemplo, imaginemos que queremos controlar un péndulo invertido. El estado del péndulo puede ser cualquier ángulo de $$-\pi$$ a $$\pi$$. El espacio de acción es cualquier torque de motor desde el $$-\infty$$ a $$\infty$$. Tratar de capturar cada combinación de cada estado y acción en una tabla es imposible.

Si una función o tabla Q no nos sirve, entonces necesitamos una función continua que sea capaz de representar la política, aunque es bastante difícil de diseñar. La solución a esto es usar un **aproximador de función** de propósito general para representar la política, algo que pueda manejar estados y acciones dentro de un espacio continuo: **Las redes neuronales profundas (Deep Learning)**.

#### ¿Por qué usar redes neuronales y no tablas o una función de transferencia para la política?

*Nota: la función de transferencia es una función que relaciona las entradas con las salidas de un sistema. Se utiliza mucho en control.*

Las tablas no son prácticas cuando el espacio de estados y acciones son muy grandes (continuos).

Para el caso de las funciones de transferencia, es difícil diseñar la estructura de estas funciones para entornos complejos.

### 3.1 Política como red neuronal
Una red neurona es un grupo de nodos llamados **neuronas artificiales** que están interconectados de forma que se vuelven un **aproximador de función universal**. Esto significa que se puede configurar la red con la correcta combinación de nodos y conexiones para imitar cualquier relación de entrada y salida. La función generada por la red neuronal puede ser extremadamente compleja, sin embargo, la naturaleza de las redes neuronales asegura que toda función se puede aproximar.

El aprendizaje de la red consiste en el ajuste de los parámetros sistemáticamente para encontrar la relación óptima de entrada/salida.


<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/red_neuronal.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Red con 2 entradas (recibe 2 valores), 2 capas ocultas con 3 neuronas cada una, y una capa de salida con las 2 salidas finales de la red.
</div>

La salida de una neurona está dada por:
$$
a = f(w_0\cdot x_0 + w_1\cdot x_1 + ... + w_{n-1}\cdot x_{n-1} + b)
$$
donde $$b$$ es el **bias** o sesgo, $$w$$ son los **weights** o pesos asginados a cada entrada $$x$$.

Sin $$f$$ la salida o activación de una neurona es una operación lineal ($$w\cdot x + b$$ es una suma ponderada o combinación lineal). Si ninguna neurona de nuestra red neuronal utilizara la función $$f$$, la salida de la red sería lineal. El inconveniente es que los problemas lineales son simples, muy por el contrario de los problemas de la vida real que son complejos, o sea, no lineales. Es por esto que se utiliza una **función de activación $$f$$** para poder aproximar funciones no lineales. Esta función de activación transforma el valor de la suma ponderada a otro valor (depende de la función) que es el que finalmente sale de la neurona y sirve de entrada a las neuronas de las siguientes capas.

Que las funciones de activación sean no lineales es fundamental para crea una red que pueda aproximarse a cualquier función. Esto se debe a que muchas funciones no lineales se pueden dividir en una combinación ponderada de salidas de función de activación.


### Funciones de activación
Las 3 funciones de activación más populares son:
1. Linear: simplemente es dejar como salida de la neurona a la suma ponderada $$w\cdot x + b$$.
2. Sigmoid: se mapea $$w\cdot x + b$$ a un valor entre 0 y 1.
3. ReLU: si $$w\cdot x + b$$ es positivo, se deja como salida $$w\cdot x + b$$. Si es 0 o negativo, la salida será 0.

Existen más funciones de activación no lineales como por ejemplo, **Tanh**, **Leaky ReLU**, **Softmax**, etc., que igualmente se utilizan, pero son menos populares.

$$
\begin{aligned}
& \text {Tabla 2. Ejemplos de transformación de valores para Sigmoid y ReLU}\\
&\begin{array}{cccc}
\hline \hline \text { Valor } & \text { Sigmoid } & \text { ReLU } \\
\hline
-2 & 0.12 & 0 \\
-1 & 0.27 & 0 \\
1 & 0.73 & 1 \\
2 & 0.88 & 2 \\
\hline
\end{array}
\end{aligned}
$$

> Se debe tener en cuenta que la red neuronal debe ser lo suficientemente compleja como para aproximarse a la función, pero no tan compleja como para que el entrenamiento no sea posible o sea muy lento.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/red_neuronal2.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Política representada como una red neuronal.
</div>


### Diseño de una red neuronal
Se debe elegir lo siguiente para implementar una red neuronal:
- Función de activación para las capas ocultas y la capa de salida respectivamente (pueden ser distintas o para los dos tipos de capa puede ser la misma)
- Número de capas ocultas
- Número de neurona en cada capa
- Estructura interna de la red: ¿Totalmente conectada (fully connected)? ¿Nos saltamos capas (red residual)? ¿La red tiene memoria interna (red recurrente)? ¿Grupos de neuronas trabajan en conjunto (red convolusional)?

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/red_neuronal_tipos.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Tipos de redes neuronales.
</div>


No existe un enfoque preestablecido para la estructura de la red, pero una idea sería comenzar con una estructura que ya ha funcionado para el tipo de problema que estamos resolviendo.


## 4. Entrenamiento: Algortimos de aprendizaje RL
La estructura de la política y el algoritmo de aprendizaje RL están íntimamente entrelazados. No se puede estructurar la política sin elegir el algoritmo de RL de aprendizaje.

Existen 3 formas de estructurar la política:
1. Policy function-based
2. Value function-based
3. Actor / critic

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/policy_estructura.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Formas de estructurar la política.
</div>


### 4.1. Policy function-based learning
Estos algoritmos de aprendizaje entrenan una red neuronal que toma las observaciones del estado y produce acciones. Esta red neuronal es la política completa, de ahí el nombre de algoritmos basados en función de política. Esta red neuronal se llama **actor** porque le dice directamente al agente qué acciones tomar.


<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/actor.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Red neuronal actor.
</div>

¿Cómo entrenamos una red neuronal o política del tipo actor? La entrenamos con los métodos **policy gradient**.

Veamos un ejemplo del uso de una red neuronal del tipo actor: Juego Breakout.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/breakout.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Captura de pantalla del juego.
</div>

En el juego, intentamos eliminar los ladrillos usando una paleta para dirigir una pelota que rebota. El juego tiene 3 acciones: mover la paleta a la izquierda, a la derecha, o no moverla. Además, tiene un espacio de estados casi continuo que incluye: posición de la paleta, posición de la pelota, velocidad de la pelota, ubicación de los ladrillos restantes.

- Entradas a la red de actor: estados de la paleta, la pelota y los bloques.
- Salidas de la red de actor: nodos que representan las acciones: izquierda, derecha y mantener.

En lugar de calcular los estados manualmente e introducirlos en la red, podemos ingresar una captura de pantalla del juego y dejar que la red aprenda qué características de la imagen son las más importantes para basar su salida. El actor mapearía la intensidad de miles de píxeles a las tres salidas.


<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/red_breakout.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Red de actor para el juego Breakout.
</div>

Una vez configurada la red, es hora de buscar enfoques para entrenarla. Un enfoque sería los **métodos policy gradient**. Los métodos policy gradient funcionan con una **política estocástica**. Este tipo de política se refiere a que en vez de entregar la salida *izquierda, derecha o mantener* determinista, la política generaría una **probabilidad** de mover a la izquierda, a la derecha o mantener. 


<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/red_estocastica.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Política estocástica.
</div>

La política estocástica incorpora la exploración en las probabilidades. Cuando el agente aprende, actualiza las probabilidades, aumentando la probabilidad de la acción que produjo una buena recompensa.

Con el tiempo, el agente empujará estas probabilidades en la dirección que produzca la mayor recompensa. Eventualmente, la acción venajosa para cada estado tendrá una probabilidad tan alta que el agente siempre realizará esa acción.

#### Policy gradient methods
¿Cómo el agente sabe si las acciones fueron buenas o no? La idea es la siguiente: ejecutar la política actual, recolectar recompensas a lo largo del camino y luego actualizar la red para aumentar las probabilidades de acciones que llevaron a recompensas más altas.

El funcionamiento de los métodos de policy gradient es el siguiente:

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/policy_gradient_ciclo.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Diagrama de flujo del funcionamiento de los métodos policy gradient.
</div>

Estos métodos toman la derivada de cada *weight* y *bias* en la red neuronal con respecto a la recompensa y los ajusta en la dirección de un aumento de recompensa positivo. Así, el algoritmo de aprendizaje mueve los *bias* y *weights* para ascender por la pendiente de recompensa, de ahí viene el nombre de gradiente.


<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/policy_gradient.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Policy gradient methods.
</div>

#### 4.1.1. Lo malo de los métodos de policy gradient
* 1.- El enfoque ingenuo de simplemente seguir la dirección del ascenso más empinado puede converger a un máximo local en vez de uno global.
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/problema1_policy.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Problema del máximo local.
</div>

* 2.- Pueden converger lentamente debido a su sensibilidad a las mediciones ruidosas, o sea, cuando se necesitan muchas acciones secuenciales para recibir una recompensa y la recompensas acumulada resultante tiene gran variación entre episodios.

Por ejemplo, en Breakout, el agente puede hacer muchos movimientos rápidos de paleta hacia la izquierda y hacia la derecha mientras la paleta finalmente se abre camino a través del campo para golpear la pelota y recibir la recompensa. ¿Todas esas acciones previas fueron realmente necesarias para obtener esa recompensa? El algoritmo de policy gradient tendría que tratar cada acción como si fuera necearia y ajustar las probabilidades es consecuencia.
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/problema2_policy.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Problema de la sensibilidad.
</div>

### 4.2. Aprendizaje basado en la función de valor (value function-based learning):
Con un agente basado en la función de valor, una función toma el estado y una de las posibles acciones de ese estado y generaría el valor de tomar esa acción.
$$
\text{valor} = f(\text{observaciones del estado}, \text{acción})
$$

>El valor va a indicar qué tan buena es la acción estando en ese estado.

Esta función sola no es suficiente para representar la política, ya que genera un valor y la política necesita generar una acción. Por lo tanto, la política sería usar esta función para chequear el valor de cada posible acción de un estado dado y elegir la acción con el valor más alto.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/value_function.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    La política en value function-based learning es elegir la acción con el valor más alto dado un estado. Este valor es calculado por la función de valor.
</div>

Esta función se llama **crítico**, ya que critica las posbiles acciones que el agente puede elegir.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/critico.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Crítico.
</div>

Esta función se puede representar con un tabla si es que los espacios de acción y estado son discretos, o con una red neuronal si son continuos. La diferencia en este último caso, es que entran las observaciones de estado y las acciones, y salen los valores de esos pares estado/acción, y la política elige la acción con valor más alto.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/tablaq1.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Tabla Q.
</div>


#### Tabla Q como crítico
En el caso de que el espacio de estados y de acciones fueran discretos los representamos con una tabla Q. El agente aprende estos valores a través de Q-learning:
1. Con Q-learning, se comienza inicializando la tabla con ceros, por lo que todas las acciones tienen el mismo aspecto para el agente.
2. El agente realiza una acción aleatoria, llega a un nuevo estado y recoge la recompensa del entorno.
3. El agente usa esa recompensa como nueva información para acualizar el valor del estado anterior y la acción que tomó usando la ecuación de Bellman.


#### Ecuación de Bellman
Permite al agente resolver la tabla Q a lo largo del tiempo dividiendo todo el problema en varios pasos más simples. En lugar de resolver el valor real de un par estado/acción en un solo paso, el agente actualizará el valor cada vez que se visite el par estado/acción a través de la programación dinámica.

La ecuación de Bellman es la siguiente:

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/ec_bellman.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Ecuación de Bellman.
</div>

Una vez que el agente ha realizado una acción desde el estado *S*, recibe una recompensa.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/ec_bellman2.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

El valor es más que la recompensa instantánea de una acción; es el rendimiento máximo esperado en el futuro. Por lo tanto, el valor del par estado/acción es la recompensa que el agente acaba de recibir más la recompensa que el agente espera cobrar en el futuro.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/ec_bellman3.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Descontamos las recompensas futuras por $$\gamma$$ para que el agente no dependa demasiado de las recompensas en el futuro. $$\gamma$$ es un número entre 0 (no mira recompensas futuras para evaluar el valor) y 1 (mira recompensas infinitamente lejanas en el futuro).


<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/ec_bellman4.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

La suma es ahora el nuevo valor del par de estado y acción $$(s,a)$$, y lo comparamos con la estimación anterior para obtener el error.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/ec_bellman5.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

El error se multiplica por una tasa de aprendizaje que le da control sobre si debe reemplazar la estimación del valor anterior con la nueva $$(\alpha = 1)$$ o empujar el valor anterior en la dirección del nuevo $$(\alpha < 1)$$.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/ec_bellman6.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Finalmente, el valor $$\delta$$ resultante se agrega a la estimación anterior y se actualiza la tabla Q.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/ec_bellman7.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>


#### Crítico como red neuronal
Imaginemos un péndulo invertido. Hay dos estados: ángulo y velocidad angular. Ambos son continuos.

La función de valor o crítico se representa con una red neuronal. La idea es la misma que con una tabla: 
1. ingresamos las observaciones de estado y una acción
2. La red neuronal devuelve el valor de ese par de estado/acción
3. La política es elegir la acción con el valor más alto.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/value_critico.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Red neuronal como crítico en value function-based learning.
</div>

Con el tiempo, la red convergerá lentamente en una función que genera el valor real de cada acción en cualquier lugar del espacio de estado continuo.

#### Lo malo de las políticas basadas en función de valor
Podemos utilizar una red neuronal para definir la función de valor para espacios de estado continuos. Si el péndulo invertido tiene un espacio de acción discreto, podemos alimentar las acciones discretas a la red crítica de una en una.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/pendulo.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Infinitos estados, pocas acciones posibles.
</div>


Las políticas basadas en la función de valor no funcionan bien para espacios de acción continuos. Esto se debe a que no hay forma de calcular el valor uno a la vez para acción infinita para encontrar el valor máximo. Incluso para un espacio de acción grande, pero no infinito, esto se vuelve computacionalmente costoso. Esto es desafortunado porque a menudo en los problemas de control tiene un espacio de acción continuo, como aplicar un rango continuo de torque a un problema de péndulo invertido.

¿Qué podemos hacer? Implementar un método policy-gradient vainilla (visto anteriormente en la sección pasada). Estos algoritmos pueden manear espacios de acción continua, pero tienen problemas para converger cuando hay una gran variación en las recompensas y el gradiente es ruidoso. Podemos fusionar las dos técnincas (value function-based learning y policy function-based) en una clase de algoritmos llamados actor-crítico.


### 4.3. Algoritmos Actor-crítico
Fusión de las dos técnicas anteriores.
- Actor: red que está tratando de tomar lo que cree que es la mejor acción dado el estado actual, tal cual se hacía en los algoritmos de policy function (**sección 4.1.**).
- Crítico: segunda red que está tratando de estimar el valor del par estado/acción que tomó el actor, como se hacía en los algoritmos de value function (**sección 4.2.**). 

Este enfoque funciona para espacios de acción continua porque el crítico solo necesita mirar la accción individual que realizó el actor y no necesita tratar de encontrar la mejor acción evaluándolas todas.

> Actor-crítico funciona para espacios de acción y estados continuos. 

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/metodo_actor_critico.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Algoritmos de actor-crítico.
</div>

#### Funcionamiento de actor-crítico
* Paso 1: El actor elige una acción de la misma manera que lo haría un algoritmo de policy function y se aplica al entorno.
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/paso1_ac.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

* Paso 2: El crítico estima el valor para ese par estado/acción actual.
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/paso2_ac.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

* Paso 3: El crítico usa la recompensa recibida para determinar la precisión de su predicción de valor.
    - El error es: $$\text{error}=\text{valor nuevo estimado del estado anterior}-\text{valor viejo para el estado anterior}$$, ambos dados por la red crítica.
    - El estado anterior es el estado desde el cual se ejecutó la acción actual.
    - El nuevo valor estimado se basa en la recompensa recibida y el valor descontado del estado actual.
    - El error permite que el crítico se de cuenta si las cosas salieron mejor o peor de lo esperado.
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/paso3_ac.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

* Paso 4: El crítico usa este error para actualizarse a sí mismo para que así tenga una mejor predicción la próxima vez que esté en ese estado.
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/paso4_ac.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

* Paso 5: El actor también se actualiza con la respuesta del crítico para que pueda ajustar sus probabilidades de volver a tomar esa acción en el futuro. De esta forma, la política asciende la pendiente de la recompensa en la dirección que recomienda el crítico en lugar de usar las recompensas directamente.
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/paso5_ac.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

El actor y el crítico son redes neuronales que intentan aprender el comportamiento óptimo:
- El actor está aprendiendo las acciones correctas usando la retroalimentación del crítico.
- El crítico está aprendiendo la función de valor para poder criticar correctamente la acción del actor.

>Los métodos de actor-crítico aprovechan lo mejor de los algoritmos de value function y policy function. Los métodos actor-crítico permiten acelerar el aprendizaje cuando hay gran variación en la recompensa recibida. Además, pueden manejar tanto espacios de estado como de acción continuos.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/red_ac.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    En los algoritmos Actor-crítico se deben configurar dos redes neuronales al crear el agente: una para el actor y otra para el crítico.
</div>

## 5. Deployment
### 5.1. Despliegue de política
Si el aprendizaje se hizo físicamente, o sea, en un entorno real, la política aprendida ya está en el agente y puede explotarse.
 
Si el aprendizaje se hizo fuera de línea (offline), o sea, en una simulación; si la política es lo suficientemente óptima, se detiene el aprendizaje y la política estática se puede implementar en cualquier número de objetivos.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/deploy_politica.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Despliegue de la política en hardaware objetivo.
</div>

### 5.2. Despligue del algoritmo de aprendizaje
En algunos casos puede ser necesario continuar aprendiendo con hardware físico real después del despliegue. Algunos entornos pueden ser difíciles de modelar con precisión. Esto quiere decir que una política óptima para el modelo puede que no lo sea para el entorno real. Otra razón puede ser que el entorno cambia lentamente con el tiempo y el agente se debe adaptar a esos cambios.

Es por todo esto que se recomienda desplegar o implementar tanto la política estática como el algoritmo de aprendizaje en el hardware de destino. Con esta configuración, tenemos la opción de ejecutar la política estática, desactivando el aprendizaje; o continuar actualizando la política, activando el aprendizaje.


<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/deploy_alg.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Despliegue del algoritmo RL en hardaware objetivo.
</div>


<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/deploy_alg2.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    El aprendizaje puede ser complementario entre el entorno simulado y el entorno real. 
</div>


## IX. Lo malo del Reinforcement Learning
Existen dos problemas principales:
1. ¿Cómo sabemos que la solución que entrega RL funciona?
2. ¿Se puede ajustar manualmente si no es perfecto?


<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/mal_rl.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Podemos tener un agente perfecto, un entorno perfecto y un algoritmo RL que converje a una solución, sin embargo, la política puede que no sea tan buena.
</div>

### Lo inexplicable de la red neuronal
La política se conforma de una red neuronal con:
- Muchos weights
- Muchos bias
- funciones de activación no lineales
- A esto hay que sumarle la estructura de la red neuronal
Todo esto resulta en una función muy compleja!

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/complex_red.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Una red neuronal es muy compleja.
</div>

> La función que aproxima la red neuronal es una función muy compleja. Se comporta como una caja negra para el diseñador. No conocemos la razón del valor de un weight o bias dado dentro de la red. Si la política no cumple con una especificación o si el entorno operativo cambia, no sabremos como ajustar la política para abordar ese problema.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/blackbox.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    La red neuronal es una caja negra.
</div>


No comprendemos el porqué de la solución entregada. En cambio, un sistema de control se puede explicar, dividir, ajustar, aislar las partes conflictivas, repararlas y volver a juntarlas. Una red neuronal **NO**. Por ejemplo, si tenemos un PID con un sistema $$x$$ y lo cambiamos al sistema $$y$$, simplemente cambiamos las ganancias.

Si el sistema no se comporta como queremos, entonces la política no es del todo correcta. ¿Corregimos la parte defectuosa? No podemos! Tenemos que rediseñar el agente o el modelo del entorno y volver a entrenarlo, lo que puede tomar tiempo.

### ¿Cómo verificamos un sistema de control tradicional?
A través de un testeo: simulación + modelo, y, con hardware físico; y verficamos que el sistema cumpla con las especificaciones, es decir, que hace lo correcto en todo el espacio de estados y en presencia de perturbaciones y fallas de hardware.

Debemos hacer este mismo nivel de prueba con una política de RL. Si encontramos un error, se tiene que volver a entrenar la política (previo a un rediseño).

El ciclo es el siguiente:
1. Rediseño
2. Entrenamiento
3. Testeo

Repetir hasta que se cumplan las especificaciones.

### El problema de la precisión del modelo del entorno
Es difícil desarrollar un modelo suficientemente realista que tenga en cuenta todas las dinámicas importantes del sistema, además que considere el ruido y las perturbaciones. En algún momento no reflejará la realidad de forma perfecta, por lo que se deben hacer prueba físicas en vez de confiar 100% en la simulación con un modelo.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/problema1.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    El modelo no es perfecto, entonces el controlador o agente RL tampoco.
</div>

Podríamos ajustar y modificar un controlador en control tradicional, pero una red neuronal no.

Como no podemos construir un modelo 100% realista, todo agente que entrene con ese modelo estará **ligeramente equivocado**. 

> La solución es terminar de entrenar el agente en hardware físico, lo que puede ser desafiante.

### ¿Cómo verificamos si la política cumple las especificaciones?
Con una política aprendida, es difícil predecir cómo se comportará el sistema en un estado en función de su comportamiento en otro. Por ejemplo, entrenamos a un agente para que controle la velocidad de un motor eléctrico haciendo que aprenda a seguir un step input de 0 a 100 RPM.
- Entrada 1: step input 0 a 100 RPM
- Entrenamos el agente para que siga la señal de referencia de pasar de 0 a 100 RPM.
- La salida será una curva con el aumento paulatino de RPM hasta llegar a 100.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/step100.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Step 100 RPM.
</div>


Sin embargo:
- Entrada 2: step input 0 a 150 RPM
¿El agente se comportará igual? La política anteriormente aprendida se comportará de forma similar a como se comportó con el step input anterior? No podemos saberlo de antemano, debemos testearlo.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/step150.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Step 150 RPM.
</div>


¿Y que pasa con un step input de 30-75? ¿o de 80-93? Tendríamos que probar **todas** las combinaciones para demostrar que la política funciona en un 100%. No hay una verificación matemática que cubra todo el rango.

Un cambio ligero puede hacer que se active un conjunto de neuronas completamente diferentes y produzca un resultado no deseado. Debemos probarlo.

> Probar más implica menos riesgo. Pero, ¿la política es 100% certera? Debemos probar todas las combinaciones de entrada. Pero, ¿y si la entrada es muy grande? Es imposible!

### Métodos formales de verificación
Las redes neuronales dificultan la verificación formal. La verificación formal garantiza que se cumpla alguna condición proporcionando una prueba formal en vez de un test.

Ejemplos:
1. Inspeccionando el código que demuestra que se cumplirá algo siempre, por ejemplo, que la señal sea no negativa.
2. Cálculo de factores de estabilidad y robustez, como los márgenes de ganancia y fase.
    - Esto es difícil para una red neuronal, ya que no podemos ofrecer garantías sobre cómo se comportará. No hay métodos para determinar su robustez o estabilidad. No podemos expllicar qué hace la función internamente.

### Reducción del problema
Reducimos el alcance del agente RL para reducir la escala de estos problemas.

> Solución: RL + control. RL se preocupará de un problema muy especializado, algo muy difícil de resolver con control tradicional. Con respecto al control, los controladores se preocuparán de lo demás.

Una política más pequeña:
- Está más enfocada y es más fácil de entender
- Impacto limitado en todo el sistema
- Menos tiempo de entrenamiento

Esta sería la solución, sin embargo, **aún no podemos garantizar estabilidad, cumplimiento de especificaciones o resistencia a incertidumbres**.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/reduccion_problema.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Reducimos la política para disminuir la complejidad.
</div>



#### Cómo lograr robustez y estabilidad
Haciendo la política RL más robusta.

* 1.- Entrenar el agente en un entorno donde los parámetros importantes del entorno se ajustan cada vez que se ejecute la simulación. 
    Por ejemplo, un robot caminante. Al comienzo de cada episodio cambiamos el valor del torque máximo.
$$
\begin{aligned}
& \text {Tabla 3. Configuración de los parámetros del entorno}\\
&\begin{array}{cccccc}
\hline \hline \text { Episodio } & \text { Torque } & \text { Longitud } & \text { Delay } & \text { Referencia } & \text{ ... }\\
\hline
1 & 2 \text{ Nm.} & 1 \text{ Cm.} & 10 \text{ Ms.} & \text{Step} & \text{ ... }\\
2 & 2.5 \text{ Nm.} & 1.3 \text{ Cm.} & 8 \text{ Ms.} & \text{Ramp} & \text{ ... }\\
3 & 2.1 \text{ Nm.} & 1.7 \text{ Cm.} & 14 \text{ Ms.} & \text{Impulse} & \text{ ... }\\
\text{ ... } & \text{ ... } & \text{ ... } & \text{ ... } & \text{ ... } & \text{ ... }\\
\hline
\end{array}
\end{aligned}
$$

La política será más robusta para esos torques, y si hacemos lo mismo para `longitud`, `delay`, y `referencia`; más robusta será.

La política eventualmente convergerá en algo robusto para esos márgenes, produciendo un diseño robusto en general. El resultado manejará un rango más amplio dentro del espacio de estados operativo.

* 2.- Para la seguridad, se puede hacer un software que ponga en *modo seguro* al agente en una situación peligrosa. Esto protegerá al sistema, lo que permite aprender cómo falla y ajustar la recompensa y entrenar para abordar esa falla.


## X. RL + Control
Utilizar el RL como herramienta para optimizar las ganancias del controlador en un sistema de control de arquitectura tradicional. Por ejemplo, un sistema de control con muchos bucles y controladores anidados, cada uno con varias ganancias. En vez de ajustar manualmente cada una de estas ganancias, puedes configurar un agente RL para aprender los mejores valores para todas a la vez.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/rl_con_control.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Agente RL aprenderá las ganancias del controlador.
</div>


### Pasos de RL + Control
1. Entorno: Sistema de control y planta
2. Recompensa: qué tan bien se desempeña el sistema y cuanto esfuerzo necesitó
3. Acciones: Ganancias del controlador
4. Después de cada episodio, el algoritmo de aprendizaje modifica la red de manera que las ganancias se mueven en la dirección que aumenta la recompensa (más desempeño y menos esfuerzo).
    - Inicialmente, o sea, en el episodio 1, la red se inicializa aleatoriamente y generar los valores aleatorios, los cuales serán las ganancias y se ejecuta la simulación.
5. Codificamos los valores de ganancia estáticos finales dentro del controlador.

#### Ventajas
1. Tenemos un sistema de control tradicional
2. Sistema de control verificable
3. Sistema de control manualmente ajustable en hardware
4. Valores de ganancia óptimos gracias a RL

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/rl_con_control2.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    El agente RL calculará las ganancias óptimas del controlador.
</div>




