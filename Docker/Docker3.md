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

![image](https://github.com/user-attachments/assets/864d1437-845e-45d4-97f8-d982391dc462)
  
- Contenedor 2: "myhello2"
-- docker run --name myhello2 hello-world

![image](https://github.com/user-attachments/assets/da2d6fe1-2e21-425e-9a5b-88e73cd93859)

- Contenedor 3: "myhello3"
-- docker run --name myhello3 hello-world

![image](https://github.com/user-attachments/assets/9e5cfb69-5afc-468a-886f-7f8da21bb94a)

- Mostramos Contenedores en Ejecución
Para ver los contenedores actualmente en ejecución:

-- docker ps

![image](https://github.com/user-attachments/assets/be678e08-4463-4ee6-ad14-945e4358cb4c)

Puede que no veamos los contenedores hello-world ya que terminan su ejecución inmediatamente después de imprimir su mensaje.

- Detener Contenedores Específicos
Detengamos dos de los contenedores que ejecutamos:

Detener "myhello1"
-- docker stop myhello1

![image](https://github.com/user-attachments/assets/ecf5053b-4e93-4347-afb2-7ac0f0c3c80c)

Detener "myhello2"
-- docker stop myhello2

![image](https://github.com/user-attachments/assets/e9dd8875-4548-4c5f-be74-08552b8df7a4)

- Borrar un Contenedor Específico
Borremos el contenedor "myhello1":
-- docker rm myhello1

![image](https://github.com/user-attachments/assets/68f7e9fe-fdb7-45be-b573-46819ac9f546)
  
- Mostrar Contenedores (Incluidos los Detenidos)
Para ver todos los contenedores, incluidos los que no están en ejecución:
-- docker ps -a

![image](https://github.com/user-attachments/assets/a6d48cd2-c06c-4f7e-9e19-27a5b4003f54)
  
- Borrar Todos los Contenedores
Primero, necesitas detener todos los contenedores en ejecución, si los hay, y luego puedes borrarlos:

-- docker stop $(docker ps -aq)
-- docker rm $(docker ps -aq)

![image](https://github.com/user-attachments/assets/83673e6f-86e7-4bf5-940e-7082829c10d0)

