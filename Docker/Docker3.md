Para llevar a cabo la Práctica 3 de Docker, seguiré las instrucciones proporcionadas y te guiaré paso a paso:

- Descargamos las imágenes de Docker

Ubuntu
-- docker pull ubuntu

Hello-world
-- docker pull hello-world

Nginx
-- docker pull nginx

![image](https://github.com/user-attachments/assets/c1405660-c0e2-414c-9593-9b82b86b580f)

- Listar Todas las Imágenes Docker
Para ver todas las imágenes descargadas:

-- docker images

![image](https://github.com/user-attachments/assets/d7ce96e1-da85-45e9-9261-1a7dba80df08)

- Ejecutar Contenedores con Nombres Específicos
Vamos a ejecutar tres contenedores del tipo hello-world y asignarles nombres específicos:

- Contenedor 1: "myhello1"
-- docker run --name myhello1 hello-world
  
- Contenedor 2: "myhello2"
-- docker run --name myhello2 hello-world
  
- Contenedor 3: "myhello3"
-- docker run --name myhello3 hello-world

- Mostrar Contenedores en Ejecución
Para ver los contenedores actualmente en ejecución:

-- docker ps



Puede que no veas los contenedores hello-world ya que terminan su ejecución inmediatamente después de imprimir su mensaje.

- Detener Contenedores Específicos
Detengamos dos de los contenedores que ejecutamos:

Detener "myhello1"
-- docker stop myhello1



Detener "myhello2"
-- docker stop myhello2



- Borrar un Contenedor Específico
Borremos el contenedor "myhello1":
-- docker rm myhello1


  
- Mostrar Contenedores (Incluidos los Detenidos)
Para ver todos los contenedores, incluidos los que no están en ejecución:
-- docker ps -a


  
8. Borrar Todos los Contenedores
Primero, necesitas detener todos los contenedores en ejecución, si los hay, y luego puedes borrarlos:

docker stop $(docker ps -aq)
docker rm $(docker ps -aq)


