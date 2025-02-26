- Primero no s aseguramos que tenemos la VPC perfectamente creada.

![image](https://github.com/user-attachments/assets/942d4fe1-b152-481f-a13d-138a0adca463)

- Lanzamos la instancia en EC2

![image](https://github.com/user-attachments/assets/49691a32-54c5-43d6-87c6-1c4835c974b5)

- Le asignamos "servidorwordpress" a la nueva instancia y seleccionamod en Inicio Rápido "Ubuntu"

![image](https://github.com/user-attachments/assets/2ad6f102-3e86-41fc-8403-af7667c42581)

- En Par de claves creamos una nueva clave con el nombre de "servidorSSH"

![image](https://github.com/user-attachments/assets/cccba2c6-e099-4a87-a2c7-de15a94ce891)

- En configuración de red asignamos el VPC "wizar" y su subred "wizard-subnet-public1-us-east-1a", le asignamos al nombre del grupo "seguridadwordpress"

![image](https://github.com/user-attachments/assets/aa9d5946-2656-49ea-ba61-4e8d1f3aa9e6)

- Creamos una nueva regla "HTTP"

![image](https://github.com/user-attachments/assets/1ed5ad8e-bb5d-4728-8469-7aa684d4c994)

- En Detalles avanzados vamos a realizar todas las instalaciones necesarias para poder trabajar con wordpress en Ubuntu

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

- Comprobamos que se ha creado correctamente la instancia y se encuentra en ejecución

![image](https://github.com/user-attachments/assets/45da52b5-3f57-4ae6-b424-12f8c107d6ed)

- Procedemos a conectarnos por nuestra consola a la instancia creada, para ello hacemos clic en la instancia y conectar

![image](https://github.com/user-attachments/assets/f817a874-d2ab-41a1-82e7-ff8a56c17584)

- En la pestaña "Cliente SSH", copiamos el código que viene en ejemplo: ssh -i "servidorSSH.pem" ubuntu@ec2-34-204-18-130.compute-1.amazonaws.com

![image](https://github.com/user-attachments/assets/d719fc9f-0ad5-43b5-a965-25fb25ec923d)

- Ejecutamos nuestro cmd como administrador y añadimos la ruta de nuestro archivo clave descargada previamente

![image](https://github.com/user-attachments/assets/df59e2e3-1653-41e3-8b3d-ee7add9a7a13)

- Conseguimos entrar exitosamente

![image](https://github.com/user-attachments/assets/a5ce7680-9d19-4c5d-b098-ca9e40eea098)

- Comprobamos que todo se haya instalado correctamente

-- Apache instalado correcctamente

![image](https://github.com/user-attachments/assets/9f64f5ab-80b1-40c1-9b9c-4c440e4ce865)

-- MariaDB instalado correctamente

![image](https://github.com/user-attachments/assets/c47cb128-7da0-42be-ac4c-484b8def29be)

-- Wordpress instalado correctamente

![image](https://github.com/user-attachments/assets/464b70e3-5b43-46ff-8835-a6076eef82cc)

- Procedemos a crear la base de datos en RDS

![image](https://github.com/user-attachments/assets/bb13cfe8-b970-4365-8158-970ea9701da9)

- La queremos de creación sencilla y de MariaDB
![image](https://github.com/user-attachments/assets/ee5b11aa-7fd0-4696-a93a-f8a1e3bfa2ca)

- Asignamos capa gratuita, nombre de labdd (bdwordpress), admin y contraseña ("12345678")

- ![image](https://github.com/user-attachments/assets/4c62aeab-69bf-4994-939a-787f28c14c2d)

- Configuramos 
