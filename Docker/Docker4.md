## Ejemplo: montando directorios usando bind mount

En este caso vamos a crear un directorio en el sistema de archivo del host, donde vamos a crear un fichero `index.html`:

```bash
$ mkdir web
$ cd web
/web$ echo "<h1>Hola</h1>" > index.html
```

![image](https://github.com/user-attachments/assets/e9e72fd4-04f6-4a84-a660-f6279e0ff477)

Y podemos montar ese directorio en un contenedor, en este caso usamos la opción `-v`:

```bash
$ docker run -d --name my-apache-app -v /home/usuario/web:/usr/local/apache2/htdocs -p 8080:80 httpd:2.4
8de025f6ff4d4b8a5a57d10a9cbb283b103209f358c43148a4716a33a404e208
```

![image](https://github.com/user-attachments/assets/9668dde2-9744-46d6-9f61-a09c5c68adfe)

Y comprobamos que realmente estamos sirviendo el fichero que tenemos en el directorio que hemos creado.

```bash
$ curl http://localhost:8080
<h1>Hola</h1>
```

![image](https://github.com/user-attachments/assets/c3dee116-8c83-4565-8c1c-d3cd6f00941f)

Eliminamos el contenedor y volvemos a crear otro con el directorio montado:

```bash
$ docker rm -f my-apache-app 
my-apache-app

$ docker run -d --name my-apache-app -v /home/usuario/web:/usr/local/apache2/htdocs -p 8080:80 httpd:2.4
1751b04b0548217d7faa628fd69c10e84c695b0e5cc33b482df2c04a6af83292

$ curl http://localhost:8080
<h1>Hola</h1>
```

![image](https://github.com/user-attachments/assets/be772f81-5324-4528-94b2-b0a1497967fb)

Además podemos comprobar que podemos modificar el contenido del fichero aunque este montado en el contenedor:

```bash
$ echo "<h1>Adios</h1>" > web/index.html 
$ curl http://localhost:8080
<h1>Adios</h1>
```

![image](https://github.com/user-attachments/assets/74751fec-ae7f-4dde-b217-de8de7fbf316)

Por último, indicar que si nuestra carpeta origen no existe y hacemos un bind mount con `-v`, esa carpeta se creará pero lo que tendremos en el contenedor es una carpeta vacía. 

## Ejemplo usando volúmenes docker

Lo primero que vamos a hacer es crear un volumen docker:

```bash
$ docker volume create miweb
miweb
```

![image](https://github.com/user-attachments/assets/078cf330-0184-4a5e-9419-cc74f9442e5f)

A continuación creamos un contenedor con el volumen asociado, usando `--mount`, y creamos un fichero `index.html`:

```bash
$ docker run -d --name my-apache-app -v miweb:/usr/local/apache2/htdocs -p 8080:80 httpd:2.4
b51f89eb21701362279489c5b52a06b1a44c10194c00291de895b404ab347b80

$ docker exec my-apache-app bash -c 'echo "<h1>Hola</h1>" > /usr/local/apache2/htdocs/index.html'

$ curl http://localhost:8080
<h1>Hola</h1>

$ docker rm -f my-apache-app 
my-apache-app
```

![image](https://github.com/user-attachments/assets/b07cd890-d826-457d-94d1-3f091d1639e3)

Después de borrar el contenedor, volvemos a crear otro contenedor con el mismo volumen asociado:

```bash
$ docker run -d --name my-apache-app -v miweb:/usr/local/apache2/htdocs -p 8080:80 httpd:2.4
baa3511ca2227e30d90fa2b4b225e209889be4badff583ce58ac1feaa73d5d77
```

![image](https://github.com/user-attachments/assets/4107824f-2df0-428d-8ec4-d9892217c6db)

Y podemos comprobar que no no se ha perdido la información (el fichero `index.html`):

```bash
$ curl http://localhost:8080
<h1>Hola</h1>
```

![image](https://github.com/user-attachments/assets/d459bdac-da90-4190-8f16-ddf5ab760d8c)

Algunas aclaraciones:

* Al no indicar el volumen, se creará un nuevo volumen.
* Si usamos el flag `-v` e indicamos un nombre, se creará un volumen docker nuevo.
* Al usar tanto volúmenes como bind mount, el contenido de lo que tenemos sobreescribirá la carpeta destino en el sistema de ficheros del contenedor en caso de que exista.


