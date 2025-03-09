Ejemplo 1: Construcción de imágenes con una una aplicación Python
En este ejemplo vamos a construir una imagen para servir una aplicación escrita en Python utilizando el framework flask. La aplicación será servida en el puerto 3000/tcp. Puedes encontrar los ficheros en este directorio del repositorio.

En el contexto vamos a tener el fichero Dockerfile y un directorio, llamado app con nuestra aplicación.

En este caso vamos a usar una imagen base de un sistema operativo sin ningún servicio. El fichero Dockerfile será el siguiente:

# syntax=docker/dockerfile:1
FROM debian:12
RUN apt-get update && apt-get install -y python3-pip  && apt-get clean && rm -rf /var/lib/apt/lists/*
WORKDIR /usr/share/app
COPY app .
RUN pip3 install --no-cache-dir --break-system-packages -r requirements.txt
EXPOSE 3000
CMD python3 app.py
Algunas consideraciones:

Sólo tenemos que instalar pip, que utilizaremos posteriormente para instalar los paquetes Python.
Copiamos nuestra aplicación en cualquier directorio.
Con WORKDIR nos posicionamos en el directorio indicado. Todas las instrucciones posteriores se realizarán sobre ese directorio.
Instalamos los paquetes python con pip, que están listados en el fichero requirements.txt.
El proceso que se va a ejecutar por defecto al iniciar el contenedor será python3 app.py que arranca un servidor web en el puerto 3000/tcp ofreciendo la aplicación.
Para crear la imagen ejecutamos:

$ docker build -t juanfran/ejemplo1111:v1 .
Comprobamos que la imagen se ha creado:

![image](https://github.com/user-attachments/assets/22d2093a-1fb7-463e-8294-7b953c7f6fbb)

$ docker images
REPOSITORY             TAG                 IMAGE ID            CREATED             SIZE
juanfran/ejemplo1111     v1                  8c3275799063        1 minute ago      226MB

![image](https://github.com/user-attachments/assets/2b4f11ca-42f1-4d47-809c-dbd55ceff018)

Y podemos crear un contenedor:
$ docker run -d -p 80:3000 --name ejemplo2 juanfran/ejemplo1111:v1
Y acceder con el navegador a nuestra página:

ejemplo3

Versión 2: Desde una imagen con python instalado
En este caso el dichero Dockerfile podría ser de esta manera:

# syntax=docker/dockerfile:1
FROM python:3.12.1-bookworm
WORKDIR /usr/share/app
COPY app .
RUN pip install --no-cache-dir -r requirements.txt
EXPOSE 3000
CMD python app.py


