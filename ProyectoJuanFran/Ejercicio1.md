-- Instalo el servidor web Apache:
- Actualizo los paquetes y repositorios:
sudo apt update && sudo apt upgrade -y

![image](https://github.com/user-attachments/assets/f1970258-9c8e-4e69-a8bc-7e32d7aa590d)

- Instalo Apache:
sudo apt install apache2 -y

![image](https://github.com/user-attachments/assets/b0a42599-7a04-44d2-a5e3-85d62c6f75df)

- Edito el archivo hosts para incluir los dominios:
sudo nano /etc/hosts
- Añado las siguientes líneas:
127.0.0.1   centro.intranet
127.0.0.1   departamentos.centro.intranet

  ![image](https://github.com/user-attachments/assets/c90280cf-a51b-454a-a1ab-8f1d5900ae92)

- Reinicio Apache:
sudo systemctl restart apache2

![image](https://github.com/user-attachments/assets/9625ee4f-53ee-49b0-9c25-0964e2ca9ef2)

-- Activo los módulos para PHP y acceso a MySQL

- Instalo PHP y módulos necesarios
  sudo apt install php libapache2-mod-php php-   mysql -y

  ![image](https://github.com/user-attachments/assets/98a54333-7f0d-4ca1-a9f8-7f86d6aeb53a)

  
- Habilito el módulo PHP en Apache:
  sudo a2enmod php
  sudo systemctl restart apache2

  ![image](https://github.com/user-attachments/assets/32238020-60ec-4034-9be2-9f2ab60e7af3)

-- Instalo y configuro WordPress
- Instalo MySQL y configuro la base de datos:
sudo apt install mysql-server -y
sudo mysql_secure_installation

![image](https://github.com/user-attachments/assets/96e5acd8-3f1e-43e3-8fab-62220b1289c9)


- Luego, crearé una base de datos para WordPress:
sudo mysql -u root -p
CREATE DATABASE wordpress;
CREATE USER 'wordpressuser'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpressuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;

![image](https://github.com/user-attachments/assets/07d208ac-5bb5-4079-a279-6f16d50d1abf)


- Descargo y configuro WordPress:
wget https://wordpress.org/latest.tar.gz
tar -xzvf latest.tar.gz
sudo mv wordpress /var/www/centro.intranet
sudo chown -R www-data:www-data /var/www/centro.intranet
sudo chmod -R 755 /var/www/centro.intranet

![image](https://github.com/user-attachments/assets/7a7ccd63-6bcd-4c1e-b86b-4c35e822567a)

- Configuro un VirtualHost para WordPress:
sudo nano /etc/apache2/sites-available/centro.intranet.conf

Contenido del archivo apache:
<VirtualHost *:80>
    ServerName centro.intranet
    DocumentRoot /var/www/centro.intranet
    <Directory /var/www/centro.intranet>
        AllowOverride All
    </Directory>
</VirtualHost>
- Habilito el sitio:
sudo a2ensite centro.intranet.conf
sudo systemctl reload apache2

![image](https://github.com/user-attachments/assets/c9d39ba5-5fb3-40cd-8fa0-c4554d7030b3)

- Habilito el sitio:
  sudo a2ensite centro.intranet.conf
  sudo systemctl reload apache2

  ![image](https://github.com/user-attachments/assets/18a7f590-e479-448c-9cf8-20e4edbb6688)

-- Activo módulo wsgi para Python
- Instalo el módulo mod_wsgi:
sudo apt install libapache2-mod-wsgi-py3 -y
sudo a2enmod wsgi
sudo systemctl restart apache2

![image](https://github.com/user-attachments/assets/28ff999b-2522-41e8-857e-6969557dfaa7)

- Creo y despliego una aplicación Python: Creo una carpeta para la aplicación:
sudo mkdir /var/www/departamentos
sudo nano /var/www/departamentos/app.wsgi
Contenido del archivo python:
def application(environ, start_response):
    status = '200 OK'
    output = b'Hello, Python application is running!'
    response_headers = [('Content-type', 'text/plain'),
                        ('Content-Length', str(len(output)))]
    start_response(status, response_headers)
    return [output]

![image](https://github.com/user-attachments/assets/09fac413-f183-4ad5-ae41-3fb885cf1a77)

- Configuro el VirtualHost para Python:
sudo nano /etc/apache2/sites-available/departamentos.centro.intranet.conf
Contenido del archivo apache:
<VirtualHost *:80>
    ServerName departamentos.centro.intranet
    WSGIScriptAlias / /var/www/departamentos/app.wsgi
    <Directory /var/www/departamentos>
        Require all granted
    </Directory>
</VirtualHost>

  ![image](https://github.com/user-attachments/assets/22a80e9b-170b-45cd-91fb-b18678cb93d9)

- Habilito el sitio:
  sudo a2ensite     
  departamentos.centro.intranet.conf
  sudo systemctl reload apache2

  ![image](https://github.com/user-attachments/assets/40b83851-4b51-4145-9dcb-68c4aa5c6f95)

-- Protección de la aplicación Python con autenticación
- Habilito autenticación básica:
sudo apt install apache2-utils -y
sudo htpasswd -c /etc/apache2/.htpasswd user1

![image](https://github.com/user-attachments/assets/5e13aede-1dc3-4be5-a549-9166dc611ab5)

- Modifico la configuración del VirtualHost:
<Directory /var/www/departamentos>
    Require valid-user
    AuthType Basic
    AuthName "Restricted Access"
    AuthUserFile /etc/apache2/.htpasswd
</Directory>

![image](https://github.com/user-attachments/assets/2ca7f91d-8a94-4d23-9b7b-5b78047bc2d4)

-- Instalo y configuro Awstats
- Instalo Awstats:
sudo apt install awstats -y

![image](https://github.com/user-attachments/assets/c4c0521d-e272-4072-a3d8-c7a630426137)

- Configuro Awstats:
sudo nano /etc/awstats/awstats.conf

![image](https://github.com/user-attachments/assets/38e5bae7-e10c-4ccf-bbb4-e7c39cb57cbb)

-- Instalo segundo servidor web
- Instalo Nginx:
sudo apt install nginx php-fpm -y

![image](https://github.com/user-attachments/assets/ab92f134-114b-414c-95f4-b96b094f3c54)

- Configuro Nginx para el dominio:
sudo nano /etc/nginx/sites-available/servidor2.centro.intranet
- Contenido:
server {
    listen 8080;
    server_name servidor2.centro.intranet;
    root /var/www/servidor2;

    index index.php index.html;

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php-fpm.sock;
    }

}

![image](https://github.com/user-attachments/assets/7dea70dc-1ec5-46d6-b382-0b4b39d9a578)

- Creo el directorio raíz:
sudo mkdir /var/www/servidor2
sudo chown -R www-data:www-data /var/www/servidor2

![image](https://github.com/user-attachments/assets/0865f1eb-320e-43a3-81de-b195573ae9b3)

- Habilito configuración en Nginx:
sudo ln -s /etc/nginx/sites-available/servidor2.centro.intranet /etc/nginx/sites-enabled/
sudo systemctl restart nginx


- Instalo phpMyAdmin:
sudo apt install phpmyadmin -y
![image](https://github.com/user-attachments/assets/aa268f69-727f-473d-b04c-8fe13fc6d2bc)
![image](https://github.com/user-attachments/assets/faf0eac6-48a9-4cf8-9e50-47197e95bdfd)



