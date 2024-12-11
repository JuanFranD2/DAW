1. Actualizar el sistema
Es una buena práctica actualizar los paquetes antes de comenzar la instalación. Abre una terminal y ejecuta el siguiente comando:

bash
Copiar código
sudo apt update
sudo apt upgrade
2. Instalar Apache
Para instalar Apache, usa el siguiente comando:

bash
Copiar código
sudo apt install apache2
Este comando instalará el paquete apache2 y todas sus dependencias.

3. Iniciar y habilitar Apache
Una vez instalado, puedes iniciar el servicio de Apache y habilitarlo para que se inicie automáticamente al arrancar el sistema:

bash
Copiar código
sudo systemctl start apache2
sudo systemctl enable apache2
4. Comprobar el estado del servicio
Verifica que Apache esté corriendo correctamente con:

bash
Copiar código
sudo systemctl status apache2
Si todo está bien, deberías ver algo como "active (running)".

5. Abrir el puerto 80 (si tienes un firewall habilitado)
Si tienes un firewall activo (por ejemplo, ufw), necesitas permitir el tráfico en el puerto 80 (HTTP):

bash
Copiar código
sudo ufw allow 'Apache'
sudo ufw reload
Si usas ufw, puedes verificar que el tráfico HTTP esté permitido con:

bash
Copiar código
sudo ufw status
6. Verificar la instalación
Abre un navegador y accede a la dirección IP de tu servidor o a localhost si estás trabajando en una máquina local:

arduino
Copiar código
http://localhost/
Deberías ver la página predeterminada de Apache, que indica que el servidor web está funcionando correctamente.

---------------------------------------------

Para configurar ambos dominios en tu máquina utilizando el archivo hosts, sigue estos pasos:

1. Editar el archivo hosts
Primero, necesitas mapear ambos dominios a tu IP local (si estás trabajando en un entorno local). Para hacerlo, edita el archivo hosts:

bash
Copiar código
sudo nano /etc/hosts
Agrega las siguientes líneas al final del archivo (suponiendo que el servidor sea local, puedes usar 127.0.0.1):

Copiar código
127.0.0.1   centro.intranet
127.0.0.1   departamentos.centro.intranet
Guarda y cierra el archivo.

2. Configurar Apache para ambos dominios
Apache debe estar configurado para servir diferentes sitios según el dominio solicitado. Para hacerlo, vamos a crear dos archivos de configuración de sitios virtuales: uno para WordPress (en centro.intranet) y otro para la aplicación en Python (en departamentos.centro.intranet).

a) Configurar el sitio para WordPress
Crear el directorio para WordPress:

bash
Copiar código
sudo mkdir -p /var/www/centro.intranet
Descargar e instalar WordPress (si no lo has hecho ya):

bash
Copiar código
cd /var/www/centro.intranet
wget https://wordpress.org/latest.tar.gz
tar -xvzf latest.tar.gz
sudo chown -R www-data:www-data /var/www/centro.intranet
Crear el archivo de configuración para Apache:

bash
Copiar código
sudo nano /etc/apache2/sites-available/centro.intranet.conf
Agrega lo siguiente en el archivo:

apache
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
Habilitar el sitio:

bash
Copiar código
sudo a2ensite centro.intranet.conf
sudo systemctl reload apache2
b) Configurar el sitio para la aplicación Python
Crear el directorio para la aplicación:

bash
Copiar código
sudo mkdir -p /var/www/departamentos.centro.intranet
Configurar un VirtualHost para servir la aplicación Python. Supongamos que tienes una aplicación Python corriendo con Flask o Django, por ejemplo.

Si usas Flask o un servidor WSGI, necesitarás configurar Apache con mod_wsgi. Vamos a suponer que tu aplicación se ejecuta con Flask:

Instalar mod_wsgi:

bash
Copiar código
sudo apt install libapache2-mod-wsgi-py3
Crear el archivo de configuración de Apache para la aplicación Python:

bash
Copiar código
sudo nano /etc/apache2/sites-available/departamentos.centro.intranet.conf
Agrega lo siguiente:

apache
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
Asegúrate de que el archivo .wsgi apunte correctamente a tu aplicación Python.

Habilitar el sitio:

bash
Copiar código
sudo a2ensite departamentos.centro.intranet.conf
sudo systemctl reload apache2
3. Probar la configuración
Ahora deberías poder acceder a ambos sitios desde tu navegador:

http://centro.intranet debería mostrar la página de WordPress.
http://departamentos.centro.intranet debería servir tu aplicación Python.
4. Asegurarte de que Apache está funcionando correctamente
Finalmente, puedes verificar el estado de Apache para asegurarte de que todo esté en orden:

sudo systemctl status apache2

Y si necesitas reiniciar Apache después de hacer cambios:

sudo systemctl restart apache2

->
