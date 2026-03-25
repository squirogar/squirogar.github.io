# Introducción a Docker

## Dockerfile

Docker file es un archivo de texto plano (sin extensión) que contiene la "receta" paso a paso para construir tu imagen.

Un buen Dockerfile es la diferencia entre una aplicación de varios gigas que tarda 10 minutos en desplegarse, y una que pesa 50 megas y arranca en segundos.

### Las 5 instrucciones básicas de un Dockerfile
1. `FROM`: Define la imagen base. Generalmente empezamos con una ya hecha. Por ejemplo, una imagen python, node, alpine, etc.
2. `WORKDIR`: Crea una carpeta de trabajo dentro del contenedor y luego se ubica dentro de ella (es como hacer un `mkdir` + `cd`. Esto se hace para evitar trabajar en la carpeta raíz (`/` o "root").
3. `COPY`: Copia archivos de tu computador real y los pega en el sistema de archivos de la imagen.
4. `RUN`: Ejecuta comandos durante la construcción de la imagen. Por ejemplo, si nosotros queremos hacer un `pip install` o `apt-get install`, tenemos que ejecutarlo mediante esta instrucción. Muy útil para instalar dependencias.
5. `CMD`: Es el comando que se ejecuta solo cuando el contenedor arranca. Este comando lo utilizamos para iniciar la aplicación que está dentro del contenedor ya creado. Una imagen solo puede tener un `CMD`.

### Laboratorio: Dockerizando una app en python
Imaginemos que tenemos un archivo `app.py` y un `requirements.txt` que todas las librerías que nuestra aplicación necesita para funcionar correctamente.

El Dockerfile se vería así:

```
# Usemos una versión ligera de python
FROM python:3.9_slim

# Definimos dónde vivirá nuestra app
WORKDIR /app

# Copiamos los requisitos e instalamos dependencias. No olvidar el punto (indica la carpeta actual dentro del contenedor)
COPY requirements.txt .

# Instalamos las dependencias en /app. (desactivamos el caché de pip porque ya estamos aprovechando el caché de capas de docker)
RUN pip install --no-cache.dir -r requirements.txt

# Copiamos el resto del código
COPY . .

# El comando para ejecutar el programada dentro del contenedor creado
CMD ["python", "app.py"]
```

### ¿Por qué se instalan primero las dependencias y luego copiamos el código? 

Es para aprovechar el **caché de capas**. Si cambias tu código , pero no tus dependencias, Docker se saltará el paso de instalación de dependencias y solo se limitará a copiar el código al contenedor. Esto hace que se construya la imagen muy rápidamente.

### ¿Cómo construímos la imagen?
Ya tenemos el Dockerfile listo, ¿qué hacemos con él? Vamos a la terminal de nuestro computador y ejecutamos:
```
$ docker build -t mi_primera_app:v1 .
```
- `-t`: Es el "tag" (nombre y versión de la imagen)
- `.`: Es la carpeta actual de nuestro computador. Es vital, ya que le dice a Docker que el Dockerfile y los archivos de código están en la carpeta actual de nuestro computador.

### Pregunta: ¿Qué pasa si yo modifico una línea en app.py y luego vuelvo a ejecutar el comando `docker build`?

Docker aprovechará el caché de capas, por lo que solamente ejecutará lo que tuvo cambios. En nuestro caso, se volverá a copiar el código al contenedor y se iniciará la aplicación una vez creado el contenedor.

El caché de capas no es solo una función extra, es la clave para que el ciclo de desarrollo sea ágil. Si no lo usas bien, cada pequeño cambio en tu código te obligará a esperar minutos miestras se descargan e instalan dependencias una y otra vez.

### ¿Cómo funciona el caché de capas?

Cada instrucción en tu Dockerfile (`FROM`, `RUN`, `COPY`, etc.), crea una capa (una diferencia en el sistema de archivos). Estas capas se apilan una sobre otra para formar la imagen final.

Docker es extremadamente eficiente: antes de ejecutar una instrucción, mira si ya la ejecutó anteriormente con el mismo contenido y en el mismo orden.

Dcoker valida el caché de forma secuencial (de arriba hacia abajo).

Si una capa cambia, todas las capas que vienen después quedan invalidadas y Docker se ve obligador a ejecutarlas de nuevo desde cero.

El orden que vimos anteriormente es el estándar de la industria:

1. `FROM`: Rara vez cambia. Siempre está en caché.
2. `WORKDIR`: Rara vez cambia. Siempre trabajamos sobre la misma carpeta dentro del contenedor.
3. `COPY` (archivo de dependencias): Solo cambia si agregamos una librería nueva.
4. `RUN` (instalación de dependencias): Es el paso más lento. Lo bueno es que, si Docker ve que no hay cambios en el archivo de dependencias, entonces se saltará este paso.
5. `COPY` (copia del código fuente): Muy frecuente. Cada vez que editamos algo se ejecutará, por eso debe ir al final.

Si pusieramos la copia del código fuente antes de la copia del archivo de dependencias, cada vez que corrijamos algún error de nuestro código fuente Docker detectará el cambio. Como resultado, invalidará el caché de la instalación de librerías y tendremos que esparar a que pip descargue todo de nuevo.

## Archivo .dockerignore

En nuestra carpeta de proyecto generalmente hay archivos basura como: carpetas .git, archivos .env, carpetas como __pycache__, etc. Básicamente, carpetas que nos ayudan a desarrollar, pero no deben ser subidas a producción.

Si no utilizamos un .dockerignore, Docker copiará todas estas carpetas inútiles junto con las carpetas que sí tienen relevancia dentro de la imagen, lo que ocasiona dos problemas:
1. Seguridad: Podrías estar subiendo credenciales a producción sin querer.
2. Caché: Si cambia un archivo de log local Docker creerá que el código "cambió" y volverá a construir la imagen innecesariamente.

El archivo .dockerignore se crea en la **misma** carpeta del Dockerfile.
