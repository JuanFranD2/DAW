Para realizar la práctica Docker 2, vamos a proceder con:

Práctica del primer artículo:

- Ejecutamos la imagen "hello-world" Abrimos una terminal y ejecutamos el siguiente comando para correr la imagen hello-world. Este comando descarga una imagen de prueba y la ejecuta en un contenedor, lo que nos permite ver si Docker está correctamente instalado y funcionando.
-- docker run hello-world

![image](https://github.com/user-attachments/assets/9fb8171a-5fb8-409f-9d8e-00a2c0016bed)

- Mostramos las imágenes Docker instaladas para ver las imágenes que tenemos instaladas en el sistema, utilizamos el comando:
-- docker images

![image](https://github.com/user-attachments/assets/2f98c7cc-8fe8-43af-8035-20674c66fcba)

- Mostramos los contenedores Docker para ver los contenedores activos, utilizamos:
-- docker ps

![image](https://github.com/user-attachments/assets/47384a46-d1fd-4b05-a903-6d9b1266f793)

- Y para ver todos los contenedores, incluyendo los inactivos:
-- docker ps -a

![image](https://github.com/user-attachments/assets/40370035-3d8a-4302-87a9-a7c514c0b0fa)

Práctica del segundo artículo: Aplicaciones

- Editamos el fichero Dockerfile para ello creamos un archivo Dockerfile en un directorio nuevo. Usamos un editor de texto para escribir lo siguiente en el archivo:

-- Utiliza una imagen base oficial de Python
FROM python:3.8-slim

-- Establece el directorio de trabajo
WORKDIR /app

-- Copia los archivos necesarios para la aplicación
COPY . /app

-- Instala las dependencias de la aplicación
RUN pip install --trusted-host pypi.python.org Flask

-- Expone el puerto donde se ejecutará la aplicación
EXPOSE 80

# Define el comando para iniciar la aplicación
ENV NAME World
CMD ["python", "app.py"]

![image](https://github.com/user-attachments/assets/163a6475-ed6a-4cf2-b9d5-983771ce70d4)

- Construye el contenedor En el directorio donde tienes el Dockerfile, ejecuta:
-- docker build -t juanfran97/hola-mundo .
  
![image](https://github.com/user-attachments/assets/4ff06955-8e10-4722-9e3d-7e4d3030b7ec)

Este comando construye la imagen con el nombre tu-usuario/hola-mundo basándose en el Dockerfile del directorio actual (.).

- Lo ejecutamos para correr la imagen en un contenedor, utilizamos:
-- docker run -p 4000:80 juanfran97/hola-mundo

![image](https://github.com/user-attachments/assets/bd4726d4-3948-470a-8a11-1dea8a20aa56)

Creamos una cuenta en hub.docker.com visitando Docker Hub e ingresamos una cuenta siguiendo las instrucciones del sitio.

- Publica tu imagen Primero, inicia sesión en Docker Hub desde tu terminal:
-- docker login

![image](https://github.com/user-attachments/assets/a6f18842-f356-47d7-8b3f-f50131459062)

- Luego, sube tu imagen a Docker Hub:
-- docker push juanfran97/hola-mundo

![image](https://github.com/user-attachments/assets/d779bc90-5955-4697-b00d-70a38c45c295)

![image](https://github.com/user-attachments/assets/28abe6a1-3b5c-4080-9e5d-4733b70e6cb2)
