Para realizar la práctica Docker 2, vamos a proceder con:
- Ejecutamos la imagen "hello-world" Abre una terminal y ejecuta el siguiente comando para correr la imagen hello-world. Este comando descarga una imagen de prueba y la ejecuta en un contenedor, lo que te permite ver si Docker está correctamente instalado y funcionando.
-- docker run hello-world

- Muestra las imágenes Docker instaladas Para ver las imágenes que tienes instaladas en tu sistema, utiliza el comando:
-- docker images

- Muestra los contenedores Docker Para ver los contenedores activos, utiliza:
-- docker ps

- Y para ver todos los contenedores, incluyendo los inactivos:
-- docker ps -a

Práctica del segundo artículo: Aplicaciones
- Edita el fichero Dockerfile Crea un archivo Dockerfile en un directorio nuevo. Usamos un editor de texto para escribir lo siguiente en el archivo:

# Utiliza una imagen base oficial de Python
FROM python:3.8-slim

# Establece el directorio de trabajo
WORKDIR /app

# Copia los archivos necesarios para la aplicación
COPY . /app

# Instala las dependencias de la aplicación
RUN pip install --trusted-host pypi.python.org Flask

# Expone el puerto donde se ejecutará la aplicación
EXPOSE 80

# Define el comando para iniciar la aplicación
ENV NAME World
CMD ["python", "app.py"]

- Construye el contenedor En el directorio donde tienes el Dockerfile, ejecuta:

-- docker build -t tu-usuario/hola-mundo .
Este comando construye la imagen con el nombre tu-usuario/hola-mundo basándose en el Dockerfile del directorio actual (.).

- Lo ejecutamos para correr la imagen en un contenedor, utilizamos:
-- docker run -p 4000:80 tu-usuario/hola-mundo

Creamos una cuenta en hub.docker.com visitando Docker Hub e ingresamos una cuenta siguiendo las instrucciones del sitio.

- Publica tu imagen Primero, inicia sesión en Docker Hub desde tu terminal:

docker login
Luego, sube tu imagen a Docker Hub:

docker push tu-usuario/hola-mundo
