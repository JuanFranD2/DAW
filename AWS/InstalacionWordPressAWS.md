- Primero no s aseguramos que tenemos la VPC perfectamente creada.

![image](https://github.com/user-attachments/assets/942d4fe1-b152-481f-a13d-138a0adca463)

- Lanzamos la instancia en EC2

![image](https://github.com/user-attachments/assets/49691a32-54c5-43d6-87c6-1c4835c974b5)

- Modificamos el nombre del servidor, el tipo(Ubuntu).

![image](https://github.com/user-attachments/assets/f72cd08f-5bca-4eb0-a5fe-3841ef0e3d3a)

- Creamos una clave para conectarnos remotamente a AWS.

![image](https://github.com/user-attachments/assets/8e9975c8-a8e9-4aa5-a967-c0fd1c1455d6)

- Modificamos la configuración de red con los parámetros indicados.

![image](https://github.com/user-attachments/assets/053b830c-3c4a-4394-9116-e22af200f680)

![image](https://github.com/user-attachments/assets/1269c9e6-79ae-4706-981d-745b5387fefa)

- Uso una instalacion de WordPress directamente por comandos en Detalles avanzados, Datos de usuario:

#!/bin/bash
# Actualizar los paquetes del sistema:
sudo apt update -y
sudo apt upgrade -y
# Instalar Apache, PHP y las extensiones necesarias:
sudo apt install apache2 php php-mysql libapache2-mod-php php-cli php-curl php-gd php-mbstring php-xml php-xmlrpc php-zip -y
# cliente mariadb
sudo apt install mariadb-client-core -y
# descargar e instalar WordPress
cd /var/www/html
sudo wget https://wordpress.org/latest.tar.gz
sudo tar -xvzf latest.tar.gz
sudo mv wordpress/*.
sudo rm -rf wordpress latest.tar.gz
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html

![image](https://github.com/user-attachments/assets/9df111d7-713c-45ac-b65d-401b9d95a240)

- Como podemos comprobar, la instancia se ha creado correctamente:

![image](https://github.com/user-attachments/assets/5f14c6ec-ddd1-4e54-9260-ccb5a76802ec)

![image](https://github.com/user-attachments/assets/2356b493-c9ad-44f7-a0e3-cb663781f7bf)

- Usamos la clave que nos da la instancia para conectarnosremotamente a la instancia.

![image](https://github.com/user-attachments/assets/4c33da39-7f88-43ad-8dbf-3cd2f4a3adf5)

