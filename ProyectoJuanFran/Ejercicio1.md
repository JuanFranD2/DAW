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

5. Configura Virtual Hosts (Opcional)
Si planeas alojar varios sitios web, puedes configurar virtual hosts. Por ejemplo:

Crea un nuevo archivo de configuración para tu sitio web:

sudo nano /etc/apache2/sites-available/mi-sitio.conf

Añade la configuración básica:

<VirtualHost *:80>
    ServerName mi-sitio.com
    ServerAlias www.mi-sitio.com
    DocumentRoot /var/www/mi-sitio
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

Crea el directorio para tu sitio web y configura los permisos:

sudo mkdir /var/www/mi-sitio
sudo chown -R $USER:$USER /var/www/mi-sitio



Habilita el nuevo sitio y recarga Apache:

sudo a2ensite mi-sitio
sudo systemctl reload apache2



7. Prueba la Configuración
Para asegurarte de que Apache está configurado correctamente, ejecuta:

sudo apachectl configtest



Si todo está bien, deberías ver el mensaje:

Syntax OK

8. Reinicia Apache
Finalmente, reinicia el servicio para asegurarte de que los cambios están aplicados:

sudo systemctl restart apache2


