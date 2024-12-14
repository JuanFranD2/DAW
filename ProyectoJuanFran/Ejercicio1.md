-- Instalo MySQL:
Para instalar MySQL en Ubuntu con el usuario root y sin contraseña, sigue estos pasos:

- 1. Actualiza el sistema:
Primero, asegúrate de que tu sistema esté actualizado. Abre la terminal y ejecuta:

sudo apt update
sudo apt upgrade

![image](https://github.com/user-attachments/assets/db3bddc5-f033-429f-a6a0-3a724b82ed0d)

- 2. Instala MySQL:
Instala el paquete de MySQL con el siguiente comando:

sudo apt install mysql-server

![image](https://github.com/user-attachments/assets/74a171b9-71ad-48b2-80f0-a894d0cb0ee2)

- 3. Configura MySQL para no usar contraseña (opcional):
Para configurar MySQL sin una contraseña para el usuario root, después de instalar MySQL, realiza lo siguiente:

- 3.1. Inicia el script de configuración de seguridad de MySQL:
Después de la instalación, MySQL puede pedirte que configures una contraseña para el usuario root. Si prefieres no tener contraseña, puedes omitir este paso, pero es recomendable hacerlo por razones de seguridad.

Para deshabilitar la contraseña, ejecuta:

sudo mysql_secure_installation

![image](https://github.com/user-attachments/assets/883f65e7-dd0d-404c-80a9-ba05d8aecb13)

Cuando te pregunte por la contraseña actual de root, deja el campo vacío (solo presiona Enter) y selecciona las opciones que te convengan.

3.2. Accede a MySQL como root:
Si decides no configurar una contraseña, podrás acceder a MySQL sin contraseña de la siguiente forma:

sudo mysql

![image](https://github.com/user-attachments/assets/01934bdd-15ed-435a-8a0c-c85e70921ef2)

4. Opcional: Cambia la autenticación del usuario root para no requerir contraseña:
Si MySQL aún requiere una contraseña para root, puedes cambiar el método de autenticación a uno sin contraseña. Esto se hace desde la consola de MySQL:

sudo mysql -u root
Luego, ejecuta este comando en MySQL:

USE mysql;
UPDATE user SET authentication_string=null WHERE user='root';
FLUSH PRIVILEGES;
EXIT;
- 5. Reinicia el servicio MySQL:
Para asegurarte de que los cambios se apliquen correctamente, reinicia el servicio de MySQL:

sudo systemctl restart mysql

![image](https://github.com/user-attachments/assets/8c1a4fca-dd16-480f-ad6f-3a86ea85100b)

- 6. Verifica:
Ahora podrás acceder a MySQL sin contraseña usando:

sudo mysql -u root

-- Para instalar Apache en un sistema Ubuntu, sigue estos pasos:

1. Actualiza los Paquetes del Sistema
Es importante asegurarte de que tu sistema está actualizado antes de instalar nuevos paquetes:

sudo apt update
sudo apt upgrade

![image](https://github.com/user-attachments/assets/5ac5266e-cd0f-43f5-837e-0bdc848dc134)

2. Instala Apache
Ejecuta el siguiente comando para instalar el servidor web Apache:

sudo apt install apache2

![image](https://github.com/user-attachments/assets/817d3a63-9ae4-41b4-9569-cb25b7a7cdbf)

Esto instalará Apache y habilitará automáticamente el servicio.

3. Verifica que Apache Está Corriendo
Una vez instalado, verifica que el servicio está funcionando:

sudo systemctl status apache2

![image](https://github.com/user-attachments/assets/e17891a0-9109-482d-8127-3b24409c66d1)

Deberías ver un mensaje indicando que el servicio está activo (active (running)).

4. Abre Apache en el Navegador
Para verificar que Apache está instalado y funcionando, abre tu navegador y accede a:

http://localhost

![image](https://github.com/user-attachments/assets/d66c0331-09be-4f75-9395-d538069f9b66)

Deberías ver la página predeterminada de Apache con el mensaje "It works!".

5. Prueba la Configuración
Para asegurarte de que Apache está configurado correctamente, ejecuta:

sudo apachectl configtest

![image](https://github.com/user-attachments/assets/ba9a58bc-ca66-43a3-85ee-1c4fe1074c0f)

Si todo está bien, deberías ver el mensaje:

Syntax OK

6. Reinicia Apache
Finalmente, reinicia el servicio para asegurarte de que los cambios están aplicados:

sudo systemctl restart apache2

-- Instalar Wordpress:
1. Configura el Archivo hosts
Abre el archivo hosts en tu sistema:

sudo nano /etc/hosts

Añade una línea que apunte el dominio centro.intranet a tu máquina local (127.0.0.1):

127.0.0.1 centro.intranet



Guarda los cambios (en Nano, usa Ctrl+O para guardar y Ctrl+X para salir).

2. Configura un Virtual Host para centro.intranet
Crea un archivo de configuración para el sitio web de WordPress en Apache:

sudo nano /etc/apache2/sites-available/centro.intranet.conf

Añade la configuración básica para el dominio:

apache
Copiar código
<VirtualHost *:80>
    ServerName centro.intranet
    DocumentRoot /var/www/centro.intranet

    <Directory /var/www/centro.intranet>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/centro_error.log
    CustomLog ${APACHE_LOG_DIR}/centro_access.log combined
</VirtualHost>



Guarda y cierra el archivo.

3. Habilita el Nuevo Virtual Host
Habilita el nuevo sitio:

sudo a2ensite centro.intranet.conf



Deshabilita el sitio por defecto si no lo necesitas:

sudo a2dissite 000-default.conf



Habilita el módulo rewrite necesario para WordPress:

sudo a2enmod rewrite



Recarga Apache para aplicar los cambios:

sudo systemctl reload apache2



4. Crea el Directorio para centro.intranet
Crea el directorio donde se alojará WordPress:

sudo mkdir -p /var/www/centro.intranet



Establece los permisos para que tu usuario pueda administrar los archivos:

sudo chown -R $USER:$USER /var/www/centro.intranet
sudo chmod -R 755 /var/www/centro.intranet



5. Descarga e Instala WordPress
Descarga WordPress:

wget https://wordpress.org/latest.tar.gz



Extrae el archivo descargado:

tar -xvzf latest.tar.gz



Mueve los archivos de WordPress al directorio del dominio:

mv wordpress/* /var/www/centro.intranet



Establece los permisos necesarios:

sudo chown -R www-data:www-data /var/www/centro.intranet
sudo chmod -R 755 /var/www/centro.intranet



6. Configura la Base de Datos para WordPress
Accede a MySQL:

mysql -u root



Crea una base de datos para WordPress:

CREATE DATABASE wordpress;



Crea un usuario y dale permisos a la base de datos:

GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpress_user'@'localhost' IDENTIFIED BY 'password';
FLUSH PRIVILEGES;
EXIT;
7. Completa la Instalación de WordPress

Abre tu navegador y accede a http://centro.intranet.
Sigue los pasos de instalación de WordPress:
Introduce el nombre de la base de datos (wordpress).
El usuario de la base de datos (wordpress_user).
La contraseña (password).
Deja el campo "Servidor de la base de datos" como localhost.


