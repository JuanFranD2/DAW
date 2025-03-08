Para instalar Docker en Ubuntu debemos seguir los siguientes pasos:

- Actualizamos el sistema:
-- sudo apt update && sudo apt upgrade -y

![image](https://github.com/user-attachments/assets/71675186-52d0-49d6-867d-debedf97d710)
  
- Instala certificados y herramienta de transferencia de datos:
-- sudo apt-get install ca-certificates curl

![image](https://github.com/user-attachments/assets/49cd2f74-08ff-41b3-8b4c-cde3ca474748)

- Crea un directorio seguro para llaves de repositorios APT:
-- sudo install -m 0755 -d /etc/apt/keyrings

![image](https://github.com/user-attachments/assets/80374666-6a0c-49a9-ab87-a4d241428a22)

  - Descarga y guarda la clave GPG de Docker en el sistema:
-- sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc

![image](https://github.com/user-attachments/assets/ebabfc38-2049-49b6-b131-3d78944bb79c)
  
- Otorgar permisos de lectura a todos los usuarios para la clave GPG de Docker:
-- sudo chmod a+r /etc/apt/keyrings/docker.asc

![image](https://github.com/user-attachments/assets/3b970755-6f21-478e-8c48-23f3b0d41370)

- Agregar el repositorio de Docker a las fuentes de Apt y actualizar la lista de paquetes:
-- echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

![image](https://github.com/user-attachments/assets/47457c36-8a98-4298-9e94-18cedae96fcc)

- Instalar Docker Engine, CLI, Containerd, Buildx y Compose plugins:
-- sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

![image](https://github.com/user-attachments/assets/cb74bd1f-c8ea-4496-94a4-39a4ece4eed3)
  
- Verifica que Docker está instalado correctamente ejecutando:
-- sudo docker run hello-world

![image](https://github.com/user-attachments/assets/90f95822-de7f-4776-b793-923ffa996059)


