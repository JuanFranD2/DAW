Ejemplo 1: Despliegue de la aplicación guestbook
En este ejemplo vamos a desplegar con Docker Compose la aplicación guestbook, que estudiamos en el módulo de redes: 

Puedes encontrar el fichero docker-compose.yaml en en este directorio del repositorio.

En el fichero docker-compose.yaml vamos a definir el escenario. El comando docker compose se debe ejecutar en el directorio donde este ese fichero.

version: '3.1'
services:
  app:
    container_name: guestbook
    image: iesgn/guestbook
    restart: always
    environment:
      REDIS_SERVER: redis
    ports:
      - 8080:5000
  db:
    container_name: redis
    image: redis
    restart: always
    command: redis-server --appendonly yes
    volumes:
      - redis:/data
volumes:
  redis:
Veamos algunas observaciones:

Aunque ya sabemos que la variable de entorno REDIS_SERVER tiene el valor redis por defecto, la hemos indicado indicando el nombre del contenedor redis.
Podríamos haber usado también el nombre del servicio, es decir, REDIS_SERVER: db, ya que, como hemos comentado, la resolución se puede hacer usando el nombre del contenedor o el nombre del servicio.
Como vimos en el ejemplo del módulo 3, al crear el contenedor tenemos que ejecutar el comando redis-server --appendonly yes para que redis guarde la información de la base de datos en el directorio /datos. Para indicar el comando que hay que ejecutar al crear el contenedor usamos el parámetro command.
Por último indicar que hemos uso un volumen docker llamado redis para guardar la información de la base de datos (en el módulo3 usamos un bind mount).
Para crear el escenario:

$ docker compose up -d
[+] Running 4/4
 ✔ Network guestbook_default  Created                                                            0.3s 
 ✔ Volume "guestbook_redis"   Created                                                            0.0s 
 ✔ Container redis            Started                                                            0.5s 
 ✔ Container guestbook        Started                                                            0.5s

![image](https://github.com/user-attachments/assets/6ee131bf-77d8-45ab-9798-4a47b230e6c7)
 
Para listar los contenedores:
$ docker compose ps

![image](https://github.com/user-attachments/assets/d9471b40-9f52-4fe2-8810-12cf8a2626dc)

NAME        IMAGE             COMMAND                                                SERVICE   CREATED          STATUS          PORTS
guestbook   iesgn/guestbook   "python3 app.py"                                       app       18 seconds ago   Up 16 seconds   0.0.0.0:8080->5000/tcp, :::8080->5000/tcp
redis       redis             "docker-entrypoint.sh redis-server --appendonly yes"   db        18 seconds ago   Up 16 seconds   6379/tcp

Para parar los contenedores:
$ docker compose stop

![image](https://github.com/user-attachments/assets/7ee3cef0-825a-40f5-803c-aa5134393ec0)

[+] Stopping 2/2
 ✔ Container guestbook  Stopped                                                                  0.8s 
 ✔ Container redis      Stopped                                                                  0.8s 
Para eliminar el escenario:

$ docker compose down
[+] Running 3/3
 ✔ Container redis            Removed                                                            0.0s 
 ✔ Container guestbook        Removed                                                            0.0s 
 ✔ Network guestbook_default  Removed                                                            0.3s 

![image](https://github.com/user-attachments/assets/0eb4981e-5b52-4951-81c2-aeff90fd64f9)

Recuerda que para eliminar también el volumen usaremos docker compose down -v.

Ejemplo 2: Despliegue de la aplicación Temperaturas
En este ejemplo vamos a desplegar con Docker Compose la aplicación Temperaturas, que estudiamos en el módulo de redes: Ejemplo 2: Despliegue de la aplicación Temperaturas.

Puedes encontrar el fichero docker-compose.yaml en en este directorio del repositorio.

En este caso el fichero docker-compose.yaml puede tener esta forma:

version: '3.1'
services:
  frontend:
    container_name: temperaturas-frontend
    image: iesgn/temperaturas_frontend
    restart: always
    ports:
      - 8081:3000
    environment:
      TEMP_SERVER: temperaturas-backend:5000
    depends_on:
      - backend
  backend:
    container_name: temperaturas-backend
    image: iesgn/temperaturas_backend
    restart: always
Como hicimos en el ejemplo anterior, aunque no es necesario porque es valor por defecto, declaramos la variable de entorno TEMP_SERVER: temperaturas-backend:5000. Como indicábamos también, podríamos uso del nombre del servicio, de esta manera quedaría como TTEMP_SERVER: backend:5000.

Para crear el escenario:
$ docker compose up -d

![image](https://github.com/user-attachments/assets/b5306ed6-7657-4be1-9fe4-00fce0e63d86)

[+] Running 3/3
 ✔ Network temperaturas_default     Created                                                      0.3s 
 ✔ Container temperaturas-backend   Started                                                      0.2s 
 ✔ Container temperaturas-frontend  Started                                                      0.2s 
Para listar los contenedores:

$ docker compose ps

![image](https://github.com/user-attachments/assets/7d33c2c5-c294-4b28-b90f-940614dc1894)

NAME                    IMAGE                         COMMAND            SERVICE    CREATED          STATUS          PORTS
temperaturas-backend    iesgn/temperaturas_backend    "python3 app.py"   backend    20 seconds ago   Up 18 seconds   5000/tcp
temperaturas-frontend   iesgn/temperaturas_frontend   "python3 app.py"   frontend   20 seconds ago   Up 17 seconds   0.0.0.0:8081->3000/tcp, :::8081->3000/tcp

