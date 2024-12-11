
En Ubuntu, puedes instalar y configurar WordPress siguiendo estos pasos:

-- Paso 1: Configurar el servidor
Instalar LAMP (Linux, Apache, MySQL/MariaDB, PHP): Ejecuta los siguientes comandos para instalar los componentes necesarios:

sudo apt update
sudo apt install apache2
sudo apt install mysql-server
sudo apt install php libapache2-mod-php php-mysql php-cli php-curl php-gd php-xml php-mbstring unzip curl

![image](https://github.com/user-attachments/assets/ab42caa8-a735-4d6f-a858-5d40950532b1)

Configurar Apache: Habilita el módulo rewrite y reinicia Apache:

sudo a2enmod rewrite
sudo systemctl restart apache2

![image](https://github.com/user-attachments/assets/88a19a3a-2f04-4d7f-a507-f8a5b6f11c3d)

-- Paso 2: Configurar la base de datos
Acceder al servidor MySQL:

sudo mysql

![image](https://github.com/user-attachments/assets/a5593812-f19c-4377-a331-9a681f384fbf)

Crear una base de datos para WordPress:

CREATE DATABASE wordpress;
CREATE USER 'wordpressuser'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpressuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
Cambia password por una contraseña segura.

![image](https://github.com/user-attachments/assets/223335b9-6c14-4d88-9847-a577905a712d)

-- Paso 3: Descargar WordPress
Ve al directorio raíz del servidor web:

cd /var/www/html

![image](https://github.com/user-attachments/assets/5f694b7f-3c1f-4952-8840-4ee710937681)

Descarga WordPress:

sudo curl -O https://wordpress.org/latest.tar.gz

![image](https://github.com/user-attachments/assets/8f40f697-8afc-40cb-8e16-b646658415b3)

Extrae el archivo:

sudo tar -xzvf latest.tar.gz

![image](https://github.com/user-attachments/assets/84225842-91e3-4610-b45a-9b24ff0eaca8)

Mueve los archivos de WordPress al directorio raíz:

sudo mv wordpress/* .



Configura los permisos:

sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html

-- Paso 4: Configurar WordPress
Copia el archivo de configuración de ejemplo:

sudo cp wp-config-sample.php wp-config.php

->

Edita el archivo de configuración:

sudo nano wp-config.php

->

Cambia las siguientes líneas con los detalles de la base de datos:

define('DB_NAME', 'wordpress');
define('DB_USER', 'wordpressuser');
define('DB_PASSWORD', 'password');
define('DB_HOST', 'localhost');
Guarda y cierra el archivo (Ctrl + O, Enter, Ctrl + X).

-- Paso 5: Completar la instalación
Abre un navegador y accede a tu dominio o IP del servidor:

plaintext
Copiar código
http://tu_dominio_o_ip/
Sigue las instrucciones del asistente de instalación:

Elige el idioma.
Configura el nombre del sitio, usuario administrador, contraseña y correo electrónico.
Haz clic en "Instalar WordPress".

-- Paso 6: Ajustes finales
Acceso al panel de administración: Ve a http://tu_dominio_o_ip/wp-admin/ para iniciar sesión con las credenciales creadas.

Opcional: Instalar Certificado SSL (HTTPS): Instala Certbot y configura SSL con Let's Encrypt:

sudo apt install certbot python3-certbot-apache
sudo certbot --apache

->

¡Listo! Ahora tienes WordPress instalado y configurado en tu servidor Ubuntu.
