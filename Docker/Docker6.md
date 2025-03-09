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

![image](https://github.com/user-attachments/assets/858b42e6-bee7-4478-bd68-1b905b173873)

Y acceder con el navegador a nuestra página:

![image](https://github.com/user-attachments/assets/3556400b-5b09-4a5c-9f1b-c8d394d9740a)

Ejemplo 2: Construcción de imágenes configurables con variables de entorno
En este último ejemplo vamos a construir una imagen de una aplicación PHP que necesita conectarse a una base de datos mariadb para guardar o leer información. Por lo tanto, vamos a construir la imagen para que podamos indicar variables de entorno para configurar las credenciales de acceso a la base de datos. Puedes encontrar los ficheros en este directorio del repositorio.

Aplicación PHP
Como ejemplo vamos a "dockerizar" una aplicación PHP simple que accede a una tabla de una base de datos. La aplicación la puedes encontrar en el directorio build/app/index.php.

Algunas cosas que hay que tener en cuenta:

Cuando programamos una aplicación tenemos que tener en cuenta que va a ser implantada usando Docker tenemos que hacer algunas modificaciones, por ejemplo en este caso, las credenciales para el acceso a la base de datos la leemos de variables de entorno (que posteriormente serán creadas en el contenedor):
<?php
 // Database host
 $host = getenv('DB_HOST');
 // Database user name
 $user = getenv('DB_USER');
 //Database user password
 $pass = getenv('DB_PASS');
 //Database name
 $db = getenv('DB_NAME');
 // check the MySQL connection status
 $conn = new mysqli($host, $user, $pass,$db);
 if ($conn->connect_error) {
     die("Connection failed: " . $conn->connect_error);
 } else {
     $sql = 'SELECT * FROM users';
     
     if ($result = $conn->query($sql)) {
         while ($data = $result->fetch_object()) {
             $users[] = $data;
         }
     }
     
     foreach ($users as $user) {
        echo "<br>";
        echo $user->username . " " . $user->password;
        echo "<br>";
    }
 }
 mysqli_close($conn);
 ?>
En el fichero schema.sql encontramos las instrucciones sql necesarias para inicializar la base de datos.
Configurar nuestra aplicación con variables de entorno
En los casos en que necesitamos modificar algo en la aplicación o hacer algún proceso en el momento de crear el contenedor, lo que hacemos es crear un script en bash que meteremos en la imagen y que será la instrucción que indiquemos en el CMD. Este script en concreto hará las siguiente operaciones:

Utilizando el fichero schema.sql (que también guardaremos en la imagen) inicializará la base de datos.
Ejecutar el servidor web en segundo plano.
En el directorio de trabajo encontramos:

build: Será el contexto necesario para crear la imagen de la aplicación.
El fichero docker-compose.yaml: Para crear el escenario.
El contexto (directorio build)
En el directorio de contexto tendremos tres ficheros:

Fichero script.sh
El fichero script.sh que se guardará en la imagen y se ejecutará con al iniciar el contenedor. Su contenido es el siguiente:

#!/bin/bash
while ! mysql -u ${DB_USER} -p${DB_PASS} -h ${DB_HOST}  -e ";" ; do
	sleep 1
done	
mysql -u ${DB_USER} -p${DB_PASS} -h ${DB_HOST} ${DB_NAME} < /opt/schema.sql
apache2ctl -D FOREGROUND
Esperamos hasta que la base de datos esté disponible, inicializamos la base de datos con el fichero schema.sql y finalmente iniciamos el servidor web.

Fichero schema.sql
Son las instrucciones sql que nos permiten crear la tabla necesaria en la base de datos.

Fichero Dockerfile
El ficheroDockerfile sería el siguiente:

# syntax=docker/dockerfile:1
FROM php:7.4-apache
RUN apt-get update && apt-get install -y mariadb-client
RUN docker-php-ext-install mysqli && docker-php-ext-enable mysqli
COPY app /var/www/html/
EXPOSE 80
ENV DB_USER user1
ENV DB_PASS asdasd
ENV DB_NAME usuarios
ENV DB_HOST mariadb
COPY script.sh /usr/local/bin/script.sh
COPY schema.sql /opt
RUN chmod +x /usr/local/bin/script.sh
CMD /usr/local/bin/script.sh
Algunas observaciones:

Creamos la imagen desde una imagen PHP.
Instalamos el cliente de mariadb que nos hará falta para inicializar la base de datos desde nuestro contenedor de la aplicación.
Siguiendo la documentación de la imagen oficial de PHP instalamos el módulo PHP mysqli.
Copiamos la aplicación en el servidor web.
Creamos las variables de entorno y le damos valores por defecto, por si no se indican en la creación del contenedor.
Copiamos los ficheros del script y del esquema de la base de datos a la imagen. Y le damos permisos de ejecución a script.sh.
Finalmente indicamos con CMD el comando que se va a ejecutar al iniciar el contenedor. En este caso ejecutaremos el script.
Creación de la imagen
Ejecutamos dentro del directorio de contexto:

![image](https://github.com/user-attachments/assets/508ebe72-bf77-4fbe-afbd-6321ef0382e5)

$ docker build -t josedom24/aplicacion_php .

![image](https://github.com/user-attachments/assets/d5d256fd-c43f-48df-97dd-3dc945a378e5)

Despliegue de la aplicación
Usaremos el fichero docker-compose.yaml:

version: '3.1'
services:
  app:
    container_name: contenedor_php
    image: josedom24/aplicacion_php
    restart: always
    environment:
      DB_HOST: servidor_mysql
      DB_USER: user1
      DB_PASS: asdasd
      DB_NAME: usuarios
    ports:
      - 8080:80
    depends_on:
      - db
  db:
    container_name: servidor_mysql
    image: mariadb
    restart: always
    environment:
      MYSQL_DATABASE: usuarios
      MYSQL_USER: user1
      MYSQL_PASSWORD: asdasd
      MYSQL_ROOT_PASSWORD: asdasd
    volumes:
      - mariadb_data:/var/lib/mysql
volumes:
    mariadb_data:
Y ya podemos levantar el escenario, ejecutando:

$ docker compose up -d

![image](https://github.com/user-attachments/assets/d53296ab-5387-45d6-8f97-8381cc164f26)

![image](https://github.com/user-attachments/assets/c33142df-9a42-4f5f-ad7b-916d8abb2f84)

Y finalmente podemos acceder a la aplicación y comprobar que funciona.

![image](https://github.com/user-attachments/assets/8cc6a5d4-7ccc-4734-b635-b81f3724beda)

