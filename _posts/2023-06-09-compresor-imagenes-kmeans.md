---
layout: post
title: Compresión de una imagen utilizando k-means
date: 2023-06-09 21:41
description: Definición, análisis, implementación y consejos del algoritmo K-means
categories: algoritmos
giscus_comments: true
related_posts: false
---

Para representar una imagen RGB podemos usar hasta: 255 (Canal R) * 255 (Canal G) * 255 (Canal B) = 16.581.375 colores (combinaciones posibles). Además, se requieren 24 bits (8 bits por cada canal) para cada uno de los pixeles de la imagen (128x128 pixeles) dando así un tamaño total de imagen de 128 x 128 x 24 = 393.216 bits.

Sin embargo, para una imagen cualquiera no se utiliza todo este espectro de colores. No obstante, el número de colores a utilizar es aún grande. Entonces, si reducimos este número de colores a algo como 16, o sea, elegimos 16 colores para representar toda la imagen.

Esto sería muy bueno porque solo necesitaríamos recordar los 16 colores, y en cada pixel, ya no se requerirá tener 3 números entre 0-255 para representar el color, sino que solamente el índice en el vector de 16 colores que corresponde al color que pintamos en el pixel.

Esta nueva representación requiere un leve overhead, ya que guardamos el vector de 16 colores. Cada uno de estos colores en el vector requiere 24 bits. Sin embargo, para la imagen en sí misma sólo se requieren 4 bits por ubicación de pixel (los 4 bits son para representar un número entre 0-15).

El tamaño total de la imagen es: (16x24) + (128x128x4) = 65.920 bits
