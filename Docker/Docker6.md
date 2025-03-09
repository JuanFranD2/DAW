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
