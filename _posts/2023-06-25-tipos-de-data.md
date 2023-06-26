---
layout: post
title: Tipos de data
date: 2023-06-10 20:30
description: Tipos de data en ML
categories: datos
giscus_comments: true
related_posts: true
---


## 1. Sparse data
Sparse data o datos dispersos se refiere a una matriz de datos de alta dimensión que está compuesto en su mayoría por 0's y el resto de data con un valor distinto de 0. 

> Atención: que los valores sean 0 no significan que no tienen significado o que no aportan información útil.

Ejemplo: En álgebra lineal una matriz dispersa o rala es una matriz de gran tamaño en la que la mayor parte de sus elementos son 0. Recordar que una matriz de alta dimensión (high-dimension) requiere una gran cantidad de memoria y procesamiento de una computadora.

## 2. Dense data
Al contrario de la sparse data, dense data se refiere a una matriz que en su mayoría está compuesto de elementos cuyos valores son distintos de 0.


## 3. Missing data
No es lo mismo que sparse data. Missing data se refiere a una matriz de datos que contiene valores como `NaN` o `Null`, o sea, valores que no son útiles en lo absoluto y no se puede concluir nada acerca de ellos, ya que no poseen un valor interpretable. Podemos llenar los valores faltantes manualmente sin afectar al resto, eliminarlos o volver a medir, etc.

---

[Fuente](https://stats.stackexchange.com/questions/266996/what-do-the-terms-dense-and-sparse-mean-in-the-context-of-neural-networks)

[Fuente](https://stats.stackexchange.com/questions/267322/difference-between-missing-data-and-sparse-data-in-machine-learning-algorithms)

