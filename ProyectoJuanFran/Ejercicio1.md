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

![image](https://github.com/user-attachments/assets/6e52dc9e-ef94-4b8c-897e-90412f375649)

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

![image](https://github.com/user-attachments/assets/71e636ee-6cee-487c-8e78-e80db27d2a0e)

Guarda y cierra el archivo.

3. Habilita el Nuevo Virtual Host
Habilita el nuevo sitio:

sudo a2ensite centro.intranet.conf

![image](https://github.com/user-attachments/assets/c8a8253a-c6ac-419c-9af0-4a14b1a294bc)

Deshabilita el sitio por defecto si no lo necesitas:

sudo a2dissite 000-default.conf

![image](https://github.com/user-attachments/assets/b1d9c812-4545-40ee-9224-c367edb2c491)

Habilita el módulo rewrite necesario para WordPress:

sudo a2enmod rewrite

![image](https://github.com/user-attachments/assets/3b04af56-9981-4aee-a37c-90089295714b)

Recarga Apache para aplicar los cambios:

sudo systemctl reload apache2

![image](https://github.com/user-attachments/assets/da63afbb-dd89-4335-9d31-2b72f01ba1b0)


4. Crea el Directorio para centro.intranet
Crea el directorio donde se alojará WordPress:

sudo mkdir -p /var/www/centro.intranet

![image](https://github.com/user-attachments/assets/7994fdc1-e445-4e3f-9f15-52f08ce29326)

Establece los permisos para que tu usuario pueda administrar los archivos:

sudo chown -R $USER:$USER /var/www/centro.intranet
sudo chmod -R 755 /var/www/centro.intranet

![image](https://github.com/user-attachments/assets/edd30823-5f9d-4018-bc65-cc9327487f25)

5. Descarga e Instala WordPress
Descarga WordPress:

wget https://wordpress.org/latest.tar.gz

![image](https://github.com/user-attachments/assets/6bad667b-718b-4796-9ec8-6c6d42ca9dac)

Extrae el archivo descargado:

tar -xvzf latest.tar.gz

![image](https://github.com/user-attachments/assets/1f94c01c-8991-4869-9b70-e7ba2578fca1)

Mueve los archivos de WordPress al directorio del dominio:

mv wordpress/* /var/www/centro.intranet

![image](https://github.com/user-attachments/assets/e0d3d908-8808-4bd0-b466-3c97177f8739)

Establece los permisos necesarios:

sudo chown -R www-data:www-data /var/www/centro.intranet
sudo chmod -R 755 /var/www/centro.intranet

![image](https://github.com/user-attachments/assets/54a38432-c662-4d89-8da3-76afacb21460)

6. Configura la Base de Datos para WordPress
Accede a MySQL:

sudo mysql

Crea una base de datos para WordPress:

CREATE DATABASE wordpress;

![image](https://github.com/user-attachments/assets/c06b4d72-eb8a-4904-8ca0-40f77840de8b)

Crea un usuario y dale permisos a la base de datos:

GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpress_user'@'localhost' IDENTIFIED BY 'password';
FLUSH PRIVILEGES;
EXIT;

![image](https://github.com/user-attachments/assets/efefd034-afc0-4551-8f63-e2c51ee0e02c)

7. Completa la Instalación de WordPress

Abre tu navegador y accede a http://centro.intranet.
Sigue los pasos de instalación de WordPress:
Introduce el nombre de la base de datos (wordpress).
El usuario de la base de datos (wordpress_user).
La contraseña (password).
Deja el campo "Servidor de la base de datos" como localhost.

![image](https://github.com/user-attachments/assets/94dd755a-0bfd-4274-b32b-103bdf49a6d9)

![image](https://github.com/user-attachments/assets/9fb13f93-cb93-4b47-8915-cbb7814a3d2d)

-- 1. Crear y Desplegar una Aplicación Python con Apache y mod_wsgi
1.1. Instalar mod_wsgi y Python:
Asegúrate de que Python y el módulo wsgi estén instalados:

sudo apt update
sudo apt install apache2 libapache2-mod-wsgi-py3 python3 python3-pip

![image](https://github.com/user-attachments/assets/bb88605c-b3c4-4726-af0d-7d10f82607d4)

1.2. Crear una Aplicación Python:
Crea una aplicación básica en /var/www/pythonapp:

sudo mkdir -p /var/www/pythonapp
sudo nano /var/www/pythonapp/app.py

Añade el siguiente contenido:

def application(environ, start_response):
    status = '200 OK'
    output = b"¡Hola! Esta es mi aplicación Python con WSGI."

    response_headers = [('Content-type', 'text/plain'),
                        ('Content-Length', str(len(output)))]
    start_response(status, response_headers)

    return [output]

    
![image](https://github.com/user-attachments/assets/8c1792d2-b09a-4f3e-ad32-84dbe478a5a9)    

1.3. Configurar Apache para la Aplicación:
Crea un archivo de configuración para el dominio:

sudo nano /etc/apache2/sites-available/pythonapp.conf
Añade esta configuración:

<VirtualHost *:80>
    ServerName python.centro.intranet
    WSGIScriptAlias / /var/www/pythonapp/app.py

    <Directory /var/www/pythonapp>
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/python_error.log
    CustomLog ${APACHE_LOG_DIR}/python_access.log combined
</VirtualHost>

![image](https://github.com/user-attachments/assets/a3d6ba09-5d1d-48b5-bed0-20c8b2bceda0)

Habilita el sitio y recarga Apache:

sudo a2ensite pythonapp.conf
sudo systemctl reload apache2

![image](https://github.com/user-attachments/assets/b81ed3a6-89b5-4614-b889-bd54637e558e)

1.4. Probar la Aplicación:
Asegúrate de que python.centro.intranet está en el archivo /etc/hosts:

127.0.0.1 python.centro.intranet

![image](https://github.com/user-attachments/assets/7bb510e2-1ad5-4963-b8e7-2f2297195a80)

Visita http://python.centro.intranet para comprobar que la aplicación funciona.

2. Proteger el Acceso con Autenticación
Crea un archivo .htpasswd con las credenciales:

sudo apt install apache2-utils
sudo htpasswd -c /etc/apache2/.htpasswd usuario

![image](https://github.com/user-attachments/assets/797b5019-3fd0-48da-83c0-34bd27d5d870)

Edita la configuración del sitio para habilitar autenticación:

sudo nano /etc/apache2/sites-available/pythonapp.conf

Añade estas líneas dentro del bloque <Directory>:

AuthType Basic
AuthName "Acceso Restringido"
AuthUserFile /etc/apache2/.htpasswd
Require valid-user

![image](https://github.com/user-attachments/assets/b197201e-8c7a-4705-b28f-9b3a641965af)

Recarga Apache:

sudo systemctl reload apache2

Visita http://python.centro.intranet y verifica que te solicita autenticación.

3. Instalar y Configurar AWStats
Instala AWStats:

sudo apt install awstats

![image](https://github.com/user-attachments/assets/bd0bb056-fad5-413d-b67f-0b527be67d00)

Configura AWStats para Apache:

Copia el archivo de configuración base:

sudo cp /etc/awstats/awstats.conf /etc/awstats/awstats.python.centro.intranet.conf

![image](https://github.com/user-attachments/assets/51faa553-ae9c-4efa-88cf-a30b6ee07a32)

Edita el archivo:

sudo nano /etc/awstats/awstats.python.centro.intranet.conf

Cambia las siguientes líneas:

LogFile="/var/log/apache2/access.log"
SiteDomain="python.centro.intranet"
HostAliases="localhost 127.0.0.1 python.centro.intranet"

![image](https://github.com/user-attachments/assets/65dc0e96-c552-4a2d-b9fd-ed44b519dab6)

Actualiza las estadísticas:
sudo /usr/lib/cgi-bin/awstats.pl -config=python.centro.intranet -update

![image](https://github.com/user-attachments/assets/a5ca44f5-6b4d-4fea-ac0f-b5c1da24986f)

Habilita el acceso a AWStats desde Apache:

sudo nano /etc/apache2/sites-available/pythonapp.conf
Añade:

Alias /awstats/icon /usr/share/awstats/icon/
ScriptAlias /awstats/ /usr/lib/cgi-bin/

![image](https://github.com/user-attachments/assets/6136425a-ec08-4f99-8980-c44580fc0b90)

Recarga Apache:

sudo systemctl reload apache2
Visita http://python.centro.intranet/awstats/awstats.pl.

4. Instalar y Configurar Nginx en el Puerto 8080
4.1. Instalar Nginx:

sudo apt install nginx

![image](https://github.com/user-attachments/assets/60f09e7e-de81-4555-816c-37dab99669fb)

4.2. Configurar Nginx para el Puerto 8080:
Edita el archivo de configuración:

sudo nano /etc/nginx/sites-available/servidor2

Añade:

server {
    listen 8080;
    server_name servidor2.centro.intranet;

    root /var/www/servidor2;
    index index.php index.html;

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.1-fpm.sock;
    }
}

![image](https://github.com/user-attachments/assets/58b7e738-8806-4906-85f9-38405836e8b3)

Habilita el sitio y reinicia Nginx:

sudo ln -s /etc/nginx/sites-available/servidor2 /etc/nginx/sites-enabled/
sudo systemctl restart nginx


5. Instalar y Configurar phpMyAdmin
Instala PHP y phpMyAdmin:

sudo apt install php php-fpm php-mysql phpmyadmin

![image](https://github.com/user-attachments/assets/35b4fb63-c6c7-41d7-8ac4-d4283a92721a)

Configura phpMyAdmin para Nginx:

Edita el archivo de configuración de Nginx (servidor2):

location /phpmyadmin {
    root /usr/share/;
    index index.php;
    location ~ ^/phpmyadmin/(.+\.php)$ {
        fastcgi_pass unix:/run/php/php8.1-fpm.sock;
        include snippets/fastcgi-php.conf;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }
    location ~* ^/phpmyadmin/(.+\.(jpg|jpeg|gif|css|js|ico|png|html|xml|txt))$ {
        root /usr/share/;
    }
}
Reinicia Nginx:

sudo systemctl restart nginx

Visita http://servidor2.centro.intranet:8080/phpmyadmin.

