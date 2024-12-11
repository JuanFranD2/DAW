1. Actualizar el sistema
Es una buena práctica actualizar los paquetes antes de comenzar la instalación. Abre una terminal y ejecuta el siguiente comando:

sudo apt update
sudo apt upgrade

![image](https://github.com/user-attachments/assets/9e6c2faa-45b8-40ca-bd2c-f8817f419a07)

2. Instalar Apache
Para instalar Apache, usa el siguiente comando:

sudo apt install apache2

![image](https://github.com/user-attachments/assets/e20817cb-94f7-41ee-9a4d-d957a22b6266)

Este comando instalará el paquete apache2 y todas sus dependencias.

3. Iniciar y habilitar Apache
Una vez instalado, puedes iniciar el servicio de Apache y habilitarlo para que se inicie automáticamente al arrancar el sistema:

sudo systemctl start apache2
sudo systemctl enable apache2

 ![image](https://github.com/user-attachments/assets/22624292-9da9-42d9-9d84-be35f0a24dd0)

4. Comprobar el estado del servicio
Verifica que Apache esté corriendo correctamente con:

sudo systemctl status apache2

![image](https://github.com/user-attachments/assets/86b545f2-f3f6-4adc-a26b-b973658bc2e7)

Si todo está bien, deberías ver algo como "active (running)".

5. Abrir el puerto 80 (si tienes un firewall habilitado)
Si tienes un firewall activo (por ejemplo, ufw), necesitas permitir el tráfico en el puerto 80 (HTTP):

sudo ufw allow 'Apache'
sudo ufw reload

![image](https://github.com/user-attachments/assets/efe9f3a7-f0b5-4dde-a9b6-abbd0eaa5ed1)

6. Verificar la instalación
Abre un navegador y accede a la dirección IP de tu servidor o a localhost si estás trabajando en una máquina local:

http://localhost/

![image](https://github.com/user-attachments/assets/3b086304-3e0f-4385-bad3-3423137ba56c)

Deberías ver la página predeterminada de Apache, que indica que el servidor web está funcionando correctamente.

---------------------------------------------

Para configurar ambos dominios en tu máquina utilizando el archivo hosts, sigue estos pasos:

1. Editar el archivo hosts
Primero, necesitas mapear ambos dominios a tu IP local (si estás trabajando en un entorno local). Para hacerlo, edita el archivo hosts:

sudo nano /etc/hosts

Agrega las siguientes líneas al final del archivo (suponiendo que el servidor sea local, puedes usar 127.0.0.1):

Copiar código
127.0.0.1   centro.intranet
127.0.0.1   departamentos.centro.intranet

->

Guarda y cierra el archivo.

2. Configurar Apache para ambos dominios
Apache debe estar configurado para servir diferentes sitios según el dominio solicitado. Para hacerlo, vamos a crear dos archivos de configuración de sitios virtuales: uno para WordPress (en centro.intranet) y otro para la aplicación en Python (en departamentos.centro.intranet).

a) Configurar el sitio para WordPress
Crear el directorio para WordPress:

sudo mkdir -p /var/www/centro.intranet
Descargar e instalar WordPress (si no lo has hecho ya):

cd /var/www/centro.intranet
wget https://wordpress.org/latest.tar.gz
tar -xvzf latest.tar.gz
sudo chown -R www-data:www-data /var/www/centro.intranet
Crear el archivo de configuración para Apache:

sudo nano /etc/apache2/sites-available/centro.intranet.conf

Agrega lo siguiente en el archivo:
Copiar código
<VirtualHost *:80>
    ServerAdmin webmaster@centro.intranet
    ServerName centro.intranet
    DocumentRoot /var/www/centro.intranet/wordpress

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    <Directory /var/www/centro.intranet/wordpress>
        AllowOverride All
    </Directory>
</VirtualHost>

->

Habilitar el sitio:

sudo a2ensite centro.intranet.conf
sudo systemctl reload apache2

->

b) Configurar el sitio para la aplicación Python
Crear el directorio para la aplicación:

sudo mkdir -p /var/www/departamentos.centro.intranet

->

Configurar un VirtualHost para servir la aplicación Python. Supongamos que tienes una aplicación Python corriendo con Flask o Django, por ejemplo.

Si usas Flask o un servidor WSGI, necesitarás configurar Apache con mod_wsgi. Vamos a suponer que tu aplicación se ejecuta con Flask:

Instalar mod_wsgi:

sudo apt install libapache2-mod-wsgi-py3

->

Crear el archivo de configuración de Apache para la aplicación Python:

sudo nano /etc/apache2/sites-available/departamentos.centro.intranet.conf

Agrega lo siguiente:

Copiar código
<VirtualHost *:80>
    ServerAdmin webmaster@departamentos.centro.intranet
    ServerName departamentos.centro.intranet
    DocumentRoot /var/www/departamentos.centro.intranet

    WSGIDaemonProcess app user=www-data group=www-data threads=5
    WSGIScriptAlias / /var/www/departamentos.centro.intranet/app.wsgi

    <Directory /var/www/departamentos.centro.intranet>
        WSGIProcessGroup app
        WSGIApplicationGroup %{GLOBAL}
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

->

Asegúrate de que el archivo .wsgi apunte correctamente a tu aplicación Python.

Habilitar el sitio:

sudo a2ensite departamentos.centro.intranet.conf
sudo systemctl reload apache2

->

3. Probar la configuración
Ahora deberías poder acceder a ambos sitios desde tu navegador:

http://centro.intranet debería mostrar la página de WordPress.
http://departamentos.centro.intranet debería servir tu aplicación Python.

->

4. Asegurarte de que Apache está funcionando correctamente
Finalmente, puedes verificar el estado de Apache para asegurarte de que todo esté en orden:

sudo systemctl status apache2

Y si necesitas reiniciar Apache después de hacer cambios:

sudo systemctl restart apache2

->
