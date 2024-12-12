-- Paso 1: Configuración inicial del servidor
Actualiza los paquetes del sistema:

sudo apt update && sudo apt upgrade -y



Instala un editor de texto (opcional):

sudo apt install nano



Asegúrate de que tienes un dominio local (centro.intranet): Modifica el archivo /etc/hosts en tu máquina local o red para resolver centro.intranet al servidor.

sudo nano /etc/hosts



Agrega la siguiente línea:

Copiar código
127.0.0.1 centro.intranet



-- Paso 2: Instalar y configurar el servidor web
Instalar un servidor LAMP (Linux, Apache, MySQL, PHP):

sudo apt install apache2 mysql-server php libapache2-mod-php php-mysql -y
Configurar Apache: Crea un archivo de configuración para el dominio centro.intranet:

sudo nano /etc/apache2/sites-available/centro.intranet.conf
Contenido del archivo:

<VirtualHost *:80>
    ServerName centro.intranet
    DocumentRoot /var/www/centro.intranet

    <Directory /var/www/centro.intranet>
        AllowOverride All
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/centro.intranet_error.log
    CustomLog ${APACHE_LOG_DIR}/centro.intranet_access.log combined
</VirtualHost>



Habilitar el sitio y mod_rewrite:

sudo a2ensite centro.intranet.conf
sudo a2enmod rewrite
sudo systemctl restart apache2



-- Paso 3: Configurar MySQL
Ejecuta el script de seguridad:

sudo mysql_secure_installation



Sigue las instrucciones y establece una contraseña segura.

Crea una base de datos y usuario para WordPress: Accede a MySQL:

sudo mysql -u root -p



Ejecuta los siguientes comandos:

CREATE DATABASE wordpress;
CREATE USER 'wordpressuser'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpressuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;



-- Paso 4: Instalar WordPress
Descarga WordPress:

wget https://wordpress.org/latest.tar.gz
tar -xzf latest.tar.gz
sudo mv wordpress /var/www/centro.intranet



Configura los permisos:

sudo chown -R www-data:www-data /var/www/centro.intranet
sudo chmod -R 755 /var/www/centro.intranet



Copia el archivo de configuración:

cd /var/www/centro.intranet
cp wp-config-sample.php wp-config.php
Edita wp-config.php:

sudo nano wp-config.php



Configura los valores de la base de datos:

define( 'DB_NAME', 'wordpress' );
define( 'DB_USER', 'wordpressuser' );
define( 'DB_PASSWORD', 'password' );
define( 'DB_HOST', 'localhost' );
define( 'DB_CHARSET', 'utf8' );
define( 'DB_COLLATE', '' );



-- Paso 5: Prueba el sitio
Abre tu navegador y visita:

http://centro.intranet



Sigue el asistente de instalación de WordPress para completar la configuración.

